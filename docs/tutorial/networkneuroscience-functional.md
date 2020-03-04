!!! warning
    This is a draft

## Functional Connectivity.

Functional MRI measures the BOLD signal - a measure associated with neuronal activity - across the entire brain in order to identify functional characteristics of the brain. One particular type of analysis that describes the functional organization of the brain is functional connectivity analysis. Functional connectivity characterizes how in-sync, or out-of-sync, particular regions are by correlating their BOLD responses at every time point across an entire fMRI scan. The rationale is that regions that are highly correlated in their BOLD responses at a given time are likely to be working together for a particular task or activity. These correlation values are stored in what is known as a connectivity matrix. Each position in the matrix represents the correlation of BOLD activity between two particular regions. ![connectivity-matrix](/docs/img/app-fmri-to-conmat.bl.header.png)

From these matrices, scientists can then examine properties that describe the inter-relatedness of many regions. These properties can be used to identify **network-level** inter-individual differences in a large cohort.

This page demonstrates how to generate functional connectivity matrices on brainlife.io. The goal of this tutorial is to show you how to generate functional connectivity matrices following fMRI preprocessing. This tutorial will be using [matt](/docs/img/app-matt.bl.header.png) to map the Glasser 180-node cortical atlas to preprocessed Freesurfer surfaces and [conmat](/docs/img/app-fmri-to-conmat.bl.header.png) to compute the correlation between each region in the atlas.

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) and presumes that you have data processed using  [fMRIPREP](https://brainlife.io/app/5c61c69f14027a01b14adcb3). If you haven't read our introduction to brainlife, or if you don't have preprocessed fMRI and Freesurfer outputs on brainlife.io, please go back through that tutorial or the [Functional MRI Preprocessing](https://brainlife.io/docs/tutorial/fmri-preprocessing-tutorial) tutorial before beginning this one.

### 1. Functional connectivity network matrices generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating **functional connectivity matrices**! This is done by examing the fMRI BOLD activity in multiple regions across the brain by correlating the regions' BOLD activity throughout the entire acquisition. The reason we do this is because regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (node) with every other region (node) in the brain is with a **network matrix** -- note that **nodes** represent the brain regions here. Each point in the network matrix represents the correlation of BOLD activity between one region and another. We can then use these network matrices to examine properties of the network that describe how interrelated specific regions in the brain are working during the fMRI acquisition in either task-related or resting-state fMRIs.

There is a [brainlife.io](https://brainlife.io) app for generating these matrices that we will use in this tutorial.

| ![conmat](/docs/img/app-fmri-to-conmat.bl.header.png) |
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.167 |

Now, let's get to work! The following steps of this tutorial will show you how to:
1. generate anatomical surfaces using Freesurfer, 
2. preprocess the anatomical (T1w & T2w) and fMRI data using fMRIPrep, 

3. and, map the Glasser 180-node atlas to the anatomical (T1w) image
4. and generate network matrices from the regions of the Glasser 180-node atlas.

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

1. Go to the 'Archives' page of your project by clicking the 'Archives' tab.
1. Select the anatomical (T1w & T2w) images and the functional data datatypes by clicking the box next to the datasets.
1. Click the 'Stage to process'
    * For Project, make sure your project is selected
    * For Process, select 'Create New Process'
    * For Description, name the process 'fmriPrep Preprocessing'
    * Hit 'OK'
1. On the 'Process' tab, click 'Submit App' to submit a new application.
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

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Volume Output'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w & T2w) images, the freesurfer output, and the functional data by clicking the drop-down menu and finding the appropriate datasets.
    * For 'space,' select 'MNI152NLin6Asym' from the drop-down menu.
    * For 'resolution,' select 'original' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmriPrep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the output.
    * VIEWER CURRENTLY IN DEVELOPMENT. WILL UPDATE ONCE COMPLETED

If you're happy with the results, then you have successfully finished preprocessing your fMRI data with fMRIPrep! Congrats!

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

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'fMRI to Connectivity Matrices'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the generated bold mask, preprocessed functional data, the Glasser parcellation-volume, and the preprocessed regressors by clicking the drop-down menu and finding the appropriate datasets.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'connectivity_matrix'
    * Hit 'Submit'
    
Nice work! You've completed this tutorial. Now that the app is finished, you're ready to perform group analyses on your connectivity matrices!
