!!! warning
    This is a draft

## Introduction to brainlife.

This page highlights the most important first steps to using brainlife.io, including creating an account and project, interacting with datasets, datatypes, and applications, uploading data, launching processes, visualizing results, and creating pipelines and publications. If you have not read the documentation regarding these topics, please visit https://brainlife.io/docs/.

![crop-reorient](/docs/img/brainlife-landing-page.png)

### 1. Create an account and project.

The first step to processing data using brainlife is to go to brainlife.io and create an account and project.

<iframe width="560" height="315" src="https://www.youtube.com/embed/86CbNUrkd8s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To do make an account and project, follow the following steps:
1) Enter brainlife.io enter your browser web address. 
2) Once on the homepage, click the 'signup' button in the top left corner.
    * Enter the following information on the signup page:
        * username
        * email
        * password
        * full name
        * institution/university
        * biography
        * position (undergraduate student)
    * Read the usage policy and select the 'Agree to our Acceptable Use Policy' box and hit submit
3) Once signed up and logged in, select the 'home' button on the home page
4) Click the 'Projects' tab on the left side of the screen (shield icon)
5) Click the '+' button at the bottom right corner of the screen.
    * Enter the following information on the My Project page:
        * name
        * description
        * Public access policy
        * In 'administrators', add the following accounts:
            * Brad Caron
            * Soichi Hayashi
            * Franco Pestilli
    * Click submit.
6) Return to the projects page and select your newly created project.

You're now ready to upload/archive/stage data!

### 2. Upload and archive data to project.

The next step to using brainlife.io is to archive data to your project. This can be done in two ways: 1) upload your own data or 2) use existing data from an open project.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If you do not have your own data, archive data in your project from the class demo project, follow the following steps:
1) Select your project from the projects page.
2) Select the 'processes' tab at the top of the screen.
3) On the processes page, click the '+' button at the bottom of the screen. Name the process 'my demos' in the projects description tab.
4) Select the newly created process and hit the 'Stage Data' button at the bottom right corner of the screen.
    * For from, select the 'IU LAB IN COGNITIVE AND COMPUTATIONAL NEUROSCIENCE - Demo Data' project to pull data from.
    * For subject, enter BC (Brad Caron's initials. This is his brain)
    * For datatype, you'll select the following datatypes one at a time. You'll have to do this process three times:
        * DWI (x2): PA and AP
        * T1
    * For data-objects, select the data object that pops up. For DWI, you'll need to select the PA and AP images separately.
    * Hit OK.

The data is now staged and ready for processing!

To upload your own data, follow the following steps:
1) Select your project from the projects page.
2) Select the 'archive' tab at the top of the screen.
3) On the archive page, click the '+' button at the bottom of the screen.
4) For datatype, choose the specific datatype for each image type (T1, T2, DWI, fMRI):
    * For each datatype, you'll need to choose the appropriate data files and a subject name (i.e. your randomly assigned ID). You can leave the rest of the fields empty.
    * Once you fill this information, hit next and then archive.

The data is now uploaded and archived to your project!

### 3. Launch a process, application (app), and visualize the results.

The next step is to stage data and launch a process in order to process the data.

<iframe width="560" height="315" src="https://www.youtube.com/embed/u9Qlh0-iaAk&t=16s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If you do not have your own data and staged data from the demo project, skip to step 4.
To launch a process, follow the following steps:
1) From the projects page, select your project and select the 'archive' tab at the top of the projects page.
2) In the archive page, select the datasets you would like to stage and process. It is usually best to select all the appropriate data for a single subject and process everything on one process.
3) Once the data is selected, hit the 'Stage to process' button on the right side of the screen. Make sure the 'project' field is your project and 'Create new process' is selected. Give a description for your process (example: "BC subject processing"). Hit submit.
4) In the processes tab, select the newly created process and hit the 'Submit App' button at the bottom right of the screen. This will launch a page with the many applications (apps) that can be used on your staged data. Select the app you would like to run and follow the prompts to launch the app.
5) Follow the prompts and enter the appropriate information for the app. Make sure to select the 'Archive all output datasets when finished' box to archive the generated data.
6) Once the app is launched, a new box on the right side of the processes page will open and turn blue. Once this turns green, that means the app is finished and you can visualize the generated data.
7) To view the data, select the eye icon next to the output dataset of interest and choose a viewer. For most datasets, 'fsleyes' will be the best viewer to use.
    * If you're not satisfied with the results, you can always delete the archived data and launch the same app with different input parameters.

### 4. Launch pipelines to efficiently and accurately process entire datasets.

The best way to ensure that all data in your project is processed in exactly the same fashion without having to manually launch processes (which is prone to errors) is to use pipelines. Pipelines will launch a process for each participant in your dataset that meets certain criteria (i.e. subject number, datatype) with just a few clicks!

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ewy3ahCVUzw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To create and launch a pipeline, follow the following steps:
1) From the projects page, select your project and select the 'pipelines' tab at the top of the projects page.
2) In the pipelines page, click the '+' at the bottom right of the screen.
3) Give the pipeline a name based on the app you're running (example: ACPC alignment) and select that app to run.
4) Select the appropriate inputs and parameters and hit the 'Submit' button. This will take you back to the pipelines page with an un-activated pipeline.
5) To activate the pipeline, select the button next to 'Offline'. Processes will then start to launch and you can monitor the progress on the pipelines page itself.

You've now submitted a pipeline! Add additional pipelines for all the apps you want to run and process your entire dataset in just a few clicks!

### 5. Create a publication for your data.

Once your data has been processed, the analyses have been ran, and you're ready to publish your data for open use, brainlife.io allows you to create a 'publication' of your data. With this publication comes a doi link that you can include in your eventual manuscript in order to direct fellow researchers to your dataset.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To create a publication, follow the following steps:
1) From the projects page, select your project and select the 'publications' tab at the top of the projects page.
2) In the publications page, click the '+' at the bottom right of the screen.
3) Give your project a title and description, and add your funding sources and contributors.
4) Select the 'Add new release' button at the bottom of the page to add the data to your publication.
    * Give your release a name and date, as you can make multiple releases if, for example, you run new processing apps following your original publication.
    * Click the 'Add datasets' button in the 'Releases' area and select the datasets you'd like to release. Hit OK.
5) Once you're happy with the information you've inputted and the data, hit 'Submit'. You will now be able to view your dataset as 'publication' and send the doi link to anyone you'd like!

You're done! You've now created a project, uploaded, staged, and archived data, launched proceses and visualized results, and create pipelines and publications. You're now a brainlife.io pro!!!
