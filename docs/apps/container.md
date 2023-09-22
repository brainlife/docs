# Run App via a Singularity

Docker is a software containerization tool that allows you to package your App and its dependencies into a portable *container* that you can run on any machine that supports Docker engine, or singularity.

Although you can create a fully functional standalone Docker container for your app, for Brainlife, we recommend to containerize only the App's *environment and the dependencies*, and not include the main part of your App (your python or Matlab scripts that drives your algorithm) on your container.

You can use singularity to run the most idiosyncratic parts of your App which can be stored and maintained inside your github repo and injected into an environment container at runtime. Like...

```bash
singularity exec -e docker://brainlife/dipy:0.13 ./app.py
```

In the above example, I am using `brainlife/dipy:0.13` as an environment container, and running app.py which is stored locally (comes with the github repo for my App) and injected into my environment at runtime. 

You can even share the same Docker container across multiple Apps that you and your lab members maintain, but you should make sure to specify the container version ("0.13" in the above example) so that your App won't be affected when the container gets updated. 

## Docker Engine

To build a docker container, you need to [install Docker engine on your laptop](https://docs.docker.com/machine/install-machine/){target=_blank} or find a server that has docker engine installed that you can use. (Contact [Franco](mailto:pestilli@utexas.edu), [Anibal](mailto:anibalsolon@utexas.edu) or [Kim](mailto:kimray@utexas.edu) if you need help.)

We assume you already have your Brainlife app hosted on Github, and you are making changes inside a cloned git repo on a machine with the Docker engine.

## Compiling Matlab Scripts

> Skip this section if you are not using Matlab

Matlab code requires a matlab license to run. If your App contains any Matlab code, it needs to be compiled to a binary format using [`mcc` command](https://www.mathworks.com/help/compiler/mcc.html){target=_blank} which allows you to execute your code without Matlab license. You will still need to run it against a special Matlab compiled runtime called *MCR* which can be distributed freely and does not require any license.

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
exit
END
matlab -nodisplay -nosplash -r build

```

This script generates a Matlab script called `build.m` and immediately executes it. `build.m` will load Matlab paths and run Matlab command called `mcc` which compiles your Matlab code into an executable binary that can run without Matlab license. The generated binary still requires a few Matlab proprietary libraries called MCR which can be download from the Matlab website.

!!! note
        Brainlife team has built a Docker MCR container [brainlife/mcr](https://hub.docker.com/r/brainlife/mcr/tags/){target=_blank} which can be used to execute your compiled Matlab code with singularity.

For the sample build script above, you will need to adjust addpath() to include all of your Matlab dependencies that your Matlab script requires. `myapp` is the name of the matlab entry function that you use to execute your application. mcc command will create a binary with the same name `myapp` inside the ./compiled directory.

A few other mcc options to note.

* `-m ...` tells the name of the main entry function (in this case it's `main`) of your application (it reads your config.json and runs the whole application)
* `-R -nodisplay` sets the command line options you'd normally pass when you run MatLab on the command line.
* -d ...` tells where to output the generated binary. You should avoid writing it out to the application root directory; just to keep things organized.
* If your app or your Matlab libraries access any non-Matlab files (mexfiles, datafiles, models, templates, etc..) you will need to include them by specifying paths with `-a`. mcc will include specified files/directories as part of your compiled binary and make it available to the compiled code as if it is available through the local filesystem (similar to how Docker containerizes things).

* If you are using OpenMP, you will need to include libgomp1 library installed in your MCR container.
* mcc compiled application can't run certain Matlab statements; like addpath. You will need to wrap some code with `if ~isdeployed` type statement to prevent it from getting executed.

!!! note
        If you are loading any custom paths via startup.m, those paths may influence how your binary is compiled. At the moment, I don't know a good way to prevent it from loaded when you run build.m. The only workaround is to temporarily rename your startup.m to something else while you are compiling your code.s

### Loading custom Matlab data structure

If your Matlab code is loading (by doing something like `load('data.mat')`) any custom data structures (like a sparse tensor array in encode's fe structure), addpath() is not enough to include them into the compiled binary. You will need to explicitly tell Matlab compiler that you are using those data structures by including a special comment like the following.

`%# function sptensor`

This tells the Matlab compiler to include the sptensor class to the compiled binary.

## Creating Docker Container

There are quite a few materials detailing how to write Dockerfile / containers online. If you are looking for a place to start, we recommend [Docker's Getting Started Guide](https://docs.docker.com/get-started/){target=_blank}.

As most Brainlife Apps execute Docker containers through singularity, there are a few Brainlife specific items that you might need to be aware so that your App will run properly on Brainlife.

### ldconfig

As singularity runs containers as a normal user, we must perform some OS-level initialization steps outside the singularity.

When Docker installs OS packages, it won't initialize dynamically linked library links until they are first invoked. As singularity runs as a normal user with set uid disabled, it won't be able to setup these paths once the container is executed through singularity. To fix this, we can run `ldconfig` command. This command creates the necessary links and cache to the most recent shared libraries installed in the lib directories. In your Dockerfile, please be sure to run ldconfig as the last command. Like...

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

If you are using ubuntu, by default it changes the default /bin/sh to point to a thing called "dash" rather than "bash". "dash" is simpler/faster to load than "bash", but unfortunately it breaks a lot of programs that expect /bin/sh to point to bash. To correct this, you can add following in your Dockerfile to reset it to bash.

```
#https://wiki.ubuntu.com/DashAsBinSh
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
```

Now, your container should be ready to GO!

## Example Dockerfile for environment containers

* [brainlife/app-dipy-workflows](https://github.com/brainlife/app-dipy-workflows/blob/master/Dockerfile){target=_blank}
* [brainlife/docker-mcr](https://github.com/brainlife/docker-mcr){target=_blank}

## Examples Apps using environment containers

* [github brainlife/app-freesurfer](https://github.com/brainlife/app-freesurfer){target=_blank}
* [github brainlife/app-wmaSeg](https://github.com/brainlife/app-wmaSeg){target=_blank}

