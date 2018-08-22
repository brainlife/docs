# Containerizing App

Docker is a software containerization tool that allow you to package your App and its dependencies into a portable *container* that can you can run on any machine that supports Docker engine, or singularity.

Although you can create a fully functional standalone Docker container for your app, for Brainlife, we recommend to containerize only the App's *environment and the dependencies*, and not include the main part of your App (your python or Matlab scripts that drives your algorithm) on your container.

You can use singularity to run the most ideosyncratic parts of your App which can be stored and maintained inside your github repo and injected into an environment container at runtime. Like..

```bash
singularity exec -e docker://brainlife/dipy:0.13 ./app.py
```

In above example, I am using `brainlife/dipy:0.13` as an environment container, and running app.py which is stored locally (comes with the github repo for my App) and injected into my environment at runtime. 

You can even share the same Docker container across multiple Apps that you and your lab members maintain, but you should make sure to specify the container version ("0.13" in above example) so that your App won't be affected when the container gets updated. 

## Docker Engine

To build a docker container, you need to [install Docker engine on your laptop](https://docs.docker.com/machine/install-machine/) or find a server that has docker engine installed that you can use. (Contact [Soichi](mailto:hayashis@iu.edu) if you need a help.)

We assume you already have your Brainlife app hosted on Github, and you are making changes inside a cloned git repo on a machine with Docker engine.

## Compiling Matlab Scripts

> Skip this section if you are not using Matlab

Matlab code requires a matlab license to run. If you are App contains any Matlab code, it needs to be compiled to a binary format using [`mcc` command](https://www.mathworks.com/help/compiler/mcc.html) which allows you to execute your code without Matlab license. You will still need to run it against a special Matlab compiled runtime called *MCR* which can be distributed freely and does not require any license.

You can create a script that compiles your matlab code.

#### compile.sh

```bash
#!/bin/bash
module load matlab/2017a

mkdir -p compiled

cat > build.m <<END
addpath(genpath('/N/u/brlife/git/vistasoft'))
addpath(genpath('/N/u/brlife/git/jsonlab'))
addpath(genpath('/N/soft/mason/SPM/spm8'))
mcc -m -R -nodisplay -a /N/u/brlife/git/vistasoft/mrDiffusion/templates -d compiled myapp

%# function sptensor
exit
END
matlab -nodisplay -nosplash -r build

```

This script generates a Matlab script called `build.m` and immediately executes it. `build.m` will load Matlab paths and run Matlab command called `mcc` which actually compiles of your Matlab code into an executable binary that can run without Matlab license. The generated binary still requires a few Matlab proprietary libraries called MCR which can be download from Matlab website. 

!!! note
        Brainlife team has built a Docker MCR container [brainlife/mcr](https://hub.docker.com/r/brainlife/mcr/tags/) which can be used to execute your compiled Matlab code with singularity.

For the sample build script above, you will need to adjust addpath() to include all of your Matlab dependencies that your Matlab script requires. `myapp` is the name of the matlab entry function that you use to execute your application. mcc command will create a binary with the same name `myapp` inside the ./compiled directory.

A few other mcc options to note.

* `-m ...` tells the name of the main entry function (in this case it's `main`) of your application (it reads your config.json and runs the whole application)
* `-R -nodisplay` sets the command line options you'd normally pass when you run MatLab on the command line.
* -d ...` tells where to output the generated binary. You should avoid writing it out to the application root directory; just to keep things organized.
* If your app or your Matlab libraries access any non-Matlab files (mexfiles, datafiles, models, templates, etc..) you will need to include them by specifying paths with `-a`. mcc will include specified files/directories as part of your compiled binary and make it available to the compiled code as if it is available through local filesystem (similar to how Docker containerizes things).

* If you are using OpenMP, you will need to include libgomp1 library installed in your MCR container.
* mcc compiled application can't run certain Matlab statements; like addpath. You will need to wrap some code with `if ~isdeployed` type statement to prevent it from getting executed.

!!! note
        If you are loading any custom paths via startup.m, those paths may influence how your binary is compiled. At the moment, I don't know a good way to prevent it from loaded when you run build.m. The only workaround is to temporarly rename your startup.m to something else while you are compiling your code.s

### Loading custom Matlab data structure

If your Matlab code is loading any data structure that contains custom Matlab data structures (like sparse tensor array in encode's fe structure), addpath() is not enough to include those data structures. You will need to explicitly tell Matlab compiler that you are using those data structures, like..

`%# function sptensor`

This odd looking *comment* lets the Matlab compiler know that it should include the sptensor class to the compiled binary.

<!--
## Create Dockerfile (MatLab)

Before you start working on Dockerfile, make sure you are already familiar with the basic concepts of Docker and [how to build a docker container](https://docs.docker.com/engine/reference/builder/). 

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

-->

## Creating Docker Container

There are quite a few materials detailing how to write Dockerfile / containers online. If you are looking for a place to start, we recommend [Docker's Getting Started Guide](https://docs.docker.com/get-started/).

As most Brainlife Apps execute Docker containers through singularity, there are a few Brainlife specific items that you might need to be aware so that your App will run properly on Brainlife.

### ldconfig

As singularity runs containers as a normal user, we must perform some OS level initialization steps outside the singularity.

When Docker installs OS packages, it won't initialize dynamicly linked library links until they are first invoked. As singularity runs as normal user with set uid disabled, it won't be able to setup these paths once container is executed through singularity. To fix this, we can run `ldconfig` command. This command creates the necessary links and cache to the most recent shared libraries installed in the lib directories. In your Dockerfile, please be sure to run ldconfig as the last command. Like...

```
#make it work under singularity
RUN ldconfig 
```

### IU HPC paths

Some OS (like RHEL6) don't allow singularity to mount a host path unless there is a corresponding directory already present inside the container (due to lack of overlayfs support by the kernel).

To make your App run on RHEL6 hosts (like IU Karst), please add following somewhere inside your Dockerfile

```
RUN mkdir -p /N/u /N/home /N/dc2 /N/soft
```

### Use Bash 

If you are using ubuntu, by default it changes the default /bin/sh to point to a thing called "dash" rather than "bash". "dash" is simpler/faster to load than "bash", but unfortunately it breaks a lot of programs that expects /bin/sh to point to bash. To correct this, you can add following in your Dockerfile to reset it to bash.

```
#https://wiki.ubuntu.com/DashAsBinSh
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
```

Now, your container should be ready to GO!

## Example Dockerfile for environment containers

* [brain-life/app-dipy-workflows](https://github.com/brain-life/app-dipy-workflows/blob/master/Dockerfile)
* [brain-life/docker-mcr](https://github.com/brain-life/docker-mcr)

## Examples Apps using environment containers

* [github brain-life/app-freesurfer](https://github.com/brain-life/app-freesurfer)
* [github brain-life/app-wmaSeg](https://github.com/brain-life/app-wmaSeg)

<!--

Matlab App Exmple 

* [github brain-life/app-life](https://github.com/brain-life/app-life)
* [dockerhub brainlife/life](https://hub.docker.com/r/brainlife/life/) (Dockerhub Autobuild)

Dipy (python) App Example

* [github brain-life/app-dipy-workflows](https://github.com/brain-life/app-dipy-workflows)

-->

