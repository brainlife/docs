!!! warning
    This is a draft. Comments are welcomed!

# Running App

Before submitting an App, you might want to know which Apps are available. The following command shows a list of Apps that takes anat/t1w data-object as an input.

```
$ bl app query --input-datatype neuro/anat/t1w

...

Id: 5ac01066029f78002be2c481
Name: ACPC alignment via ART
Type: (t1: neuro/anat/t1w) -> (t1: neuro/anat/t1w<acpc_aligned>)
Description: This app uses the Automatic Registration Toolbox (ART) to perform ACPC alignment of the T1 image. See https://www.nitrc.org/projects/art/ for more information.

...

(Returned 6 results)
```

We can run this app to align our t1 image to the ACPC axial plane. It then outputs a new data-object with datatype `neuro/anat/t1w` and a datatype tag `acpc_aligned`, signifying that the data has been acpc aligned.

To run an app, we need the app's id, the ids of the input or inputs we want to supply it with, the id of the project to save it to, and a config JSON string for any additional input parameters required.

```
$ bl app run                                      \
    --id 5ac01066029f78002be2c481                 \
    --input t1:5b031990251f5200274d9cc4           \
    --project 5afc2c8de68fc50028e90820            \
    --config '{"reorient" : true, "crop" : true}'

Data Staging Task Created, PROCESS:
SERVICE: soichih/sca-product-raw
STATUS: Waiting to be processed by task handler
(running since 5 seconds ago)
```

After staging has completed, you should see the following output in the terminal:

```
Data Staging Task Created, PROCESS:
SERVICE: soichih/sca-product-raw
STATUS: Successfully finished
(538 ms ago)
ACPC alignment via ART Task for app 'ACPC alignment via ART' has begun.
```

You can wait for the app to finish by..

```
$ bl app wait --id 5b031dacc1b8f90044ad6c3b
```

Then, after the app has finished (and the data-object has been stored), you can download the resulting data-object just by supplying the resulting data-object id. You can get the resulting data-object id by querying the list of data-objects (which will be sorted by date) and then running something like this:

```
$ bl data download --id 5afddb42251f5200274d9ca1

## Bash Script

Here is a sample bash script to run app-pRF by first uploading a data object and submitting the app itself.

```bash
#!/bin/bash

# login (you only need to run this once every 30 days) 
bl login --ttl 30
# change --ttl (time-to-live) value to a desired days you'd like to keep your token alive 

# upload input

# Let's assume you have following files
# /somewhere/stimulus/stim.nii.gz
# /somewhere/task/bold.nii.gz

bl data upload --datatype 5afc7c555858d874a40c6dda --project 5afc2c8de68fc50028e90820 --subject "soichi1" --json /somewhere/stimulus 2>> upload.err | jq -r '._id' > stim.id
bl data upload --datatype 59b685a08e5d38b0b331ddc5 --project 5afc2c8de68fc50028e90820 --subject "soichi1" --datatype_tag "prf" --json /somewhere/task 2>> upload.err | jq -r '._id' > func.id

# For -d (datatype) ID, you can query it by `bl datatype query -q stimulus` or `bl datatype query -q func`
# For -p (project) ID, you can query project by `bl project query -q "project name"`

# If you have func/task sidecard file, you can store them in a json file (like "metadata.json") and load them with your data by adding `--meta metadata.json` to the upload command.

# Running the App!
bl app run --id 5b084f4d9f3e2c0028ab45e4 --project 5afc2c8de68fc50028e90820 --input tractogram_static:$(cat func.id) --input stimimage:$(cat stim.id) --config '{"frameperiod": "1.3"}' --json | jq -r '._id' > task.id

# You can query app by `bl app query -q "prf"`

# --config is where you pass JSON object containing config for your App. 

# Waiting for the App to finish (and archive output data)
bl app wait $(cat task.id)
if [ ! $? -eq 0 ];
   echo "app failed"
   exit 1
fi
echo "finished!"

# Download the output data
bl data download -i  
for id in $(bl data query --taskid $taskid --json | jq -r ".[]._id"); do
    echo "downloading data $id"
    bl data download $id
done

# Now you should see directories containing each output data with the data-object ID as a directory name.

```


