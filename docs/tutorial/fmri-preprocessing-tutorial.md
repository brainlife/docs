!!! warning
    This is a draft

## Functional MRI (fMRI) preprocessing.

This page demonstrate common steps used to preprocess functional magnetic resonance imaging data (fMRI) on brainlife.io The goal of this turorial is to show how to process functional data for successive analyses – functional region analysis and functional connectivity. This tutorial will be using fmriPrep (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/) for all anatomical and fMRI processing (https://brainlife.io/app/5c61c69f14027a01b14adcb3; https://brainlife.io/app/5dfceebd32bff0640ce27bbd).

This tutorial will use a combination of skills developed in the introduction-to-brainlife tutorial (https://brainlife.io/docs/tutorial/introduction-to-brainlife/). If you have not read this, or you are not comfortable staging, processing, archiving and viewing data on brainlife.io, please go back and follow that tutorial before beginning this one.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 1. Anatomical preprocessing.

The first step of fmriPrep involves processing the anatomical images. These can include both T1w and T2w images. fmriPrep will correct for signal inhomogeneities (i.e. intensity banding), remove non-brain material, generate cortical and white matter surfaces using Freesurfer, and align images and surfaces to a standard template. 

### 2. Functional preprocessing.

The second step of fmriPrep involves processing the functional (fMRI) images. These can include both task-related (i.e. person performing a task in the scanner) or resting-state (i.e. person does no task in the scanner). fmriPrep will estimate and correct head motion across all of the fMRI volumes, perform slice-timing correction (i.e. reorder images based on when the slice was acquired), correct for field-related susceptibility distortions, register functional volumes to the anatomical image, and map the fMRI signal onto the surfaces generated in step 1.

Useful information about the fmriPrep anatomical preprocessing can be found in the original Nature paper: 
  - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/#S13title
  
There are two versions of the [brainlife.io](https://brainlife.io) app to fmriPrep. One generates outputs mapped to the surfaces, and the other generates outputs mapped to the volumes:

| ![fmriprep-volume](/docs/img/app.fmriprep-volume-header.png) |
| ![fmriprep-surface](/docs/img/app.fmriprep-surface-header.png) |
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.267 |
| https://doi.org/10.25663/brainlife.app.160 |

For this tutorial, we will use the surface-based version.

To perform fmriPrep, follow the following steps:

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
1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Surface Output'.
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

If you're happy with the results, then you have now finished preprocessing your fMRI data with fMRIPrep! You are now ready to move onto the next tutorial: population receptive field mapping!
