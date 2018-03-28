
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

* **Datasets Archive**

    The datasets tab you are seeing now shows the current content of your dataset archive. Datasets under this tab are stored in our object storage permanently (and some are backed up to our tape archives also).

* **Process Scratch Space**

    You can not directly use archived datasets to run apps. To run apps, datasets will be automatically staged out of your archive and transferred to Brainlife's scratch space and on various compute resources where apps are executed.

    Datasets on process scratch space will be automatically removed within 25 (or sonner). If you have any output datasets that you'd like to keep them permanently, you will need to archive it back to the Dataset Archive. 

Now, click `plus button` at the bottom of the screen to open the dataset upload dialog.

![upload form](/img/upload.form.png)

Select Datatype that you'd like to upload (currently limited to t1/t2 and dwi) and upload your dataset. 

!!! note
    If you don't have any data to upload, you can skip this step and you can use pre-uploaded datasets from various public projects.

Upload form will run server side validation / data normalization service. You can check the result from this step. If everything looks good, click `Archive`.

![update validate](/img/upload.validate.png)

Once uploaded, you should see a new dialog showing details about your new datasets. All archived datasets are immutable (readonly), but you can make changes to the metadata if necessary (desc, tags, etc..) 

![dataset](/img/dataset.png)

!!! note
    `Stored in` field shows where your dataset is archived. For larger dataset, it might take a while for it to be archived. Please give a few minutes.

## Visualize Dataset

Any datasets stored in Brainlife can be visualized using Web-based and Native(Docker) visualization apps registered for specific datatypes. To launch a visualizer, click on visualizer icon(:fa-eye:) at the top of dataset dialog.

For example, `neuro/anat/t1` datasets can be visualized by following set of Visualization apps.

![dataset](/img/viewers.png)

!!! note
    Similar to `Apps`, developers can develop and contribute new visualization apps to run on Brainlife. If you are developing visualization apps, or have apps that you'd like us to add, please contact us at [brlife@iu.edu](mailto:brlife@iu.edu).

Click any of the visualization app that you'd like to launch to visualize your data. 

## Downloading BIDS

You can search / select and bulk download datasets. On the dataset table, select datasets you'd like to download by clicking on the check box, then click `Download (BIDS)` button.

![dataset](/img/download.png)

Brainlife will stage selected datasets, organize them into a BIDS structure, and let you download the whole structure as a single tar ball. Once it's ready, click `Download`.

!!! note 
    At the moment, all datasets will simply be stored under `/derivatives` directory - due to limitation on BIDS specification.

## Apps

Before we proceed to `Process` tab, let's take a detour and visit the `Apps` page.  

![dataset](/img/apps.png)

This page shows all Brainlife apps that are publicly available that you can execute on resources and datasets that you have access to. Please take a look and see what type of apps are available. You can click on each tiles to see more details.

On Brainlife, **apps* are usually small programs that perform specific set of data processing. Although we have a few apps that behaves more like a *pipeline* or *workflow*, most Brainlife apps only does one thing, and one thing well. By joining these small apps together to form a *workflow*, it gives you greater flexibility, increased code reuse, and allow developers to focus on the task that they want to implement, rather than pre/post processing normally required to make the app functional.

