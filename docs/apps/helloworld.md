Let's begin by creating a brand [new github repository](https://help.github.com/articles/creating-a-new-repository/). Please be sure to make the repository public so that Brain-Life can download your app.

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

Following part in `main` instructs PBS (or slurm job manager) to request certain number of nodes / process-per-node. It's not necessary if you are not planning to run your app via PBS scheduler.

```
#PBS -l nodes=1:ppn=1
#PBS -l walltime=00:05:00
```

You can receive input parameters from Brain-Life through a JSON file called `config.json` which is created by Brain-Life when the app is executed through Brain-Life. 

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

> Instead of using `jq` command to parse `config.json` in `main`, you can use python's json parsing library inside `main.py` if you prefer.

Finally, the `main` runs a python script called `main.py`. Let's create it.

```
#!/usr/bin/env python

import sys
import dipy.tracking as dipytracking
dipytracking.bench()

t1=sys.argv[1]
print(t1)

f = open("output.txt", "w")
f.write("hello world")
f.close()
```

Any output files from your app should be written to the current directory, so that Brain-Life can pick them up. Please be sure to add any output files from your app to .gitignore so that it won't be part of your git repo.


`.gitignore`

```
output.txt
```


Now, you can test run your app simply by executing `main`

```
./main
...
```

> If you are testing on HPC clusters, be sure to enter the interactive shell session before running your `main` by executing something like `qsub -I`

If your app outputs correct output file on the local directgory, your application is ready to be pushed to the github. 

```
git add .
git commit -m"initial import"
git push
```

Now you are ready to register your application on the Brian-Life so that your user can start submitting your app!

To summarize, Brain-Life app has following characteristics.

* Published in Github.
* It has `main` which starts the app.
* It reads input parameters from `config.json` created by Brain-Life on the current directory.
* It writes output files on the current directory.

> For more examples on Brain-Life apps, please see [Brain-Life hosted apps](https://github.com/search?q=org%3Abrain-life+app-).


