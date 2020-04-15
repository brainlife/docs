
# Getting Started with brainlife.io

Now, let’s learn how to use brainlife.io! This “Getting Started” tutorial will give you a quick overview for how to:
* Sign up 
* Create new projects
* Upload data 
* Launch visualizations
* Run processes on data
* Archive results 

We will cover some of these topics more in-depth in other sections of the documentation.
 
## Sign Up

To begin using brainlife.io, you need first make sure you are registered on the site ([you can do that here](https://brainlife.io/auth/)). You can choose to sign up through Google, ORCID, Github, or through your institution. 

If you want your own dedicated brainlife.io username and password, you can [sign up for brainlife.io] here(https://brainlife.io/auth/#!/signup) to register an account. You will be asked to confirm your email address once you register.

You can associate multiple authenticators to your account once you register by going to Settings > Account > Account Settings. Scroll down to Connected Accounts and click "Connect" next to the authenticator you want to connect through. 

!!! warning
    If you register through a third party authenticator, please use the same authenticator each time you login, or you will end up creating multiple Brainlife accounts. 

## Create Project

Nice, you now have a brainlife.io account! To begin working with brainlife.io, you will need to first create a new project. 

Projects is where you can organize your datasets, perform data processing, and share work with your team. You will learn more about Projects in the [Projects section](https://brainlife.io/docs/user/project/) of the documentation.

You should automatically land on the Projects page when you enter brainlife.io. If not, click the `Project` button from the menu on the lefthand side. Then, press the `New Project` button in the righthand corner of the screen (it will look like a plus sign until you hover your mouse over it).

![project new button](/docs/img/project_new_button.png)

Enter any `Name` and `Description` -- be sure to leave everything else as the default option. Then click `Submit`. 

Congratulations! You just created your first private project on brainlife.io. That was easy!

### What is a Data-Object?

Before we dive into the next section, we should cover a term that you will become very familiar with on brainlife.io called a **data-object.** We descrive data-objects as a set of files for a specific subject and modality. A data-object is the smallest set of data that you can interact with on brainlife.io. For example, `neuro/dwi` data-object consists of files such as `dwi.nii.gz` (a file containing the actual 4D diffusion brain image data), `dwi.bvecs`, and `dwi.bvals` (which describe how the image was acquired by the MRI scanner). Freesurfer output for a subject is considered to be a single "data-object" containing many directories and files. brainlife.io processes data at each subject level and for each data-object. You cannot change the content of a data-object once you create it, but you can modify its metadata.


## Upload Data

Now, let's upload some test data. Once you save your project, it will appear on the top of the Projects page in the `My Projects` section. Click on your project and then click the `Archive` tab.                                    

There are two ways to store data on brainlife.io:

* **Archive**
    The `Archive` tab you are currently on shows all the current content in your data archive. Data-objects under this tab are stored in our archival storage permanently (but not backed-up until you publish them).
    
* **Processes**

    You cannot directly run Apps on archived data. To run Apps, data-objects will be automatically staged out of your archive and transferred to various compute resources, where the Apps will be executed. Data generated in the `Processes` page will be automatically removed within 25 days or sooner. If you have any output data that you'd like to keep permanently, you will need to archive them in the Archive page. 

Now, click the same button you clicked in the lower righthand corner of the screen to create your project. When you hover over the button this time, it will say `Upload Dataset`. Click it!

![upload form](/docs/img/upload.form.png)

The form above will pop up. Select the Datatype that you want to upload, attach your file, and provide the subject name. If you do not have any data to upload, you can upload data from other [public projects](https://brainlife.io/projects) or import data from the [datasets page](https://brainlife.io/datasets#%7B%22query%22%3A%22%22%2C%22datatypes%22%3A%5B%5D%7D).

!!! How do I use brainlife.io data for my project?
    If you download data from brainlife.io to add to your project, you will need to change the file type. Make sure you have 7-Zip or a similar software on your computer. Once you download the data you want, right-click on the file and hover over "7-Zip" and then click "Extract." Click "OK." That will extact the data in a GZ file, which you can then upload into your brainlife.io project!
    
You will see a confirmation that your data has been uploaded to brainlife.io. If everything looks good, click `Archive`.

![update validate](/docs/img/upload.validate.png)

Now you can see the details of your data in your Archives tab. The `Archived in` field shows where your data is archived (it may take a large data-object longer to be archived, so please give it a few minutes). You will be able to make small edits to this profile, such as adding details about the data and tags or editing the metadata. 

![data-object](/docs/img/dataset.png)

## Visualize Data-objects

To launch a visualization program, click your data-object record (not the checkbox) to open the details box. Then click on the visualizer icon (:fa-eye:) in the upper righthand corer of the box.

Any data-objects stored in brainlife.io can be visualized using web-based apps or using Apps registered for each datatype. 

Once you click the visualizer icon, you will see all of the apps your data-object can be visualized with, which you can see in the example below. Web-based apps have a black bar and brainlife.io registered Apps have a green bar (you can also read which is which in the descriptions). 

![viewers](/docs/img/viewers.png)

!!! note
    If you are wondering how you can develop and contribute new visualization Apps to run on brainlife.io, email us at [brlife@iu.edu](mailto:brlife@iu.edu). You can also let us know if there is a particular app you would like us to add!

To visualize your data -- simply select the App you want to launch! 

## Downloading BIDS

You can search, select, and bulk-download data using BIDS. In the Archive tab, select the data-object you'd like to download by checking the box next to it. A box will appear on the right, where you can select `Download (BIDS)` button.

![download](/docs/img/download.png)

brainlife.io will stage selected data-objects, organize them into a [BIDS structure](http://bids.neuroimaging.io), and let you download the whole structure as a single tar ball. Once it's ready, click `Download`.

!!! note
    At the moment, all Brainlife data-objects will be stored under `/derivatives` directory regardless of the datatype.

## Apps

Before we proceed to `Process` tab, let's take a quick detour and visit the `Apps` page.

![apps](/docs/img/apps.png)

The `Apps` page shows all Brainlife Apps that are publicly available. Please take a look and see what types of Apps are currently available. 

On Brainlife, Apps are normally small programs that perform a specific data processing. Although we have a few Apps that behave more like a typical *pipeline* or *workflow* (including pre/post processing, data analysis, reporting, etc..), most Brainlife Apps should only do one thing, and one thing well. 

### Datatypes

Brainlife Apps exchange data through `datatypes`. Developers involved with interoperating input/output data should discuss and agree on the set of files/directory structure and their semantics, and register a new datatype by submitting an issue on [datatypes github repo](https://github.com/brain-life/datatypes).

![datatype](/docs/img/datatype.png)

Various colored boxes show the input and output datatypes. For example, the above image shows that this app will take `dwi` input data-object, and generate another `dwi` data-object with a datatype tag of "masked", and also output another data-object of a datatype `mask`. 

For more information on datatype, please visit [datatypes page](/docs/user/datatypes)
<!--If you are familiar with an electronic prototyping product called [*littleBits*](https://littlebits.com/tag/prototyping), it is very similar in the concept.-->

## Data Processing

Now that we know what `Apps` are, we can go back to your private project, to practice data processing. Open the `Processes` tab. You should see an empty page as you do not have any processes yet. On Brainlife, `Process` is where you can submit a group of tasks/Apps that can share input/output data-objects. 

We will create a new process by clicking the `+` button at the right bottom corner of the page. Enter any name you would like for your process. You should see a screen that looks like this now.

![empty process](/docs/img/empty.process.png)

To process data, you first need to stage data from our archive to your process. Each process can only process data that is either staged or generated by other Apps. Click `Stage New Data` button to open a dialog, please select `NKI (Rockland Sample)` project, and select any `anat/t1w` data-object.

![select data](/docs/img/select.datasets.png)

Click `OK` to stage. You should see a box showing "Staging Data-object" with selected data. While it is staging your data, please submit your first App. Click `Submit New App` button. A dialog should show up with a list of Apps that you can submit using your `anat/t1w` data-object. Brainlife allows you to select only the Apps where you have all required input data.

Please run the `ACPC alignment with ART` App on your data. `ACPC alignment` is a common alignment tool used to re-orient/re-position the Brain image in common orientation suited for further image analysis. 

Find and click the App, then make sure that Brainlife has automatically selected your staged data as input. Leave other options default. Click `Submit`. Brainlife should now find the most appropriate resource to run this App, and transfer data to the resource and submit it to the local batch scheduler.

Once started, a task should take a few minutes to run. Once completed, you should see a screen that looks like this.

![task success](/docs/img/task.success.png)

You can browse/download any output files as they are generated under `Raw Output` section. Once completed successfully, you can launch various visualization tools by clicking the :fa-eye: button next to the Output section. Open `Volume Viewer` on both the original input data and the ACPC alignment output data and see how this algorithm has re-oriented your data. 

Below is the before/after view. Can you see that bottom one is better aligned/re-positioned at ACPC line? For more info on ACPC alignment see [here](https://github.com/vistalab/vistasoft/wiki/ACPC-alignment)

![acpc orient](/docs/img/acpc.orient.png)

Now that you have finished running ACPC alignment, you will be able to submit a few new Apps under `Submit New App` dialog that you couldn't submit before. Please feel free to submit other Apps or stage more data. 

!!! hint
    If you are not sure which data to stage, please see [Apps](https://brainlife.io/apps) page and find which datatype each App requires to run.

## Archiving

So far, you have staged data, submitted an App that generated data derivatives, and visualized them.

Now, please note that all processes are meant to be temporary and Brainlife will remove processes within 25 days of data generation. (Some resources have even shorter data purging policy). If you would like to permanently keep the output data you just generated, you will need to `archive` them by clicking on :fa-archive: button next to the Output section. You can edit any metadata and description, and click the `Archive` button to archive it.

After you archive your data, open the `Archive` tab and make sure that your data is listed there. You can click on the data-object record to see more details.

## Data-Objects

As you submit more Apps and generate data from them, it becomes harder to keep up with how each data is generated. Brainlife keeps track of a record of how each data-objects were generated (called `data provenance`).

Under `Archive` tab, select any data-object you have just archived. Click `Provenance` tab.

![provenance](/docs/img/provenance.png)

The green boxes are the original input data-objects (uploaded to Brainlife from outside) and the white boxes are the Apps run to generate the data derivatives. You can pan/zoom the diagram, or re-layout some items by dragging/dropping.

## What's Next

You should now be familiar with basic functionalities of Brainlife. Please take a look at other pages for more information. For example, if you'd like to write your app and register on Brainlife, please take a look at [App Developer Guide](/docs/apps/introduction). If you'd like to learn how to bulk process multiple subjects, please take a look at [Pipeline](/docs/user/pipeline).

Please let us know how we can improve this tutorial, or send us pull requests with your edits.
