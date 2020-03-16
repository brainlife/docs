!!! warning
    This is a draft

## Functional MRI (fMRI) preprocessing.

This page demonstrates common steps used to preprocess functional magnetic resonance imaging (fMRI) data on brainlife.io The goal of this tutorial is to show you how to process functional data for successive analyses, including **functional region analysis** and **functional connectivity** (you'll learn more about both below). This tutorial will be using [fMRIPrep](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/) for all anatomical and fMRI processing. We'll be looking at [fMRIPrep - Volume Output](https://brainlife.io/app/5c61c69f14027a01b14adcb3).

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of fMRIPrep involves processing the anatomical images. There are a few common issues and **artifacts**, or anomalies visible in the image but not actually present in the brain, that occur in anatomical images that come straight from the fMRI scanner. One of the most noticeable artifacts, which is detrimental to processing, is **signal inhomogeneity**. This is when the signal from certain portions of the brain, typically the central part, are brighter than the other regions of the brain. Signal inhomogeneity is caused by how the scanner reconstructs the anatomical image. The fMRIPrep app can automatically fix this signal inhomogeneity for us! fMRIPrep will also remove non-brain material from images and align images and surfaces to a standard template (i.e. MNI). Before we can begin using fMRIPrep, however, we must first generate cortical and white matter surfaces from the anatomical images using Freesurfer. These surfaces will be used by fMRIPrep to help correct artifacts and issues with fMRI data and to map functional data.

### 2. Functional preprocessing.

The second step of fMRIPrep involves processing the functional (fMRI) images. These can include both task-related (a person performing a task in the scanner) or resting-state (a person does no task in the scanner) fMRIs. Like any image directly from the scanner, there are common artifacts and issues with fMRI data. One of the biggest problems is **motion**. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. fMRIPrep will estimate the motion that occurred across all of the fMRI volumes and correct for it. This will align all of the volumes perfectly to ensure motion is not altering the data. 

Another common artifact in fMRI data is **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition -- fMRIPrep will also automatically correct for this! 

fMRIPrep can additionally fix the lag in the BOLD signal and the fact that we need to acquire data in non-contiguous slices. This is referred to as **slice-timing**, or the order in which slices are acquired. Without fMRIPrep, our data would not be ordered properly and we wouldn't have contiguous brain slices.

Finally, images from the fMRI scanner might not be perfectly aligned to images collected before it, such as the anatomical images. To fix this, we need to register the functional volumes to the anatomical image and map the fMRI signal onto the surfaces generated in the anatomical preprocessing step -- fMRIPrep does this registration automatically!

Useful information about fMRIPrep anatomical preprocessing can be found in this [original Nature paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/#S13title).
  
There are two versions of the [brainlife.io](https://brainlife.io) fMRIPrep app. One generates outputs mapped to the volumes ([fMRIPrep-volume](https://brainlife.io/app/5c61c69f14027a01b14adcb3)) and the other generates outputs mapped to the surfaces ([fMRIPrep-surface](https://brainlife.io/app/5dfceebd32bff0640ce27bbd)).

For this tutorial, we will use the volume-based version.

<!---
### 3. Functional connectivity network matrices generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating **functional connectivity matrices**! This is done by examing the fMRI BOLD activity in multiple regions across the brain by correlating the regions' BOLD activity throughout the entire acquisition. The reason we do this is because regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (node) with every other region (node) in the brain is with a **network matrix** -- note that **nodes** represent the brain regions here. Each point in the network matrix represents the correlation of BOLD activity between one region and another. We can then use these network matrices to examine properties of the network that describe how interrelated specific regions in the brain are working during the fMRI acquisition in either task-related or resting-state fMRIs.

There is a [brainlife.io](https://brainlife.io) app for generating these matrices that we will use in this tutorial.

| ![conmat](/docs/img/app-fmri-to-conmat.bl.header.png) |
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.167 |
-->

Now, let's get to work! The following steps of this tutorial will show you how to:
1. generate anatomical surfaces using Freesurfer, 
2. preprocess the anatomical (T1w & T2w) and fMRI data using fMRIPrep 

<!---
3. and, map the Glasser 180-node atlas to the anatomical (T1w) image
4. and generate network matrices from the regions of the Glasser 180-node atlas.
-->

### Copy appropriate data over from a single subject in the InterTVA project

1. Click the following link to go to the project's page for the 'InterTVA' project: https://brainlife.io/project/5c8415aa34225c0031027372
1. Click the 'Archive' tab at the top of the screen to go to the archive's page.
1. Select the following datatypes from one subject by clicking the boxes next to the data:
    * func/task rest
    * anat/t1w
    * anat/t2w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project', select your project from the drop-down menu.
    * For 'Process', select 'Create New Process' and title it "fMRI Prep Tutorial". Hit 'Submit'.
        * This will take you to the process on your Project's page
1. Archive the data in your project by clickin the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: generate Freesurfer surfaces!

### Generate surfaces using Freesurfer:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer.'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w & T2w) images by clicking the drop-down menu and finding the appropriate datasets.
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
            
Once you're happy with the surfaces, you can move onto running fMRIPrep!

### Preprocess your data with fMRIPrep:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Volume Output'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w & T2w) images, the freesurfer output, and the functional data by clicking the drop-down menu and finding the appropriate datasets.
    * For 'space,' select 'MNI152NLin6Asym' from the drop-down menu.
    * For 'resolution,' select 'original' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmriPrep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon next to the 'html' tagged output.
    * Choose the 'html' viewer

**If you're happy with the results, then you have successfully finished preprocessing your fMRI data with fMRIPrep! You're now ready to move onto the next tutorial: functional network connectivity!**

<!---
### Map the Glasser 180-node atlas:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Multi-Atlas Transfer Tool'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged Freesurfer output by clicking the drop-down menu and finding the appropriate dataset.
    * For 'space,' select 'fsaverage6' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmriPrep'
    * Hit 'Submit'

Once the app is finished, you're ready to move onto the final step: network matrix generation!

### Generate functional connectivity network matrices:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'fMRI to Connectivity Matrices'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the generated bold mask, preprocessed functional data, the Glasser parcellation-volume, and the preprocessed regressors by clicking the drop-down menu and finding the appropriate datasets.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'connectivity_matrix'
    * Hit 'Submit'
    
Nice work! You've completed this tutorial. Now that the app is finished, you're ready to perform group analyses on your connectivity matrices!
-->
