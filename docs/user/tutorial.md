
# Tutorial

This tutorial will guides you through following functionalities of Brainlife. 

* Signing up
* Create new project and uplaod data
* Launch visualizer to visualize your data
* Run process on datasets and archive results.

For more high-level overview of Brainlife, see [About Page](/about).

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

Before we proceed to `Process` tab, let's take a quick detour and visit the `Apps` page.  

![dataset](/img/apps.png)

The `Apps` page shows all Brainlife apps that are publicly available that you can execute on resources and datasets that you have access to. Please take a look and see what type of apps are currently available. You can click on each tiles to see more details.

On Brainlife, **apps* are normally small programs that perform specific set of data processing. Although we have a few apps that behaves more like a typical *pipeline* or *workflow* (including pre/post processing, data analysis, reporting, etc..), most Brainlife apps should only do one thing, and one thing well. Please see (about)[/about] for more details.

Brainlife apps interoperate through `datatypes`. Developers exchanging input/output dataset should discuss and agree on set of files / directory structure between the apps and register new datatype by submitting an issue ticket on [datatypes github repo](https://github.com/brain-life/datatypes)

![dataset](/img/datatype.png)

Various colored boxes shows the input and output datatypes. For example, above image shows that this app will take `dwi` input dataset, and generates another `dwi` dataset with datatype tag of "masked", and also output another dataset of a datatype `mask`. 

For more information on datatype, please visit [datatypes page](/user/datatypes)
<!--If you are familiar with an electronic prototyping product called *littleBits*, it is very similar in the concept.-->

## Data Processing

Now that we know what `Apps` are, let's go back to your private project, then open `Processes` tab. You should see an empty page as you don't have any processes yet. On Brainlife, `Process` where you can submit a group of tasks/apps that can share input / output datasets. 

Let's create a new process by clicking the plus button at the right bottom corner of the page. Enter any name you'd like for your process. You should see a screen that looks like this now.

![dataset](/img/empty.process.png)

To process data, you first need to stage any dataset from our archive to your process. Each process can only process data that is either staged, or generated by other apps. Click `Stage New Dataset` button. On the Select Datasets dialog, let's select `NKI (Rockland Sample)` project, and select any `anat/t1w` data.

![dataset](/img/select.datasets.png)

Click `OK` to stage. You should see a box showing "Staging Datasets" with selected datasets. While it's staging your data, let's submit our first App. Click `Submit New App` button. A dialog should show up with a list of `Apps` that you can submit using your `anat/t1w` dataset. Brainlife tries best to limit your selection options to prevent you from submitting apps that you don't have all required input datasets yet.

Let's run `ACPC alignment with ART` app on your data. `ACPC alignment` is a common alignment tool used to re-orient / re-position the Brain image in common orientation suited for further image analysis. 

Find and click the app, then make sure that Brainlife has automatically selected your staged data as input. Leave other options default. Click `Submit`. Brainlife should now find the most appropriate resource to run this app, and transfer data to the resource and submit it to the local batch scheduler.

Once started, job should take a few minutes to run. Once completed, you should see a screen that looks like this.

![dataset](/img/task.success.png)

You can browse / download any output as they are generated under `Raw Output` area. Once completed successfully, you can launch various visualization tools by clicking the :fa-eye: button next to the Output section. Open `Volume Viewer` on both the original input data and the ACPC alignment output data and see how this algorithm has re-oriented your data. 

Below is my before/after view. Do you spot a difference?

![dataset](/img/acpc.orient.png)

**Congratulations!** You have successfully staged data, submitted an app with it, and visualized output data through Brainlife. 

Now, it's important to note that, all processes are meant to be temporary and Brainlife will remove processes within 25 days of data generation. If you'd like to permanently keep the output dataset you just generated, you will need to `archive` data generated by clicking on :fa-archive: button next to the Output section. It will open an inline modal where you can edit any metadata, description, and click the `Archive` button to archive it.

After you archive your data, open the `Datasets` tab and make sure that your dataset is listed there. You can click on the dataset record to see more details.

Once you finish running ACPC alignment, you will be able to submit a few new apps under `Submit New App` dialog. Please feel free to submit other apps / stage more datasets.

## Datasets

As you submit more apps and generate datasets from them, it becomes harder to keep up with how a given dataset was generated. Brainlife keeps track of a record of how a given dataset was generated all the way from the original input dataset (called `data provenance`). 

Under `Datasets` tab, select any dataset you have generated and look under `Provenance` tab. 

![dataset](/img/provenance.png)

The green boxes are the input datasets (came from outside Brainlife) and white boxes are Apps ran to generate the data derivatives. You can pan / zoom the diagram, or re-layout some items by dragging/dropping.

## What's Next

You should now be familiar with basic functionalities of Brainlife. Please take a look at other pages for more information. For example, if you'd like to write your app and register on Brainlife, please take a look at [App Developer Guide](/apps/introduction). If you'd like to learn how to bulk process multiple subjects, please take a look at [Pipeline](/user/pipeline).

Please let us how we can improve this tutorial, or send us your edits via pull-requests.