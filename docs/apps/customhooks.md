Life cycles hooks are the script used to start / stop / monitor your apps by Brain-Life. By default, it looks for executable installed on each resource in the PATH with named `start`, `status`, and `stop`. Resource owner needs to make sure these scripts are installed and accessible by your apps. 

> For most PBS, SLURM, and vanila VM, resource owner can install [ABCD default hooks](https://github.com/brain-life/abcd-spec/tree/master/hooks).

By default, `start` hook should look for a file named `main` to start your app. Therefore, the only file required to make your app runnable by Brain-Life is this `main` executable on the root directory of the app's git repository. 

Under most circumstances, app developers shouldn't have to worry about these hook scripts. However, if your app requires some special mechanism to start / stop and monitor your app, you might need to provide your own hook scripts. 

You can specify the paths to these hook scripts by creating a file named `package.json`

```json
{
  "abcd": {
    "start": "./start.sh",
    "stop": "./stop.sh",
    "status": "./status.sh"
  }
}
```

Then, you will need to provide those hook scripts as part of your app.

> Please be sure to `chmod +x *.sh` so that your hook scripts are executable.

### `start.sh`

Following is an example for `start` script. It submits a file named *main* (should be provided by each app) through qsub. It stores `jobid` so that we can monitor the job status.

```
#!/bin/bash

#return code 0 = job started successfully.
#return code non-0 = job failed to start

qsub -d $PWD -V -o \$PBS_JOBID.log -e \$PBS_JOBID.err main > jobid
```

### `stop.sh`

Following is an example for `stop` script. This scripts reads the jobid created by `start` script and call qdel to stop it.

```
#!/bin/bash
qdel `cat jobid`
```

### `status.sh`

status hook is a bit more complicated. It needs to return various exit codes based on the status of the app. It uses the `jobid` stored by start script to query the job status with `qstat` PBS command. 

Anything you output to stdout will be used to set task's status message. For example, you can output the last line from the log file to relay the last log entry to the users on Brain-Life.

```
#!/bin/bash

#return code 0 = running
#return code 1 = finished successfully
#return code 2 = failed
#return code 3 = unknown (retry later)

if [ ! -f jobid ];then
    echo "no jobid - not yet submitted?"
    exit 1
fi

jobid=`cat jobid`
if [ -z $jobid ]; then
    echo "jobid is empty.. failed to submit?"
    exit 3
fi

jobstate=`qstat -f $jobid | grep job_state | cut -b17`
if [ -z $jobstate ]; then
    echo "Job removed before completing - maybe timed out?"
    exit 2
fi

case "$jobstate" in
Q)
    showstart $jobid | grep start
    exit 0
    ;;
R)
    #get last line of last log touched
    logfile=$(ls -rt *.log | tail -1)
    tail -1 $logfile
    exit 0
    ;;
H)
    echo "Job held.. waiting"
    exit 0
    ;;
C)
    exit_status=`qstat -f $jobid | grep exit_status | cut -d'=' -f2 | xargs`
    if [ $exit_status -eq 0 ]; then
        echo "finished with code 0"
        exit 1
    else
        echo "finished with code $exit_status"
        exit 2
    fi
    ;;
*)
    echo "unknown job status $jobstate .. will check later"
    exit 3
    ;;

esac

```

