Let's begin by creating a brand new [github repository](https://help.github.com/articles/creating-a-new-repository/). Please be sure to make the repository public so that Brain-Life can download your app.

Git clone your new repository to where you will be developing and running your app to test.

```
git clone git@github.com:soichih/app-test.git
```

Now, create a file named `main` which is used to run your app by Brain-Life. You can also run this file to test your application locally.

```
#!/bin/bash

#PBS -l nodes=1:ppn=1
#PBS -l walltime=00:05:00

#parse config.json for input parameters
t1=$(jq -r .t1 config.json)
./main.py $t1

```

Please be sure to set the executable bit.

```
chmod +x main
```

> [`jq`](https://stedolan.github.io/jq/) is a common command line tool used to parse a small JSON file and pull values out of it. You can install it by `apt-get install jq`. 


!!! note
    All Brainlife resource should at least have `jq` and `singularity` command installed. 

Following part in `main` instructs PBS (or slurm job manager) to request certain number of nodes / process-per-node. It's not necessary if you are not planning to run your app via PBS scheduler.

```
#PBS -l nodes=1:ppn=1
#PBS -l walltime=00:05:00
```

You can receive input parameters from Brainlife through a JSON file called `config.json` which is created by Brainlife when the app is executed. See [Example config.json](https://github.com/brain-life/app-dtiinit/blob/master/config.json.sample).

Following lines parses the `config.json` using `jq` and pass it to the main part of the application `main.py`.

```
#parse config.json for input parameters
t1=$(jq -r .t1 config.json)
./main.py $t1
```

To be able to test your application, let's create a test `config.json`.

```
{
   "t1": "/somewhere/t1.nii.gz"
}
```

You should add this file to .gitignore since it is only used locally on your machine to test your app.

!!! note
    Instead of using `jq` command to parse `config.json` in `main`, you can use python's json parsing library (`import json`) inside `main.py` if you prefer.

    Or..  for Matlab, you could use [jsonlab](https://github.com/fangq/jsonlab.git) module to parse the json.

Our `main` script runs a python script called `main.py`. Let's create it.

```
#!/usr/bin/env python

import sys
import dipy.tracking as dipytracking
dipytracking.bench()

t1=sys.argv[1]

#do something interesting with t1...
print(t1)
f = open("output.txt", "w")
f.write("hello world")
f.close()
```

!!! todo
    Please contribute to make this a bit more interesting hello world example!

Any output files from your app should be written to the current directory so that Brainlife can pick them up after your job is completed. For now, we are not going to worry about the output datatype (we are going to use `raw`)

Please be sure to add any output files from your app to .gitignore so that it won't be part of your git repo after you 

`.gitignore`

```
config.json
output.txt
```

Now, you should be able to test run your app simply by executing `main`

```
./main
```

<!--
!!! hint
    If you are testing on HPC clusters, be sure to enter the interactive shell session before running your `main` by executing something like `qsub -I`
-->

If your app outputs correct output file on the local directgory, your application is ready to be pushed to the github. 

```
git add .
git commit -m"initial import"
git push
```

Now you are ready to register your app on the Brianlife to run it through Brainlife!

To summarize, we've done following to create Brainlife app.

* Created a new repo on Github.
* Created `main` which runs the app.
* App reads input parameters from `config.json` created by Brainlife at runtime (we created a test `config.json`  to test our app).
* App writes output files to the current directory (and an expected format for each datatype - we will work on this later)

!!! info
    You can see more concrete examples of Brainlife apps at [Brainlife hosted apps](https://github.com/search?q=org%3Abrain-life+app-).

<!--
All input parameters are assumed to be text (char). You need to write your functions that are going to be MATLAB compiled with all the arguments as text. Arguments passing a number need to be given as text and within the function converted to integers values (str2num(), etc.). 
-->

