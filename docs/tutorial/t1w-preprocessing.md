!!! warning
    This is a draft

## Anatomical (T1w) preprocessing.

This page demonstrate common steps used to preprocess anatomical magnetic resonance imaging data (T1-weighted or T1w) on brainlife.io The goal of this turorial is to show how process anatomical data for successive analyses – volumetric analyses from T1w meausres, combination of T1w and diffusion-weighted MRI (dMRI) or functional neuroimaging data (fMRI) pipelines.

This tutorial will use a combination of skills developed in the introduction-to-brainlife tutorial (https://brainlife.io/docs/tutorial/introduction-to-brainlife/). If you have not read this, or you are not comfortable staging, processing, archiving and viewing data on brainlife.io, please go back and follow that tutorial before beginning this one.

### 1. Set the orientation of the brain.

The first step is to make sure the anatomy of the brain 'looks right.' This means that the coordinate system for the T1w image is stored rotated such that the brain appears up-right and centered when opened in the major visualization toolboxes that the scientific community uses (i.e., generally we look at images where the nose faces towards right-hand side in the sagittal plane, and forward in the axial and coronal planes). This rotation involves changing information in the T1w data files and headers. Beside agreeing to the standard convetions, this orientation is important because it assures that the data inside the T1w iamges will be "indexed,"  or read, appropirately allowing different software libraries for neuroimaging data analysis to  read the data, interpret the position and dimensions of individual three-dimensional pixels (i.e., voxels). During this process oftentimes the neck is removed from the original MRI data, so to fit the image within an adequate volume. This can be done on brainlife.io using the [Crop & Reorient T1](https://brainlife.io/app/5a0f38bf31769f1e4d46cc58) app.

Useful information about MRI image orientation can be found at: 
  - https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Orientation%20Explained 
  - http://gru.stanford.edu/doku.php/mrTools/coordinateTransforms

### 2. Anterior Commissure - Posterior Comissure (ACPC) alignment.

The next step is to align the anatomical image to the point where two white matter tracts cross hemispheres: the anterior commissure and posterior commissure (ACPC). The anterior commissure connects the two temporal lobes and is located below the anterior portion of the corpus callosum (i.e. fornix), the large white matter tract connecting the two hemispheres. The posterior commissure is located just behind the cerebral aqueduct, an important connection between the third and fourth ventricles. This step will move the image in a way that effectively "centers" the image. Specifically, the image will be moved so that a horizontal line drawn from the front to the back of the brain will pass through both landmarks, and a horizontal line drawn from the temporal lobes and a vertical line from the top of the brain to the bottom bisect at the midpoint of the two landmarks. A majority of the following processing steps require the anatomical image to be aligned to the ACPC plane, including Freesurfer parcellation. This can be done on brainlife.io using the [HCP ACPC Alignment (T1w)](https://brainlife.io/app/5c61c69f14027a01b14adcb3) app.

### 3. Freesurfer Brain Parcellation - Generation.

The next step is perform anatomical preprocessing and divide (i.e. parcellate) the brain into different regions and tissue types (i.e. white matter, gray matter) using Freesurfer. These regions represent different gray matter (i.e. cortical) landmarks such as the motor and somatosensory cortices, and are typically derived from histological properties of these regions. Sometimes, they are derived from functional activation (i.e. fMRI) patterns. Freesurfer will divide the anatomical image into different tissue types (i.e. white matter, gray matter), parcellate the gray matter into known anatomical landmarks, and output statistics regarding the volume and thickness of the different tissue types and anatomical landmarks. These regions are then used for a large number of downstream steps, including white matter tract segmentation. Information regarding the volume and thickness of each region can also be used for group analyses. This can be done on brainlife.io using the [Freesurfer](https://brainlife.io/app/58c56d92e13a50849b258801) app.

### 4. Freesurfer Brain Parcellation - Statistics.

The final step is to take the parcellation generated from Freesurfer and collate the important statistics into a .csv file for easy viewing and implementation of group analyses. Within each region of the parcellation (i.e. ROI), Freesurfer computes simple statistics regarding the number of vertices, surface area, volume, cortical thickness, mean and gaussian curvatures, and the fold and curvature indices. These statistics can be used to perform group-level analyses, including group differences within ROIs and correlation analyses with behavioral data. This can be done on brainlife.io using the [Freesurfer Statistics](https://brainlife.io/app/5e31bea8b81ce26c2f9c9a88) app.

Now, let's get to work! The following steps of this tutorial will show you how to:

1. Crop and reorient the anatomical (T1w) image,
1. ACPC align the anatomical (T1w) image, 
1. generate cortical and white matter surfaces and parcellations using Freesurfer,
1. obtain common statistics from the Freesurfer parcellation.

### Copy appropriate data over from a single subject in the HCP 3T Diffusion project

1. Click the following link to go to the project's page for the 'HCP 3T Diffusion' project: https://brainlife.io/project/5941a225f876b000210c11e5/
1. Click the 'Archive' tab at the top of the screen to go to the archive's page.
1. Select the following datatypes from one subject by clicking the boxes next to the data:
    * anat/t1w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project', select your project from the drop-down menu.
    * For 'Process', select 'Create New Process' and title it "anatomical (T1w) Prep Tutorial". Hit 'Submit'.
        * This will take you to the process on your Project's page
1. Archive the data in your project by clickin the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: crop and reorient the T1w image!

### Crop and reorient anatomical (T1w) image.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Crop & Reorient T1'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w) image by clicking the drop-down menu and finding the appropriate dataset.
    * Select the boxes for 'crop' and 'reorient'
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'cropped_reoriented'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
    
Once you're happy with the alignment, you can move onto ACPC aligning the cropped & reoriented anatomical data!

### ACPC-align anatomical (T1w) image.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'HCP ACPC Alignment (T1w)'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the cropped & reoriented anatomical (T1w) image generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For template, choose 'MNI152_1mm' by clicking the drop-down menu and finding the appropriate file
    * For reorient, make sure the box is checked.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'acpc_aligned'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
    * Only have the file titled 'out.nii.gz' selected in the viewer

Once you're happy with the alignment, you can move onto Freesurfer parcellation generation!

### Freesurfer Brain Parcellation - Generation.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer.'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the ACPC aligned anatomical image generated above by clicking the drop-down menu and finding the appropriate datasets.
    * Select the boxes for 'hippocampal' and 'hires'
    * For 'version,' select '6.0.0' from the drop-down menu.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'freesurfer'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'freeview' as your viewer
        * This will load the following volumes and surfaces: aseg, brainmask, white matter mask, T1, left/right hemisphere pial (cortical), and white (white matter) surfaces.
    * To view the aparc.a2009s segmentation on an inflated surface, do the following:
        * Click File --> Load surface
            * Choose the lh.inflated and rh.inflated surfaces
            * Hit 'OK'
        * Select inflated surface of choice (i.e. left or right hemisphere)
        * Click the drop-down menu next to 'Annotation' and choose 'Load from file'
            * Choose the appropriate hemisphere aparc.a2009s.annot file (lh.aparc.a2009s.annot)
            * Hit 'OK'
            * The aparc.a2009s parcellation should be overlayed on your inflated surface! Now, repeat the process on the other hemisphere.
            
Once you're happy with the surfaces, you can move computing statistics!

### Freesurfer Brain Parcellation - Statistics.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer.'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the Freesurfer output generated in Step 3 by clicking the drop-down menu and finding the appropriate dataset (look for the datatag 'freesurfer'.
    * For parcellation, please choose the parcellation of your choice. For this tutorial, we will use the 'aparc.a2009s'. To choose this, select the 'aparc.a2009s' option from the drop-down menu.
    * Select the box for 'Archive all output datasets when finished
        * For 'Dataset Tags', type and enter 'freesurfer-stats'
    * Hit 'Submit'
    
To view the results, download the data by clicking the 'download' button next to the dataset. Then open the 'lh' and 'rh' csv files into your favorite spreadsheet of choice (i.e. excel) and you're ready to start your group analyses!

You have now finished preprocessing your anatomical (T1w) image! You are now ready to move onto the next tutorial: function MRI (fMRI) preprocessing!
