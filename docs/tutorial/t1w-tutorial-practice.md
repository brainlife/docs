!!! warning
    This is a draft

## Identify the effects of concussion on the volumes of brain tissue - Anatomical Preprocessing.

As a structural neuroimager and neuroscientist, I am interested in how structural properties of the brain relate to behavior or disease. For example, I am interested in how concussions may affect the volume of the motor cortex, the primary visual cortex, the hippocampus, and prefrontal cortex and how that relates to behavior. I want to know: 1) how concussions affect the volume of these structures, 2) are different tissue types affected differentially from concussions, and 3) how these alterations relate to behavior. In order to answer these questions, I need to compare measures derived from concussed athletes with a healthy control population in order to identify any concussion-specific effects on brain tissue. These questions are important to understanding the effects of concussions and may be useful in identifying markers that can predict whether or not a person has a concussion.

To answer these questions, I will need a specific type of MRI image: an anatomical T1w image. I then need to preprocess this data in a specific way in order to measure the volumes of the structures of interest in an accurate and reliable way.

Luckily, brainlife.io has all the tools and data necessary to answer these questions! I also have a large team of researchers (i.e. you) to help me with the processing and analyses!

I will ask you to perform a number of tasks on healhty, non-concussed subjects and report some quality-assurance (QA) images while I will work on the concussed athletes! I will also have you generate some simple statistics of these healthy, non-concussed subjects.

This will require you to combine elements from the 'Anatomical (T1w) preprocessing' tutorial and the 'Introduction to brainlife' tutorial. If you have not walked through those, please do that before continuing.

### 1. Create a project to perform your preprocessing.

The first task for any processing on brainlife.io is to create a project where you can track and monitor your processes. You will generate your own project that you'll use to preprocess the healthy subjects.

To complete this task, do the following:

1. Create a project for your preprocessing.
    * Make sure to fill in the 'Detail' page with the project description
    * Report a screenshot of the completely filled out project page
    * Provide the link to your projects page

We are now ready to start copying over the anatomical data from healthy, non-concussed subjects!

### 2. Copying data from at least *three* open projects.

The next step for any processing on brainlife.io is to archive data to your project. You will copy over at least anatomical (T1w) images from **10** healthy, non-concussed subjects from at least **3** different open projects on brainlife.io.

To complete this task, do the following:

1. Copy **10** healthy, non-concussed subjects from at least **3** different open projects.
    * Make sure to archive the data.
    * Report a screenshot of the 'Archive' tab on the project page with the archived data.
    
We are now ready to start processing our data!

### 3. Crop & reorient the anatomical data.

The first task in processing the anatomical (T1w) images is to make sure the images are in the proper orientation. You will crop and reorient the anatomical (T1w) images from the 10 healthy, non-concussed subjects you copied over. This will prepare the data for the next steps in anatomical preprocessing.

To complete this task, do the following:

1. Stage the anatomical data from each subject to the 'Processes' tab.
    * Make sure to stage each subject into it's own process with the name of the subject as the name of the process.
    * Report a screenshot of *three* subject's processes once the data has completed staging.
1. Run the crop & reorient app.
    * Make sure to archive the data upon completion.
1. View the results
    * Report screenshots of **3** subject's pre- and post-cropped/reoriented anatomical images.

Once this process is complete, we are now ready to align our images to a standard anatomical plane!

### 4. ACPC alignment

The next task in processing anatomical (T1w) images is to align the cropped and reoriented images to an anatomical plane (ACPC) that makes later processing much more consistent. You will align the cropped and reoriented images from the **10** healthy, non-concussed subjects you generated to the ACPC plane. This will prepare the data for the next steps in anatomical preprocessing.

To complete this task, do the following:

1. Run the ACPC alignment app.
    * Make sure to use the cropped and reoriented output from 3.
    * Make sure to archive the data upon completion.
1. View the results
    * Report screenshots of **3** subject's pre- and post-ACPC aligned anatomical images.
    
Once the processing is complete, we are now ready to generate our structures of interest and measure their volumes!

### 5. Freesurfer parcellation - Generation.

The next task in processing anatomical (T1w) images is to parcellate the ACPC aligned anatomical image using Freesurfer. This will generate the many structures we're interested in, including cortical and white matter surfaces, the hippocampus, the motor cortex, the prefrontal cortex, and the primary visual cortex and compute the volume of these structures. You will generate these structures and measures using the ACPC aligned images. 

To complete this task, do the following:

1. Run the Freesurfer parcellation app.
    * Make sure to use the ACPC aligned output from 4.
    * Make sure to archive the data upon completion.
1. View the results
    * Report screenshots of the following surfaces from **3** subjects:
        * Left and right hemisphere cortical (pial) surface
        * Left and right hemisphere white matter surface
        * Aparc.a2009s.aseg segmentation with the proper colorLUT loaded
    
Now the fun begins! We can now start performing some statistical analyses and ask some scientific questions!

### 6. Freesurfer parcellation - Statistics.

The final task in this practice is to report the statistics of the parcellation from Freesurfer and perform some simple statistical analyses on the healthy, non-concussed subjects. This will require using the proper app to report the statistics in .csv files. In order to answer our questions of interest, we will need to generate the following statistics:

1. The average (and standard deviation) volume of all the cortical structures across our healhty, non-concussed subjects.
    * Please report these values in a table or .csv
1. The correlation coefficient (r) between the total gray- and white-matter volumes.
    * Please report coefficient coefficient and an image of the correlation plot.
1. The correlation coefficient (r) between the cortical volumes of the primary visual area (V1) and the precentral gyrus (M1; motor cortex).
    * Please report the correlation coefficient and an image of the correlation plot.
1. The correlation coefficient (r) between the cortical volumes of the hippocampus and the prefrontal cortex (PFC).
    * Please report the correlation coefficient and an image of the correlation plot.
1. The correlation coefficient (r) between the white matter and gray matter volumes of both the left and right hemispheres.
    * Please report the correlation coefficient and an image of the correlation plot.
    
Once we know these statistics, we can compare the averages and correlations with concussed athletes to identify the effects of concussion on the volume of brain tissue!
