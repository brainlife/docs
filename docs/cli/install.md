
# CLI Installation

With brainlife CLI, you can ..

* Upload/download data from your computer.
* Upload data stored in BIDS format.
* Submit Apps, and monitor (you can fully script data processing through brainlife)
* Query projects, data, datatypes, etc.

Brainlife CLI is distributed through [npm](https://www.npmjs.com/){target=_blank} (node package manager) which is a part of nodejs. You will need to have nodejs/npm installed on your machine before you can install brainlife CLI command. Most operation systems support nodejs through their software distribution systems. You can find the nodejs installation document [here](https://nodejs.org/en/download/package-manager/){target=_blank}.

On ubuntu/debian machines, you can run the following command to install nodejs 14 (current LTS).

```
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```

For other OSs, please see [Installing node.js](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions){target=_blank} page.


!!! Info "For Windows Users"
    In order to use brainlife CLI on Windows, you will first need to install [WSL (Windows Subsyste for Linux)](https://docs.microsoft.com/en-us/windows/wsl/install-win10){target=_blank} and run all commands inside the Ubuntu terminal.

    Apart from WSL, we recommend the following software packages installed on Windows systems.

    * [7-zip](https://www.7-zip.org/){target=_blank} for uncompressing downloaded data (.tar.gz)
    * [MRIcron](https://www.nitrc.org/projects/mricron){target=_blank} for visualizing brainimaging data 

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
  data           view and utilize stored data
  app            query and run brainlife apps
  help [cmd]     display help for [cmd]

```

## Updating the CLI

We are making a lot of bug fixes/updates. If you run into any problem, please try updating the CLI by running the following. 

```
$ sudo npm update -g brainlife
```

## Login

Before you can start using bl tool, you should login to brainlife by running the following command.

!!! note
    If you don't have a brainlife account, please [Sign Up](https://brainlife.io/auth/#!/signup){target=_blank} before proceeding.

```
$ bl login --ttl 7
username:  hayashis
password:  
Successfully logged in for 6 days, 23 hours, 59 minutes, 59 seconds
```

The `--ttl 7` will request brainlife to keep you logged in for 7 days. Please adjust this number if necessary.

### Signout

`bl login` command will store your access token under the following path

```
~/.config/brainlife.io/.jwt
```

brainlife CLI itself does not provide a capability to sign out natively, but you can remove this file if you wish to sign out before your token expires. 

### Docker container
You can use [brainlife CLI](https://hub.docker.com/repository/docker/brainlife/cli){target=_blank} hosted as a docker container via Singularity by running the following command.
```
$ singularity run docker://brainlife/cli login
```
You can then use any of the functionality of the CLI without needing to install all of the dependencies on your local machine!

To run it using docker itself, you can
```
$ docker run -it brainlife/cli login 
```

If you don't want to keep typing `singularity run docker://brainlife/cli` every time you run brainlife CLI, you can add this to your `~/.bashrc`

```
function bl {
    singularity run docker://brainlife/cli $@
}
```

!!! note
    depending on how your environment is configured, you might need to add this on each bash script you are executing (or do `source ~/.bashrc` in your script)


### Google Colab 

You can install brainlife CLI on Google Colab by running `!npm install brainlife -g` with a code block. All brainlife CLI should work, except for the`bl login` command. 
The login command will not work as it requires TTY to prompt for password and Colab does not support it. 
For now, please copy and paste the following python snippet to perform the login operation 
and issue JWT token used by other `bl` CLI.

```python
import getpass
import requests
import json
import os

username = str(input("Enter brainlife username: "))
password = getpass.getpass("Enter brainlife password: ")

#issue jwt
res = requests.post('https://brainlife.io/api/auth/local/auth', data={'username': username, 'password': password})
body = json.loads(res.content)
print(body["message"], res.status_code)

#store jwt
if res.status_code == 200:
  if not os.path.exists('/root/.config'):
    os.mkdir("/root/.config")
  if not os.path.exists('/root/.config/brainlife.io'):
    os.mkdir("/root/.config/brainlife.io")
  with open("/root/.config/brainlife.io/.jwt", "w") as f:
    f.write(body["jwt"])
    print("stored jwt")
```

!!! note
    The issued token will be stored on `/root/.config/brainlife.io/.jwt`. This directory is no persisted, 
    so each user will need to run the code above to login, or each time the VM used to host your notebook 
    is initialized.


