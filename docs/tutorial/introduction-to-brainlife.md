!!! warning
    This is a draft

## Introduction to brainlife.

This page highlights the most important first steps in learning how to use brainlife.io, including creating an account and project, interacting with datasets, launching processes, visualizing results, and creating pipelines. If you have not read the documentation regarding these topics, please visit [https://brainlife.io/docs/](https://brainlife.io/docs/).

![landing](/docs/img/brainlife-landing-page.png)

### A. Create an account.

The first step to processing data using brainlife is to go to brainlife.io and create an account.

<!--
<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/sV55YSCJd-k" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
-->

To do make an account and project, follow these steps:

1. Enter brainlife.io into your browser web address. 
1. Once on the homepage, click the "signup" button in the top left corner.
    * Enter the following information on the signup page:
        * username
        * email
        * password
        * full name
        * institution/university
        * biography
        * position (undergraduate student)
1. Read the Acceptable Use and Privacy policies and then check the box that says "I have read and consent to brainlife.io Acceptable Use Policy and Privacy Policy" and hit submit

### B. Create a Project.

Next we will start managing some data on the platform by creating a Project.

1. Go to the [brainlife.io home page](brainlife.io) and click 'home.' This will bring you inside the platform.
1. Click the 'Projects' tab on the left side of the screen (shield icon).
1. Click the '+' button at the bottom right corner of the screen.
    * Enter the following information on the My Project page:
        * name
        * description
        * Public access policy
        * In 'administrators', add the following accounts (if you start typing a name, the system will autocomplete for you):
            * Brad Caron
            * Soichi Hayashi
            * Franco Pestilli
    * Click submit.
1. Return to the projects page and select your newly created project.

You are now ready to copy data from another project!

### C. Copy data from another project.

The next step will be to copy data into your project. For that, we will use open data from another project.

To copy data from an open project follow the steps below.

1. Go to the main page of your project.
1. Click on the 'Processes' tab.
1. Once inside the Processes tab, click the '+' button at the bottom-right of the screen. 
1. Add a new process and name it 'temp copy data' in the projects description tab.
![Processes Page](/docs/img/projects.process.new.png)

1. Select the newly created process and hit the 'Stage Data' button at the bottom right corner of the screen.  
    * In the 'From' section, select the 'IU LAB IN COGNITIVE AND COMPUTATIONAL NEUROSCIENCE - Demo Data' project to pull data from.
    * For "Subject,' enter BC
    * For 'Datatype,' you will select a single anatomical file this is a T1-weighted MRI (T1w). The brianlife.io Datatypes is called: neuro/anat/T1w
    * For 'Data-object,' click the dropdown menu and select the appropriate data. Since there's only one T1, the correct dataset should auto-populate.
    * Hit 'OK.'

![Import Data From Other Project](/docs/img/projects.processes.stagedata.selecteddata.png)
 
The data is now staged and ready for processing!

<!---
To upload your own data, follow the following steps:
1) Select your project from the projects page.
2) Select the 'archive' tab at the top of the screen.
3) On the archive page, click the '+' button at the bottom of the screen.
4) For datatype, choose the specific datatype for each image type (T1, T2, DWI, fMRI):
    * For each datatype, you'll need to choose the appropriate data files and a subject name (i.e. your randomly assigned ID). You can leave the rest of the fields empty.
    * Once you fill this information, hit next and then archive.
The data is now uploaded and archived to your project!
--->

### D. Launch a process, application (app), visualize and archive the results.

The next step is to launch an application (app) in order to process the data. For this, we will process the T1w (anatomical) image staged in C. In this tutorial, we will divide (i.e. segment/parcellate) the T1 anatomical image using Freesurfer.

<!--
<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/u9Qlh0-iaAk?start=13" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
-->

First we will need to To launch a process using the following steps:

1. On the 'Processes' tab, make sure the process generated in [Part C](https://brainlife.io/docs/tutorial/introduction-to-brainlife/#c-copy-data-from-another-project) is selected.
1. Click on the 'Submit App' button at the bottom right of the screen. This will launch a page with the many apps that can be used on your staged data. 
1. In the searchbar, type 'Freesurfer' and click on the app card. This will open a page with options for choosing which project to save the results and specific input parameters that may affect the outputs of the app.
1. For now, leave all the inputs and parameters as is. We will discuss what these options do in later tutorials.
1. Hit 'OK.'

Once the app is launched, a card will appear on the 'Processes' tab with a blue header. This means the app is currently running, or waiting to run. 
![Blue-header](/docs/img/app-freesurfer-running-blue-header.png)

Once this turns green, that means the app is finished and you can view the results!
![Green-header](/docs/img/app-freesurfer-complete-green-header.png)

To view the results:

1. Click the eye icon next to the output and choose 'Freeview' as your viewer.

This will automatically load important outputs generated by Freesurfer, including cortical and white matter surfaces and a standard parcellation of the cortical surface.

![Freesurfer-output](/docs/img/output-freesurfer-freeview.png)

In order to make sure the generated data does not get deleted over time, we need to save (i.e. archive) the data. Archived data can be viewed and staged via the 'Archive' tab on the Projects page. There are two ways to archive data on brainlife.io: when submitting the and after the app is completed.

To archive the data automatically once the app is finished running, follow these steps:

1. After Step 4 above, click the box next to the option 'Archive all output datasets when finished.' This will save (i.e. archive) the data immediately following completion of processing.

![App-archive](/docs/img/app-freesurfer-archive-data.png)

To archive the data after the app is completed and you've reviewed the results, follow these steps:

1. In the 'Output' section, select the file drawer icon on the far right of the screen.
1. Hit 'OK.'

![Archive-output](/docs/img/archive-freesurfer-outputs.png)

### You're finished! ###

You've now successfully created an account and project, copied data from an open project, and launched an application and processed your first MRI data on brainlife.io!

<!---
### E. Launch pipelines to efficiently and accurately process entire datasets.

The best way to ensure that all data in your project is processed in exactly the same fashion without having to manually launch processes (which is prone to errors) is to use pipelines. Pipelines will launch a process for each participant in your dataset that meets certain criteria (i.e. subject number, datatype) with just a few clicks!

In this tutorial, we will create a pipeline to run Freesurfer on any T1 anatomical images in our Project.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/p2k8mbaG1bY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

To create and launch a pipeline to run Freesurfer, follow the following steps:

1. From the projects page, select your project and select the 'pipelines' tab at the top of the projects page.
1. Once inside the pipelines page, click the '+' at the bottom right of the screen.
1. Give the pipeline a name based on the app you're running (example: Freesurfer).
1. Select the Freesurfer app, and leave all the parameters on the page as is.
1. Hit the 'Submit' button. This will take you back to the pipelines page with an un-activated pipeline.
1. To activate the pipeline, select the button next to 'Offline'. Processes will then start to launch and you can monitor the progress on the pipelines page itself!

You've now submitted a pipeline! Add additional pipelines for all the apps you want to run and process your entire dataset in just a few clicks!
--->

<!---
### F. Create a publication for your data.

Once your data has been processed, the analyses have been ran, and you're ready to publish your data for open use, brainlife.io allows you to create a 'publication' of your data. With this publication comes a doi link that you can include in your eventual manuscript in order to direct fellow researchers to your dataset.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To create a publication, follow the following steps:
1. From the projects page, select your project and select the 'publications' tab at the top of the projects page.
2. In the publications page, click the '+' at the bottom right of the screen.
3. Give your project a title and description, and add your funding sources and contributors.
4. Select the 'Add new release' button at the bottom of the page to add the data to your publication.
    * Give your release a name and date, as you can make multiple releases if, for example, you run new processing apps following your original publication.
    * Click the 'Add datasets' button in the 'Releases' area and select the datasets you'd like to release. Hit OK.
5. Once you're happy with the information you've inputted and the data, hit 'Submit'. You will now be able to view your dataset as 'publication' and send the doi link to anyone you'd like!

You're done! You've now created a project, uploaded, staged, and archived data, launched proceses and visualized results, and create pipelines and publications. You're now a brainlife.io pro!!!
--->
