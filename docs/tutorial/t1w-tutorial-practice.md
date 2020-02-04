!!! warning
    This is a draft

## Anatomical (T1w) preprocessing - Practice & report generation.

Now that you've walked through the Anatomical (T1w) preprocessing, it's time to test your skills!

In this practice, you will be required to combine elements from the Anatomicaly (T1w) preprocessing tutorial and the 'Introduction to brainlife' tutorial. If you have not walked through those, please do that before continuing.

### 1. Create a project to perform your preprocessing.

The first task in this practice is to create a project to perform your preprocessing. Please report the name and link of your project, fill in the details panel, and take a screenshot of the archives page of your project.

### 2. Copying data from at least *three* open projects.

The next task in this practice is to copy the anatomical (neuro/anat/t1w) datatypes from at least *three* open projects on brainlife.io. You will need to archive the data and take a screenshot of the archives page with your archived data.

### 3. Crop & reorient the anatomical data.

The next task in this practice is to crop and reorient the anatomical data in order to prepare it for ACPC alignment and Freesurfer parcellation. This will require staging the data, naming the process, and running the proper app on all of your subjects. Each tab should correspond to a single subject. Please report a screenshot of your processes tab with the staged data, at least *one* process running (i.e. blue header), a completed process (i.e. green header), and images of the pre and post cropping/reorientation from at least *three* subjects.

### 4. ACPC alignment

The next task in this practice is to take the crop and reoriented anatomical data and align it to the ACPC plane. This will require using the output from the crop and reorient app as the input for this step. Please report a screenshot of the pre and post alignment anatomical images from at least *three* subjects.

### 5. Freesurfer parcellation - Generation.

The next task in this practice is to parcellate the ACPC aligned anatomical image using Freesurfer. This will require using the output from the ACPC alignment app as the input for this step. Please report screenshots of the left and right hemisphere pial surfaces, white matter surfaces, and the aparc.a2009s.aseg segmentation with the proper colorLUT from at least *three* subjects.

### 6. Freesurfer parcellation - Statistics.

The final task in this practice is to report the statistics of the parcellation from Freesurfer and perform some simple statistical analyses. This will require using the proper app to report the statistics in .csv files. Please report the following stastics:

1. All subject average and standard deviation for all the parcels (both left and right hemisphere) and measures reported from the app.
    * report these values in a table or .csv
1. Identify the correlation (r) coefficient between the gray matter volumes of the the primary visual area (V1) and the precentral gyrus (M1).
    * report coefficient and plot
1. Identify the correlation (r) coefficient between the vertex numbers of the somatosensory cortex (S1) and the prefrontal cortex.
    * report coefficient and plot
1. Identify the correlation (r) coefficient between the white matter and gray matter volumes of both the left and right hemispheres.
    * report coefficient and plot
