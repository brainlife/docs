# Introduction

## What is an *App*?

Brainlife Apps are snippets of code comprising a (short) series of processing steps within a larger data analysis workflow. Apps are meant to be reusable by other users and not just by the App developer. 

The App should not only be able to reproduce the results of the data processing done by the developer, but it should also work on new data provided by other users. The code in each App should use general tools and clarity in code writing so to make the App understandable by other users.

All Apps should ..

1. Be hosted on gitHub.com as public repositories. Apps can comprise any combination of MatLab, Python, or other types of code.
2. Have a single executable file named `main` in the root directory of the git repository. It will be executed on selected compute resources, or submitted via sbatch/qsub and other batch scheduling programs.
3. Read all input parameters from a `config.json` file. `config.json` is created by brainlife.io at runtime on the current working directory. 
3. Read all input data from paths specified in `config.json` (config.json contains both the input parameters; such as command line arguments as well as paths to data files; like t1.nii.gz)
4. Write all output files in the current directory (`./`), in a structure defined as a Brainlife [`datatype`](https://brainlife.io/datatypes){target=_blank}. More information about [Brainlife datatypes](/docs/user/datatypes/)
ter.

!!! note
    Brainlife Apps follow a technical specification called Application for Big Computational Data analysis or [ABCD](https://github.com/brainlife/abcd-spec){target=_blank}

App can run on many different environment / clusters. To normalize the execution environment, many Apps execute program using singularity on a specific container that comes with all dependencies and libraries. 

## Prerequisite

Before you start making an App for brainlife, we should be familar with the following general concepts.

1. You should be familar with at least 1 programming language (Matlad, Python, R, etc..)

2. You should be comfortable with bash terminal and some basic bash scripting. Please see [https://docs.microsoft.com/en-us/learn/modules/bash-introduction/](https://docs.microsoft.com/en-us/learn/modules/bash-introduction/){target=_blank}

3. You should know how to use [git](https://git-scm.com/docs/gittutorial){target=_blank} and [github](https://guides.github.com/){target=_blank} to publish your git repo.

4. Although you can develop and test your App on your local computer, brainlife.io will run your App mostly on various HPC resources. Therefore, you should be familar with basic concetps on HPC systems. Please see [Intro to HPC at IU (YouTube)](https://www.youtube.com/watch?v=atHXod7ZsfY){target=_blank}

5. We run our App through a docker container on singularity. You don't have to know how to create containers yourself (as we can help you with that) but it would be helpful for you know what container is and what singularity does. Please see [singularity introductuion](https://singularity-tutorial.github.io/00-introduction/){target=_blank}

## App Development Timeline

You would normally follow these steps to develop and register your App on Brainlife.

1. Develop an algorithm that runs on your laptop or local cluster with your test datasets.
2. Create a sample `config.json` file. 
5. Create `main` that parses `config.json` and passes it to your algorithm.
6. Publish it as public github repo.
7. Register your App on Brainlife. During this step, you can define what parameters and input file(s) should be made available to your App via `config.json`.
8. Contact resource administrators and ask them to enable your App (more below). 

!!! hint
    Creating an App for brainlife.io requires you to be familiar with wide range of techincal knowledges in areas including programming, unix/shell, git, HPC. 
    Please refer to [learn-neuroimaging.github.io](https://learn-neuroimaging.github.io/tutorials-and-resources/11-programming/){target=_blank} for a community curated list of online tutorials.

# Enabling an App on a compute resource

An App needs to be enabled on each compute resource to run. Each user will have a different set of resources that they have access to, but Brainlife provides default **shared** resources for all users. If you want any Brainlife user to be able to run your App, you can [contact the resource administrators](mailto:brlife@iu.edu) to enable your App.

You will need to discuss how to handle any dependencies/libraries that your App might require with resource administrators. To make things easier and reproducible, you should consider Dockerizing your App's **dependencies** (but not the App itself) so that you can run your App through your container using [singularity](https://singularity.lbl.gov/){target=_blank} from your `main`. 

!!! hint
    Most compute resources now provide singularity which increases the number of resources where you might be able to run your Apps.

## App Launch Sequence

Brainlife executes an App following these steps.

1. A user requests to run your App through Brainlife.
2. Brainlife queries a list of compute resources that user has access to and currently available to run your App. Brainlife then determines the best compute resource to run your App.
3. Brainlife stages input datasets out of Brainlife's datasets archive, or transfer any dependent task's work directory that are required to run your App.
4. Brainlife creates a new working directory by git cloning your App on (normally) a resource's scratch disk space, and place `config.json` containing user-specified configuration parameters and various paths the input files.
5. Brainlife then runs `start` hook installed on each compute resources as part of `abcd specification` (App developer should have to worry about this under the most circumstances).
6. On a PBS cluster, `start` hook then **qsub**s your `main` script and place it on the local batch scheduler queue.
7. Local job scheduler runs your `main` on a compute node and your App will execute your algorithm, and generate output on the working directory.
8. Brainlife periodically monitors your job and relay information back to the user.
9. Once the job is completed, the user archives an output dataset if the result is valid.

## Datatype

Different Brainlife Apps can exchange input/output datasets through Brainlife *datatypes* which are developer-defined file/directory structure that holds a specific set of data.

Here are some examples of currently registered datatypes.

* neuro/anat/t1w (t1.nii.gz)
* neuro/anat/t2w (t2.nii.gz) 
* neuro/dwi (diffusion data and bvecs/bvals)
* neuro/freesurfer (entire freesurfer output)
* neuro/tract (tract.tck containing fiber track data)
* neuro/dtiinit (dtiinit output - dti output directory)
* generic/images (a list of images)
* raw (unstructured data often used during development)

Your App should read from one or more of these datatypes and write output data in a format specified by another datatype. By identifying existing datatypes that you can interoperate you can interface with datasets generated by other Apps and your output can be used by other Apps as their input.

To create a new datatype, please visit [#datatype](https://brainlife.slack.com/archives/C946FA6PK){target=_blank} channel on brainlife.io slack.

_Please see [datatype](/docs/user/datatypes/) page for more detail.

!!! quote
    "Write programs that do one thing and do it well. Write programs to work together." - A Quarter Century of Unix by Doug McIlroy

Brainlife app should follow the [Do One Thing and Do It Well](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well){target=_blank} principle where a complex workflow should be split into several smaller Apps (but no more than necessary nor practical) to promote code-reuse and help parallelize your workflow and run each App on the most appropriate compute resource available.

!!! hint
    Before writing your apps, please browse [currently registered Brainlife Apps](https://brainlife.io/warehouse/#/apps){target=_blank} and datatypes under Brainlife.io to make sure you are not reinventing Apps. If you find an App that is similar to what you need, please contact the developer of the App and discuss if the feature you need can be added to the App.

## Job Complexiy / Resource Requirement

All App should have clearly defined upper bound (aka "Big-O") in terms of resource and computing time requirement. Most `main` script starts with batchscheduler resource directives that looks like this.

```
#!/bin/bash
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=2G         # memory per cpu-core (4G is default)
#SBATCH --time=00:01:00          # total run time limit (HH:MM:SS)
```

This informs the batch scheduler how long / how much of its computing capability is needed to run your App. Your job will be killed if you exceed what you specify in these directives.

When the job is executed on HPC system, the batch scheduler will allocate requested amount of time / resources for the job and it will consume SUs on those resources based on these parameters. It is important to keep the resource requirement as low as possible so that your job will not consume more SUs than necessary. You should measure the amount of CPU / memory resource typically consumed and set these parameter accordingly. If your job takea a long time to run, you will need to set the walltime requirement high enough to accomdate it. Not only the cost of running jobs increases as you increase the resource requirement, it will also increase the queue time of the job waiting for the block of requested resource to become available for your job.

!!! warning
    Please be conservative with amount of cpus-per-task (or ppn). Most batchscheduler will consume SUs based on number of CPU counts you request times the amount of time. For example, if you request a job to run for 2 hours with 16 cores, it will consume 2x16=32 units. If you only use 4 cores for your job, please set it to 4 so that it will only consume 2x4=8 units instead.

brainlife.io is used to not just reproduce the original results published by app developer, but to process user's own data. Your App should be able to handle variety of input data - not just the data that you have used to develop the App. This is a very difficult challenge and it usually takes time for an App to become robust. Often, user could submit your App with data that is much larger or more complex than they have intended. Sometimes, it is even impossible to finish the computation within specified amount of time. To handle such case, you could implement your App so that you can restart your App and resume processing from where it was terminated previously.

For example, if your App consists of running 3 separate steps, each taking an hour, then you can make each steps to first check to see if each step is already completed. If your job dies due to timeout, then user can simply rerun the same job and only run the remaining steps if your App is designed to handle rerunning. We recommend doing the following.

For example, you can first check to see if the output from each step exists, and if it does, skip it.

``` bash
echo "running process1"
if [ ! -f output1.data ];
    ./process1 -in <someinput> -out tmp.data
    mv tmp.data output1.data
else
    echo "output1.data already exists.. skipping this step"
fi

echo "running process2"
if [ ! -f output2.data ];
    ./process2 -in <someinput> -out tmp.data
    mv tmp.data output2.data
else
    echo "output2.data already exists.. skipping this step"
fi

#etc..

```

If the job completes step1 but not step2, user can simply rerun the job to see if it can complete the 2nd process.


