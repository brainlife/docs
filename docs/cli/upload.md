!!! warning
    This is a draft. Comments are welcomed!

# Uploading Data

In order to upload a data object, brainlife requires that you supply a project and datatype associated with it. For example, the data we will be uploading–a t1 weighted image–must contain a file called 't1.nii.gz' in order to be successfully stored.

Before following along with this readme, you will need to make your own project on [brainlife.io](https://brainlife.io).

Have you made one? Then let's begin:

## Querying the Project

To search through the list of projects, we can use `bl project query`

My username is `stevengeeky`, so I'll query all of the projects I'm the admin of:

```
$ bl project query --admin stevengeeky
Id: 5afc2c8de68fc50028e90820
Name: Test Project
Admins: stevengeeky
Members: stevengeeky
Guests:
Access: Access: private (but listed for all users)
Description: test

(Returned 1 result)
```

Keep the id in mind for later. To specifically extrapolate the id from the query, install `jq` and run something similar to the following (using the previous query as an example):

```
$ bl project query --admin stevengeeky --json | jq -r '.[0]._id'
```

Which returns `5afc2c8de68fc50028e90820`.

`--json` instructs the CLI to output results in JSON format so that jq can parse the output.

Also, if you don't know what to run or how to run something, simply attach --help to the end of any command:

```
$ bl project query --help

  Usage: bl-project-query [options]

  Options:

    --id <id>           filter projects by id
    --query <query>   filter projects by name or description
    --admin <admin>     filter project by admins in it
    --member <members>  filter project by members in it
    --guest <guests>    filter project by guests in it
    --skip <skip>       number of results to skip
    --limit <limit>     maximum number of results to show
    --json              output data in json format
    -h, --help          output usage information
```

## Querying Datatype

Now, we need a datatype for our data. Since I will be uploading a t1 weighted image, I'll query for the list of datatypes which might match what I want:

```
$ bl datatype query --query t1
Id: 58c33bcee13a50849b25879a
Name: neuro/anat/t1w
Description: T1 Weighted
Files: [(required) t1: t1.nii.gz]

(Returned 1 result)
```

The single result returned happens to be exactly what we want. This will be the datatype for our uploaded data.

## Uploading Dataset

Now, it's time to actually upload our data. I have a file named `t1.nii.gz` in a directory called `t1/`, and to upload it I need to supply a few things. Let's run `bl data upload --help` to figure out what those things are:

```
$ bl data upload --help

  Usage: bl-data-upload [options]

  Options:

    --directory <directory>          directory where your data is located
    --project <projectid>            project id to upload data to
    --datatype <datatype>            datatype of uploaded data
    --datatype_tag <datatype_tags>   datatype_tag of uploaded data
    --desc <description>             description of uploaded data
    --subject <subject>              subject of uploaded data
    --session <session>              session of uploaded data
    --tags <tags>                    tags of uploaded data
    --meta <metadata>                name of file containing metadata (JSON) of uploaded data
    -h, --help                       output usage information
```

All that is required to upload a data is a directory to upload, a project id, and a datatype id. But you can also supply other options to provide additional information about it, such as applicable datatype tags, search tags, and which session and subject the data is from.

I will upload my data by using the following command, given the id of my project and datatype of my data:

```
$ bl data upload \
    --project 5afc2c8de68fc50028e90820   \
    --datatype neuro/anat/t1w \
    --desc 'My t1 weighted image' \
    --subject 12345 \
    --session 1 \
    --t1 t1/t1.nii.gz \
    --meta t1/t1.json \
    --tag "t1" \
    --tag "image"
```

Notice that I supplied `--tag` twice to add more than one search tag to my data. This works the same way with `datatype_tags`. --meta is optional but you should point to any "sidecar" json if you have one

You can upload data by specifying a single directory containing all of the files for its associated datatype (using `--directory`). However, you can also specify the path for each individual file id, as is done above (`--t1 t1/t1.nii.gz`, where `--t1` is the file id and `t1/t1.nii.gz` is the file to upload).

```
Looking for /path/to/t1/t1.nii.gz
Waiting for upload task to be ready...
SERVICE: soichih/sca-service-noop    Brain Life
STATUS: Synchronously running service
(running since 13 seconds ago)
```

You can see the process update in real time. When it has finally finished it should output:

```
Dataset successfully uploaded!
Now registering data ...
Finished data-object registration!
```

## Querying Datasets

Now that we've uploaded a data, we can view it by querying the list of data:

```
$ bl data query --subject 12345

Id: 5b031990251f5200274d9cc4
Project: Test Project
Admins: stevengeeky
Members: stevengeeky
Guests:
Subject: 12345
Session: 1
Datatype: neuro/anat/t1w
Description: My t1 weighted image
Create Date: 5/21/2018, 3:10:08 PM (2 minutes ago)
Storage: jetstream
Status: stored

(Returned 1 result)
```

## Bulk Upload

Say we have all files from several subejcts in a single folder (the current folder) with two datafiles, DWI (dffusion-weighted MRI data files) and T1W (t1 weighted MRI, anatomical data files). Say that each subject is either a control (CNTR) or a patient (PTNT).

We will first login on brainlife. Then set the current proejct variable (project IDs are the hash-numbers indicated on each project web-address on brainlife.io)

We will match the DWI files to the correspondign brainlife.io datatype, which is neuro/dwi. We will also match the T1W files to the corresponding brainlife.io datatype which is neuro/anat/t1w.

We will automatically find the total number of subejcts and for each subject we will push on the brainlife.io platform the T1W and the DWI and match them with the corresponding datatype. The CLI command will run the brainlife.io datatype validator and upload the file, whch will magically populate the project online.

A bash script would would look like the following:

```bash
#!/bin/bash
bl login

project=<project id string of numbers>

for type in CTRL PTNT; do
    for t1 in $(ls $type_*_T1W.nii.gz); do
        subject=${type}_${t1:4:3}
        bl data upload --project $project --datatype neuro/dwi --subject $subject --dwi ${subject}_DWI.nii.gz --bvecs ${subject}.bvec --bvals ${subject}.bval
        bl data upload --project $project --datatype neuro/anat/t1w --subject $subject --t1 $t1
    done
done
```

!!! note
    You should add --meta if you have any sidecard to go with each dwi/t1

## BIDS Upload

If you have data organized in a BIDS format, you can upload the entire dataset with a single command.

```
bl bids upload --tag test --project 5b031990251f5200274d9cc4 -d my_bids_dir
```


