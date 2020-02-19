
# CLI Installation

With brainlife CLI, you can ..

* Upload/download datasets from your computer.
* Upload datasets stored in BIDS format.
* Submit Apps, and monitor (you can fully script data processing through Brainlife)
* Query projects, datasets, datatypes, etc.

Brainlife CLI is distributed through [npm](https://www.npmjs.com/)(node package manager) which is a part of nodejs. You will need to have nodejs/npm installed on your machine before you can install brainlife CLI command. Most operation systems support nodejs through their software distribution systems. You can find the nodejs installation document [here](https://nodejs.org/en/download/package-manager/).

```
$ sudo apt install nodejs
```

!!! Info "For Windows Users"
    In order to use brainlife CLI on Windows, you will first need to install [WSL (Windows Subsyste for Linux)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and run all commands inside the Ubuntu terminal.

    Apart from WSL, we recommend the following software packages installed on Windows systems.

    * [7-zip](https://www.7-zip.org/) for uncompressing downloaded data (.tar.gz)
    * [MRIcron](https://www.nitrc.org/projects/mricron) for visualizing brainimaging data 

!!! note
    On IU Karst / Carbonate / RED, brainlife CLI is already installed as part of nodejs module. Please run `module load nodejs` and skip this installation step.

Once you have nodejs/npm installed, you can then run the following command to install brainlife CLI on your environment.

```
$ sudo npm install -g brainlife
```

You can then check the installation by 

```
$ bl --version
1.3.7

$ bl -h
Usage: bl [options] [command]

Options:

  -V, --version  output the version number
  -h, --help     output usage information

Commands:

  login          login to brainlife and generate a temporary access token
  profile        query the available list of profiles
  resource       query the available list of resources
  datatype       query the available list of datatypes
  project        create and view brainlife projects
  dataset        view and utilize stored datasets
  app            query and run brainlife apps
  help [cmd]     display help for [cmd]

```

## Updating the CLI

We are making a lot of bug fixes/updates. If you run into any problem, please try updating the CLI by running the following. 

```
$ sudo npm update -g brainlife
```

## Login

Before you can start using bl tool, you should login to Brainlife by running the following command.

!!! note
    If you don't have a brainlife account, please [Sign Up](https://brainlife.io/auth/#!/signup) before proceeding.

```
$ bl login --ttl 7
username:  hayashis
password:  
Successfully logged in for 6 days, 23 hours, 59 minutes, 59 seconds
```

The `--ttl 7` will request Brainlife to keep you logged in for 7 days. Please adjust this number if necessary.

### Signout

`bl login` command will store your access token under the following path

```
~/.config/brainlife.io/.jwt
```

brainlife CLI itself does not provide a capability to signout natively, but you can remove this file if you wish to signout before your token expires. 
