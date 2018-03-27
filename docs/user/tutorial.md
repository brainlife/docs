
# Tutorial

This tutorial will guides you through some main functionalities of Brainlife.io. For more high-level overview of Brainlife, see [About Page](/about).

## Sign Up

If you have not registered to Brainlife.io yet, please do so by visiting The [Authentication Page](https://brainlife.io/auth/) and clicking on a preferred 3rd party authentication method; Google, ORCID, Github, or through your institution.

!!! warning
    If you register through the 3rd party authenticator, please use the same authenticator each time you login, or you will end up creating multiple Brainlife accounts. 

If you'd like to setup a dedicated user/pass for Brainlife, please click "Sign Up" link to enter a new user name / password. You will be asked to confirm your email address once you register.

!!! note
    You can associate multiple authenticator to your account once you register, by going to Settings / Account, Connected Accounts, and click "Connect" next to various 3rd party authenticators. 

## Create Project

Once you login, you should land on Brainlife Apps page. 

Before we can start using Brainlife, we need to create a new project. 

Click on `Project` button on the left hand side menu, then click a plus side button at the bottom of the project list.

![project new button](/img/project_new_button.png)

!!! note
    *Project* is where you can organize your dataset, do data processing, and share them with your project members. For more information about project, please read [project page](/user/project)

Enter any `name`, and `description`, and leave everything else default. Click `Submit`. 

Congratulations! You just created your first private project!

## Upload Dataset

Now, let's upload some test datasets. Open the `Datasets` tab.

!!! note
    In Brainlife, datasets are set of files / directories for specific modality or data derivatives, for a specific subject. All data processing is done at the subject level. Datasets are immutable;you can only modify the metadata, but not the data files once you create them.

Brainlife has 2 kinds of data storages. 

* Datasets Archive

    The datasets tab you are seeing now shows the current content of your datasets archive. Datasets stored under this tab will be stored in our object storage permanently (and some are even backed up to our tape archives).

* Process Scratch Disk

    You can not directly use archived datasets to run apps. To run apps, datasets will be automatically staged out of your archive and transferred to Brainlife's scratch volumes and on compute resources where apps are actually submitted. 




