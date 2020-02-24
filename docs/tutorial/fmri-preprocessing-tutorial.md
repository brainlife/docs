!!! warning
    This is a draft

## Functional MRI (fMRI) preprocessing.

This page demonstrate common steps used to preprocess functional magnetic resonance imaging data (fMRI) on brainlife.io The goal of this turorial is to show how to process functional data for successive analyses – functional region analysis and functional connectivity. This tutorial will be using fmriPrep (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/) for all anatomical and fMRI processing (https://brainlife.io/app/5c61c69f14027a01b14adcb3; https://brainlife.io/app/5dfceebd32bff0640ce27bbd).

This tutorial will use a combination of skills developed in the introduction-to-brainlife tutorial (https://brainlife.io/docs/tutorial/introduction-to-brainlife/). If you have not read this, or you are not comfortable staging, processing, archiving and viewing data on brainlife.io, please go back and follow that tutorial before beginning this one.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 1. Anatomical preprocessing.

The first step of fmriPrep involves processing the anatomical images. There are a few common artifacts and issues with anatomical images straight from the scanner. One of the most noticeable, and detrimental to processing, is signal inhomogeneity. Signal inhomogeneity is when the signal from certain portions of the brain (usually central) are brighter than the other regions of the brain. This is due to how the scanner reconstructs the anatomical image. fMRIPrep will do correct for this signal inhomogeneity automatically for us! fMRIPrep will also remove non-brain material and align images and surfaces to a standard template (i.e. MNI). Before performing fMRIPrep, however, we must first generate cortical and white matter surfaces from the anatomical images using Freesurfer.

### 2. Functional preprocessing.

The second step of fmriPrep involves processing the functional (fMRI) images. These can include both task-related (i.e. person performing a task in the scanner) or resting-state (i.e. person does no task in the scanner). Like any image directly from the scanner, there are common artifacts and issues with fMRI data. One of the biggest is motion. For long scanning "runs", it is nearly impossible for a participant to lie completely motionless. When sublte movements do occur, this can alter the results in a specific region and make your data less accurate. fMRIPrep will estimate the motion that occurred across all of the fMRI volumes and correct for it. This will make all of the volumes perfectly aligned to one another and ensure motion is not altering the data. Another common artifact is susceptibility distortions. This is when regions of the brain look 'wavy' or fold inwards or outwards on itself. This is due to how the scanner sweeps across the entire brain during acquisition. fMRIPrep will also automatically correct for this! The final issue with fMRI occurs due to the lagginess of the BOLD signal and the need to acquire data in non-contiguous slices. This is referred to as slice-timing (i.e. the order in which slices are acquired). Because of this, we need to order the data in the proper way, or your data with not have contiguous brain slices. fMRIPrep also corrects for this automatically! Finally, images from the scanner might not be perfectly aligned to images collected before it (i.e. the anatomical images). Thus, we need to register the functional volumes to the anatomical image, and map the fMRI signal onto the surfaces generated in the anatomical preprocessing step. fMRIPrep does this registration automatically!

Useful information about the fmriPrep anatomical preprocessing can be found in the original Nature paper: 
  - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/#S13title
  
There are two versions of the [brainlife.io](https://brainlife.io) app to fmriPrep. One generates outputs mapped to the volumes, and the other generates outputs mapped to the surfaces:

| ![fmriprep-volume](/docs/img/app-fmriprep-volume-header.png) |
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.160 |

| ![fmriprep-surface](/docs/img/app-fmriprep-surface-header.png) |
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.267 |

For this tutorial, we will use the volume-based version.

### 3. Functional connectivity network matrices generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating functional connectivity matrices! This is done by examing the fMRI BOLD activity in multiple regions across the entire brain by correlating their BOLD activity across the entire acquisition. The logic behind this is that regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (i.e. node) with every other region (i.e. node) in the brain is with a network matrix. Each point in the matrix represents the correlation of BOLD acitivity between one region and another. We can then use these network matrices to examine properties of the network that can describe how interelated specific regions in the brain are during the fMRI acquisition (either task or resting-state).

In this tutorial, we will generate anatomical surfaces using Freesurfer, preprocess the anatomical (T1w & T2w) and fMRI data using fMRIPrep, map the Glasser 180-node atlas to the anatomical (T1w) image, and generate network matrices from the regions of the Glasser 180-node atlas.

### To generate surfaces using Freesurfer, follow the following steps:

1. Go to the 'Archives' page of your project by clicking the 'Archives' tab on your Projects page.
1. Select the anatomical (T1w & T2w) images, the functional data, and the fieldmap datatypes by clicking the box next to the datasets.
1. Click the 'Stage to process'
    * For Project, make sure your project is selected
    * For Process, select 'Create New Process'
    * For Description, name the process 'fmriPrep Preprocessing'
    * Hit 'OK'
1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer'.
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w & T2w) images by clicking the drop-down menu and finding the appropriate datasets.
    * Select the boxes for 'hippocampal' and 'hires'
    * For 'version', select '6.0.0' from the drop-down menu.
    * Select the box for 'Archive all output datasets when finished
        * For 'Dataset Tags', type and enter 'freesurfer'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'freeview' as your viewer
        * This will load the following volumes and surfaces: aseg, brainmask, white matter mask, T1, left/right hemisphere pial (cortical) and white (white matter) surfaces.
    * To view the aparc.a2009s segmentation on an inflated surface, do the following:
        * Click File --> Load surface
            * Choose the lh.inflated and rh.inflated surfaces
            * Hit 'OK'
        * Select inflated surface of choice (i.e. left or right hemisphere)
        * Click the drop-down menu next to 'Annotation' and choose 'Load from file'
            * Choose the appropriate hemisphere aparc.a2009s.annot file (i.e. lh.aparc.a2009s.annot)
            * Hit 'OK'
            * The aparc.a2009s parcellation should be overlayed on your inflated surface! Repeat the process on the other hemisphere!
            
Once you're happy with the surfaces, you can now move onto running fMRIPrep!

### To preprocess your data with fMRIPrep, follow the following steps:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Volume Output'.
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w & T2w) images, the freesurfer output, and the functional data by clicking the drop-down menu and finding the appropriate datasets.
    * For 'space', select 'fsaverage6' from the drop-down menu.
    * Select the box for 'Archive all output datasets when finished
        * For 'Dataset Tags', type and enter 'fmriPrep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'freeview' as your viewer
        * This will load the following volumes and surfaces: aseg, brainmask, white matter mask, T1, left/right hemisphere pial (cortical) and white (white matter) surfaces.

If you're happy with the results, then you have now finished preprocessing your fMRI data with fMRIPrep! You are now ready to move onto the next step in this tutorial: mapping the Glasser 180-node atlas to the T1w image!

### To map the Glasser 180-node atlas, follow the following steps:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Multi-Atlas Transfer Tool'.
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged Freesurfer output by clicking the drop-down menu and finding the appropriate dataset.
    * For 'space', select 'fsaverage6' from the drop-down menu.
    * Select the box for 'Archive all output datasets when finished
        * For 'Dataset Tags', type and enter 'fmriPrep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'freeview' as your viewer
        * This will load the following volumes and surfaces: aseg, brainmask, white matter mask, T1, left/right hemisphere pial (cortical) and white (white matter) surfaces.
