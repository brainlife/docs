
## Background

Brainlife provides a mechanism to make a computing resource, such as a Cloud compute system or a high-performance computing cluster, available to specific projects or to the entire community of users. This mechanism is supported via [Amaretti](https://github.com/brain-life/amaretti). By default, a compute resource shared via Amaretti is available to all users, but this can be changed. Apps must be registered to run on the resource. Once an App is registered on a resoruce users can submit a process running the Apps.

As shared resources are shared by all users, often users have to wait on the queue for requested tasks to get executed. Due to potential access control violation, we do not recommend any sensitive data to be processed using shared resources.

Brainlife allows you to register your own compute resources for the following use cases.

1. You have access to your own HPC resource, and you'd like to use it to run Brainlife apps, for better performance, or better access control.
2. You are an app developer and you'd like to use your own resource to troubleshoot your apps on your own resource, for easier debugging.
3. You are an app developer and your app can only run on specialized resources (like Hadoop, Spark, etc..) that Brainlife's shared resources do not provide.

Currently, Brainlife does not allow non-admin users to share your personal resources with other members. 

Resource owner decides which apps are allowed to run on their resource. If you register a resource and enable some apps on it, only you can run those apps on your resources. If you are going to publish your app, and you want all users to be able to execute your app, please ask Brainlife's resource administrator (for now, it's just [Brainlife admin](mailto:brlife@iu.edu) ) to enable your app.

## Registering Resources

To register your resource, go to [Brainlife Settings](https://brainlife.io/wf/#!/resources) page, and Under "HPC Systems" click "Add New Account". A resource form should appear. Please populate the following fields.

* *Name* Enter the name of rhe resource
* *Hostname* The hostname of your compute resource (usually a login/submit host)
* *Username* Username used to ssh to this resource
* *Workdir* Directory used to stage and store generated datasets by apps. *You should not share the same directory with other resources*. Please make sure that the specified directory exits (mkdir if not).
* SSH Public Key: Copy the content of this key to your resource's ~/.ssh/authorized_keys. Please read [authorized_keys](https://www.ssh.com/ssh/authorized_keys/) for more detail.

You can leave the rest of the fields empty for now.

Click OK. Once you are finished with copying ssh key and make sure the workdir exists, click "Test" button to see if Brainlife can access your resource. You should see a green checkbox if everything is good.

## Configuring Resources

Once you register your resource, you will need to do a few things to prepare your resources so that Brainlife can successfully execute Brainlife apps on your resource.

### ABCD Default Hooks

Some app provides its own [ABCD Hooks](https://github.com/brain-life/abcd-spec), however, many of them relies on default hooks that are installed on each resource. As a resource provider, you will need to provide these default hooks ("start", "stop", and "status" scripts) and make them available to all app by adding them to `$PATH`. If you are not sure how to write these scripts, you can install and use the our default ABCD hooks by doing following.

```
cd ~
git clone https://github.com/brain-life/abcd-spec
```

Then, add one of following to your ~/.bashrc

#### For PBS cluster

```
export PATH=~/abcd-spec/hooks/pbs:$PATH
```

#### For Slurm cluster

```
export PATH=~/abcd-spec/hooks/slurm:$PATH
```

#### For direct execution - no batch submission manager

```
export PATH=~/abcd-spec/hooks/direct:$PATH
```

### Common Binaries

Brainlife expects certain binaries to be installed on all resources. Please make sure following commands are installed.

* jq (command line json parser commonly used by Brainlife apps to parse config.json)
* git (used to clone / update apps installed)
* singularity (user level container execution engine)

For IU HPC resource, please feel free to use following ~/bin directory which contains jq

[~/.bashrc]

```
export PATH=$PATH:/N/u/brlife/Carbonate/bin
```

For singularity, you can either install it on the system, or for HPC systems you should add following in your `~/.modules` file.

```
module load singularity
```

By default, singularity uses user's home directory to cache docker images (and /tmp to create a merged container image to run). If you have limited amount of home directory, you should override these directories by setting something like the following in your .bashrc

```
export SINGULARITY_CACHEDIR=/N/dc2/scratch/<username>/singularity-cachedir
```

Please replace <username> with your username, and make sure specified directories exists.

## Enabling Apps

Once you have registered and configured your resource, now you can enable apps to run on your resource.

Go back to the Brainlife / Settings / Resources page, and click the resource you have created. Under services section, enter the git org/repo name (like "brain-life/app-life)" of the app that you'd like to enable, and the score for each service. The higher the score is, the more likely the resource will be chosen to run your app (if there are multiple resources available). Brainlife gives higher score for resources that you own, so you can just leave it the default of 10 - unless it's competing with another resource that you own. Click OK.

You can see which resource an app is configured to run, and which resource will be chosen when you submit it under App detail / Computing Resources section on Brainlife. [example](https://brainlife.io/warehouse/app/58c56cf7e13a50849b258800)


