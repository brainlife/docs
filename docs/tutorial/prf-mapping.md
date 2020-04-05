!!! warning
    This is a draft

## Population receptive field mapping.

This page demonstrates common steps used to map population receptive field (pRF) measures to the occipital cortex. This tutorial will walk through ways to [automatically map pRF data based on an atlas](https://brainlife.io/app/5cc4cd3f4ed9df00317f621d) and to [map fMRI pRF data](https://brainlife.io/app/5afc9831322997002773ed1c).

The tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) and the [Anatomical Preprocessing tutorial](https://brainlife.io/docs/tutorial/t1w-preprocessing/) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of pRF mapping often involves processing the anatomical images. In order to automatically map pRF measures using an atlas, or to map functional MRI pRF data, we first need to generate cortical and white matter surfaces and brain region parcellations using [Freesurfer](https://brainlife.io/app/58c56d92e13a50849b258801). These surfaces and parcellations will be used to map pRF data to the occipital lobe surface.

### 2. pRF Mapping.

The next step is to map **population receptive field (pRF)** data to the cortical occipital surface. **pRF** is a method that uses fMRI to map visual function in the **occipital lobe**, which is the visual processing center of the brain. Specifically, stimuli designed to cover different properties of the visual field are used to map functional activitation of the occipital lobe. These specific properties include location in space (eccentricity) and orientation of the stimuli (polar angle). This works due to the unique preservation of every location in our visual field onto the retina all the way to the cortex, a property called **retinotopy**, discovered by Drs. David Hubel and Torsten Wiesel in 1959. 

In their fundamental work, Hubel and Wiesel demonstrated this property in cats. Specifically, they recorded from visual cortex neurons in cats and showed visual light bars of different orientations and locations in the visual field to the cats. They found that specific locations of the visual cortex responded preferentially to specific angles and locations in the visual field. From this work, vision scientists and neuroscientists have been able to map the organization of the visual cortex using similar methodologies to those used by Hubel and Wiesel through neuroimaging!

There are two ways you can map this visual function data to the cortex: the first involves the [fitting of an atlas](https://brainlife.io/app/5cc4cd3f4ed9df00317f621d) to the occipital lobe and the other involves [fitting fMRI pRF data](https://brainlife.io/app/5afc9831322997002773ed1c) to the occipital lobe. On brainlife.io, we developed apps to perform both of these tasks! This tutorial will walk you through both methods.

Useful information about pRF can be found in this [Neuroimage paper](https://pubmed.ncbi.nlm.nih.gov/17977024/). More information about the atlas-based mapping can be found in this [PLOS Computational Biology paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003538). 

Now, let's get to work! The following steps of this tutorial will show you how to:

1. generate anatomical surfaces using Freesurfer, 
1. map pRF data using an atlas,
1. and map pRF data using fMRI pRF data

### Copy appropriate data over from a single subject in the HCP 7T / Retinotopy (pRF) project

1. Click the following link to go to the project's page for the 'HCP 7T / Retinotopy (pRF)' project: [https://brainlife.io/project/5afc7d24322997002773ed19/](https://brainlife.io/project/5afc7d24322997002773ed19/)
1. Click the 'Archive' tab at the top of the screen to go to the Archive page.
1. Select the following datatypes from one subject by clicking the boxes next to the data:
    * func/task
    * stimulus
    * anat/t1w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project,' select your project from the drop-down menu.
    * For 'Process,' select 'Create New Process' and title it "pRF Tutorial." Hit 'Submit.'
        * This will take you to the process on your Projects page
1. Archive the data in your project by clickin the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: generate Freesurfer surfaces!

### Generate surfaces using Freesurfer:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer.'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w) image by clicking the drop-down menu and finding the appropriate dataset.
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
            
Once you're happy with the surfaces, you can move onto running the atlas-based pRF mapping!

### pRF Mapping - Atlas-based

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'pRFs / Benson14-Retinotopy'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For freesurfer, select the Freesurfer output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'atlas_based'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon next to the 'prf' tagged output.
    * Choose the 'PRF' viewer
    
If you're happy with the results, then you're ready to move onto mapping the fMRI pRF data!

### pRF Mapping - fMRI-based

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'pRFLife mrTools'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For func/task, select the staged func/task pRF data by clicking the drop-down menu and finding the appropriate dataset.
    * For stimulus, select the staged stimulus pRF data by clicking the drop-down menu and finding the appropriate dataset.
    * For freesurfer, select the Freesurfer output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Leave all other options as defaults.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmri_based'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon next to the 'prf' tagged output.
    * Choose the 'PRF' viewer

**If you're happy with the results, then you have successfully finished mapping visual function data (pRF) using both an atlas-based mapping method and an fMRI-based mapping method! You're now ready to move onto the next tutorial: diffusion MRI preprocessing!**
