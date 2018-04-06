
Docker is a software containerization tool that allow you to package your app and its dependencies into a portable "container". Once built, you can run it on any machine as long as it supports singularity regardless of the type of host OS, or host software installed.

Although you can create a fully functional standalone docker container for your app, normally we only need to containerize the application *dependendencies*. For Brainlife, we recommend not to include the main part of your app (your python or bash scripts that drives your application) on your container. Once you create container for you application *environment*, with singularity, your can use it to execute the most ideosyncratic parts of your app through it. 

You might also want to share the same *environment* container across multiple app that you want to run, or with other member of your lab who might be using a very similar set of software dependencies. 
!!! warning
	If you do share your *environment* container, be sure to specify which version(tag) of the container you are using in your app. Like..
	```
	singularity exec -e docker://brainlife/mrtrix_on_mcr:1.0 ./ensembletracking.sh
	```

	Without `1.0`, you won't know which version of the container you are executing which defeats the reproducibility of your app.

To build a docker container, you need to [install Docker engine on your laptop](https://docs.docker.com/machine/install-machine/) or find a server that has docker engine installed that you can use. (Contact [Soichi](hayashis@iu.edu) if you need a help.)

We assume you already have your Brainlife app hosted on Github, and you are making changes inside a cloned git repo on a machine with Docker engine.

## Compile Matlab

To run your application through a container, all Matlab scripts need to be compiled to a binary format using the [`mcc` MatLab command](https://www.mathworks.com/help/compiler/mcc.html). You can create a script that runs something like following.

```
#!/bin/bash
module load matlab/2017a

mkdir -p compiled

cat > build.m <<END
addpath(genpath('/N/u/brlife/git/vistasoft'))
addpath(genpath('/N/u/brlife/git/jsonlab'))
addpath(genpath('/N/soft/mason/SPM/spm8'))
mcc -m -R -nodisplay -d compiled myapp
%# function sptensor
exit
END
matlab -nodisplay -nosplash -r build

```

This script generates a Matlab script called `build.m` and immediately executes it. `build.m` will setup the path and run Matlab command called `mcc` which does the actual compilation of your Matlab code and generates a binary that can be run without Matlab license. The generated binary still requires a few Matlab proprietary libraries called MCR. You can freely download MCR library from Matlab website, or use MCR container such as brainlife/mcr to execute your binary with.

You will need to adjust addpath() to include all of your dependencies that your application requires. "myapp" is the name of the matlab script that you use to execute your application. mcc commad will create a binary with the same name "myapp" inside the ./compiled directory.

* `-m ...` tells the name of the main entry function (in this case it's `main`) of your application (it reads your config.json and runs the whole application)
* `-R -nodisplay` sets the command line options you'd normally pass when you run MatLab on the command line.
* -d ...` tells where to output the generated binary. You should avoid writing it out to the application root directory; just to keep things organized.

If your app uses any MEX files (.c code) you will need to add an extra option (-a) to specify where those MEX files are stored. For example..

```
mcc -m -R -nodisplay -a /N/u/hayashis/BigRed2/git/encode/mexfiles -d compiled myapp
```

* You can use OpenMP to parallelize your application by using a container that has libgomp1 installed.
* mcc compiled application can't run certain Matlab statements; like addpath(). You might need to create a stripped down version of the main function that does not include those statements (or wrap them inside `if not isdeployed` statement that gets executed only when you run it directly on Matlab) . 

If you don't have MatLab installed on your local machine, then you can do the compliation on the machine that has Matlab installed, the build the docker container on your own machine.

> NOTE. If you are loading any custom paths via startup.m, those paths may influence how your binary is compiled. At the moment, I don't know a good way to prevent it from loaded when you run build.m. The only workaround might be temporarly edit your startup.m to not include any addpath (or rename startup.m to startup.m.disabled) then run your compile script. 

## Loading an ENCODE fe structure from an mcc application

If you are writing a function that will load a fe structure containing a sparse tensor array created by ENCODE, you need to add the following decorator above the load function:

`%# function sptensor`

This lets the compiler know it should include the sptensor class, which `load()` does not use by default. If you do not include this, a warning will occur when you run the function stating the sptensor class was not found and it will load as an empty field. 

## Create a bash script to run your app

Unless your application sorely consists of matlab, you may want to create another bash script which will do any pre(/post)processing of the data. For example:

`docker.sh`
```bash
#!/bin/bash
./preprocess.sh #run some things (need to parse config.json using command line tool like jq)
./mca/main #then execute matlab binary
```

## Create Dockerfile (MatLab)

Before you start working on Dockerfile, make sure you are already familiar with the basic concepts of Docker and [how to build docker container](https://docs.docker.com/engine/reference/builder/). 

All Docker containers have a base-container. This base is used to derive all other containers. If your application uses Matlab, then I recommend using[the Brain-Life Docker Container](https://hub.docker.com/r/brainlife/mcr/). Alternatively, you could also use a more general OS container such as a Ubuntu, CentOS. The [neurodebian container](https://store.docker.com/images/63dbd2e1-f29e-498b-8b16-1477770ae733?tab=description) is a good alternative.

On this example, I am going to use [the LiFE Docker Container](https://github.com/brain-life/app-life/blob/master/Dockerfile) as a template (this was compiled using MCR:2016a on Ubuntu Linux compatible with NeuroDebian), but we are going to [app-dtiinit](https://github.com/brain-life/app-dtiinit) as an example to build a Docker container. To start:

1. cd into your local app-dtiinit folder (this folder should have the main file). 
2. copy the Dockerfile from [online](https://github.com/brain-life/app-life/blob/master/Dockerfile) into the current directory.
3. Open the `Dockerfile` and add the following lines, which will add a series of Docker commands: 
   1. Set the Docker base image and maintainer.
   ```
   FROM brainlife/mcr:R2016a
   MAINTAINER Your Name <youremail@iu.edu>
   ```
   2. Add dependencies. 
   This app requires the external, package FSL  to add this dependency we will the following lines to the Dockerfile.

   ```
   RUN sudo apt-get install fsl-complete
   ```
   3. Add the app-dtiinit to the Docker. 
   We want to put the entire content of you git repository (in our example because we have some MatLab code, the repository has been previously made into a standalone executable and compiled and the compiled version is under app-dtiinit/msa) on to this container somewhere. I am going to put it under /app.
   ```
   ADD . /app
   ```

   4. Lastly, need to specify the output directory to use
   ```
   RUN mkdir /output
   WORKDIR /output
   ```

   5. Then set the entry point of your application
   ```
   ENTRYPOINT ["/app/docker.sh"]
   ```

3b. Prepare for your container to be run under singularity. Singularity allows users to run your container where they don't have root access (or docker engine access). One issue with running Docker container under singularity is that, often dynamically linked libraries creates symlinks under /usr/lib64 directory when they are first executed. (TODO - explain why this fails under singularity). To setup all necessary symlinks before singularity executes the container, run ldconfig at the end of your Dockerfile

   ```
   RUN ldconfig
   ```

   For more information about singularity, please see http://singularity.lbl.gov/docs-docker#best-practices

4. Now, create a sample config.json on your current directory to be used..

`config.json`
```
{
   "some": "param",
   "another": 123,
   "input": "/input/somefile.nii.gz"
}
```

5. Build the container (make sure to name it something like `-t brainlife/someapp`), 

```
docker build -t brainlife/check.
```

6. Then run it to test it

```
docker run --rm -it \
        -v /host/input:/input \
        -v `pwd`:/output \
        brainlife/yourapp
```

7 (optional). You can also save the above two commands into separate .sh files for ease of access. 

```
vim run_docker.sh
(copy and paste into run_docker.sh
docker run --rm -it \
        -v /host/input:/input \
        -v `pwd`:/output \
        brainlife/yourapp)
./run_docker.sh
```

Do something similar for building the container into its own build.sh file.

## Create DockerFile (Python)
There are a few differences between the Matlab and Python versions of DockerFile. Namely the version and dependencies. While the Matlab one will show how to create one from an existing DockerFile, this part will show how to create a DockerFile from scratch. Please remember to install Docker before continuing.

1. In the folder of your Brain-Life application. Create a file called DockerFile.

```
vim Dockerfile
```

2. In the DockerFile, set the OS and version of Python you would like to use.

```
FROM ubuntu:16.04 (or whatever OS fits your fancy)
FROM python:2
```

3. Now install the dependencies your application requires.

To install pip and git, which you would most likely want to do, add the following to your DockerFile.

```
RUN apt-get update && apt-get install -y python-pip git
```

To pip install your dependencies add the following command:

```
RUN pip install numpy Cython your-other-dependencies
```

To git clone a repository and install from there add the following

```
RUN git clone the git url of the repository /rep-name
RUN cd /rep-name && python setup.py build_ext --inplace
```

4. Now we need to make a folder for adding our main python files. The DockerFile will run your applications from here.

```
RUN mkdir /app
COPY main.py /app
COPY another necessary .py file /app
```

Copy all necessary python files into the app folder.

5. We will now create a folder for the output of your application.

```
RUN mkdir /output
WORKDIR /output
```

If you want to test your application locally, make sure you have your **local** config.json file in the output folder.

6. If you did any git clones and install from the setup.py in there make sure you also set the PYTHONPATH like so

If you installed everything from pip, you may skip this step.
```
ENV PYTHONPATH /my-git-repo:$PYTHONPATH
```

7. Finally, we add the last line to finish up our DockerFile.

```
CMD /app/main.py
```

Congratulations! You wrote the DockerFile! You can see a full example here: "add example here Aman"

## Building and running your container

1. Build the container (make sure to name it something like `-t brainlife/someapp`), 

```
docker build -t brainlife/check .
```

2. Then run it to test it

```
docker run --rm -it \
        -v /host/input:/input \
        -v `pwd`:/output \
        brainlife/yourapp
```

3 (optional). You can also save the above two commands into separate .sh files for ease of access. 

run_docker.sh
```
docker run --rm -it \
        -v /host/input:/input \
        -v `pwd`:/output \
        brainlife/yourapp
```

Do something similar for building the container into its own build.sh file.

```
vim run_docker.sh
(copy and paste into build.sh)
docker build -t brainlife/check.
./build.sh (to run)
```

You only need to run the build once, unless you change something in your DockerFile. Otherwise you can directly run with the command or with run_docker.sh.

You will also probably need to give your run_docker.sh and build.sh permissions.

```
chmod +x build.sh run_docker.sh
```

## README

Once you know that your container works, you should push your container to docker hub and update the README of your git repo to include instructions on how to run your container.

## Examples

Matlab App Exmple 

* [github brain-life/app-life](https://github.com/brain-life/app-life)
* [dockerhub brainlife/life](https://hub.docker.com/r/brainlife/life/) (Dockerhub Autobuild)

Dipy (python) App Example

* [github brain-life/app-dipy-workflows](https://github.com/brain-life/app-dipy-workflows)


