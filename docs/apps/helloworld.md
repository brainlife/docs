> Please read [App Developers / Introduction](/apps/introduction.md) first. 

# HelloWorld

Here, we will create a "HelloWorld" Brainlife App. 

Let's begin by creating a brand new [github repository](https://help.github.com/articles/creating-a-new-repository/). Please be sure to make the repo public so that Brainlife can access it. You can name it whatever you like (like "app-helloworld").

Git clone your new repository to wherever you will be developing/editing and testing your App.

```
git clone git@github.com:username/app-helloworld.git
```

Now, create a file called `main`.

#### main

```bash
#!/bin/bash

#PBS -l nodes=1:ppn=1
#PBS -l walltime=00:05:00

#parse config.json for input parameters (here, we are pulling "t1")
t1=$(jq -r .t1 config.json)
./main.py $t1
```

Please be sure to set the executable bit.

```bash
chmod +x main
```

!!! hint
    [`jq`](https://stedolan.github.io/jq/) is a command line tool used to parse a small JSON file and pull values out of it. You can install it on your machine by running something like `apt-get install jq` or `yum install jq` depending on your OS/distribution. All Brainlife resources should have common binaries installed including `bash`, `jq`, and `singularity`.

The first few lines in our `main` instructs PBS or Slurm batch systems to request a certain number of nodes/processes to our App. 

```bash
#PBS -l nodes=1:ppn=1
#PBS -l walltime=00:05:00
```

You will receive all input parameters from Brainlife through a JSON file named `config.json` which is created by Brainlife when your App is executed. See [Example config.json](https://github.com/brain-life/app-dtiinit/blob/master/config.json.sample. As an App developer, you will define what parameters needs to be entered by the user and input datasets later when you register your App on Brainlife.

Following lines parses the `config.json` using `jq` and the value of `t1` to the main part of the application which we will create later.

```bash
#parse config.json for input parameters
t1=$(jq -r .t1 config.json)
./main.py $t1
```

To be able to test your application, let's create a test `config.json`.

#### config.json

```json
{
   "t1": "/somewhere/t1.nii.gz"
}
```

Please update the path to wherever you have your test `anat/t1w` input file. If you don't have any, you can download one from [Brainlife/O3D](https://brainlife.io/pub/5a0f0fad2c214c9ba8624376) publication page. Just click the Datasets tab, and select any `anat/t1w` data to download.

You should add `config.json` to .gitignore as `config.json` is created at runtime by Brainlife, and we just need this now to test your app. 

!!! hint
    A good pattern might be to create a file called `config.json.sample` used to test your App, and create a symlink `ln -s config.json config.json.sample` so that you can run your app using `config.json.sample` without including the actual `config.json` as part of your repo. This allows other users to construct their own `config.json` if they want to run your app via command line.

!!! note
    Instead of parsing `config.json` inside `main`, you could use other parsing library as part of your algorithm itself, like Python's `import json`, or Matlab's [jsonlab](https://github.com/fangq/jsonlab.git) module inside the actual program that `main` will be executing.

Our `main` script runs a python script called `main.py` so let's create it.

#### main.py

```
#!/usr/bin/env python

import sys
import nibabel as nib

#just dump input image header to output.txt
img=nib.load(sys.argv[1])
f=open("output.txt", "w")
f.write(str(img.header))
f.close()

```

Again, be sure to set the executable bit.

```
chmomd +x main.py
```

Any output files from your app should be written to the current working directory and in a file structure that complies with whichever the datatype of your dataset is. For now, we are not going to worry about the output datatype (assuming we will use `raw`)

Please be sure to add any output files from your app to .gitignore so that it won't be part of your git repo. 

#### .gitignore

```
config.json
output.txt
```

## Testing

Now, you should be able to test run your app locally by executing `main`

```
./main
```

<!--
!!! hint
    If you are testing on HPC clusters, be sure to enter the interactive shell session before running your `main` by executing something like `qsub -I`
-->

Now, it should generate an output file called `output.txt` containing the dump of all nifti headers.

```
<class 'nibabel.nifti1.Nifti1Header'> object, endian='<'
sizeof_hdr      : 348
data_type       : 
db_name         : 
extents         : 0
session_error   : 0
regular         : r
dim_info        : 0
dim             : [  3 260 311 260   1   1   1   1]
...
...
...
qoffset_x       : 90.0
qoffset_y       : -126.0
qoffset_z       : -72.0
srow_x          : [ -0.69999999   0.           0.          90.        ]
srow_y          : [   0.            0.69999999    0.         -126.        ]
srow_z          : [  0.           0.           0.69999999 -72.        ]
intent_name     : 
magic           : n+1
```

## Pushing to Github

If everything looks good, push our files to the Github.

```bash
git add .
git commit -m"created my first BL App!"
git push
```

Congratulations! We have just created our first Brainlife App. To summarize, we've done following.

* Created a new public Github repo.
* Created `main` which parses `config.json` and runs our App.
* Created a test `config.json`.
* Created `main.py` which runs our algorithm and generate output files.
* Tested the App, and pushed all files to Github.

!!! info
    You can see more concrete examples of Brainlife apps at [Brainlife hosted apps](https://github.com/search?q=org%3Abrain-life+app-).

To run your App on Brainlife, you will need to do following.

1. [Register your App on Brainlife.](/apps/register/)

2. Enable your App on at least one Brainlife compute resource. 

    For now, please email [mailto:brlife@iu.edu](brlife@iu.edu) to enable your App on our shared test resource.

<!--
All input parameters are assumed to be text (char). You need to write your functions that are going to be MATLAB compiled with all the arguments as text. Arguments passing a number need to be given as text and within the function converted to integers values (str2num(), etc.). 
-->

