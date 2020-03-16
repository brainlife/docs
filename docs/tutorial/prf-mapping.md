!!! warning
    This is a draft

## Population receptive field mapping.

This page demonstrates common steps used to map population receptive field (pRF) measures to the occipital cortex. This tutorial will walk through ways to [automatically map pRF data based on an atlas](https://brainlife.io/app/5cc4cd3f4ed9df00317f621d) and to [map fMRI pRF data](https://brainlife.io/app/5afc9831322997002773ed1c).

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) and the [Anatomical Preprocessing tutorial](https://brainlife.io/docs/tutorial/t1w-preprocessing/)) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of pRF mapping often involves processing the anatomical images. In order to automatically map pRF measures using an atlas, or to map functional MRI pRF data, we first need to generate cortical and white matter surfaces, and brain region parcellations, using [Freesurfer](https://brainlife.io/app/58c56d92e13a50849b258801). These will be used map pRF data to the occipital lobe surface.

### 2. pRF Mapping.

The next step is to map **population receptive field (pRF)** data to the cortical occipital surface. **pRF** is a method for mapping visual function in the occipital lobe -- i.e. the visual center of the brain -- using fMRI. Specifically, visual stimuli including rotating wedges and scanning bars are used to map functional activitation of the occipital lobe based on location of the stimuli in the visual field and the orientation of the stimuli. This works due the specific preference for specific orientations and the preservation of visual field locations in the visual cortex (i.e. **retinotopy**). Every location in the visual field has a corresponding retinal and occipital cortex location and a prefered orientation of stimuli. Because of this, we can map visual field stimuli and the reaction of the occipital lobe to these different stimuli.

There are two ways in which one could map this visual function data to the cortex: the first involves the [fitting of an atlas](https://brainlife.io/app/5cc4cd3f4ed9df00317f621d) to the occipital lobe, and the other involves [fitting fMRI pRF data](https://brainlife.io/app/5afc9831322997002773ed1c) to the occipital lobe. On brainlife.io, we have developed apps to perform both of these tasks. This tutorial will walk you through both methods.

Useful information about pRF can be found in this [Neuroimage](https://pubmed.ncbi.nlm.nih.gov/17977024-population-receptive-field-estimates-in-human-visual-cortex/) paper. More information about the atlas-based mapping can be found in this [PLOS Computational Biology](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003538) paper. More information about the method we've implemented for mapping fMRI-based pRF data can be found in this [
  
There are two versions of the [brainlife.io](https://brainlife.io) fMRIPrep app. One generates outputs mapped to the volumes ([fMRIPrep-volume](https://brainlife.io/app/5c61c69f14027a01b14adcb3)) and the other generates outputs mapped to the surfaces ([fMRIPrep-surface](https://brainlife.io/app/5dfceebd32bff0640ce27bbd)).

For this tutorial, we will use the volume-based version.

Now, let's get to work! The following steps of this tutorial will show you how to:
1. generate anatomical surfaces using Freesurfer, 
2. preprocess the anatomical (T1w & T2w) and fMRI data using fMRIPrep 

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
