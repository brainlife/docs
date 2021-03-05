!!! warning
    This is a draft

## Functional Connectivity.

Functional MRI measures the BOLD signal - a measure associated with neuronal activity - across the entire brain in order to identify functional characteristics of the brain. One particular type of analysis that describes the functional organization of the brain is functional connectivity analysis. Functional connectivity characterizes how in-sync, or out-of-sync, particular regions are by correlating their BOLD responses at every time point across an entire fMRI scan. The rationale is that regions that are highly correlated in their BOLD responses at a given time are likely to be working together for a particular task or activity. These correlation values are stored in what is known as a connectivity matrix. Each position in the matrix represents the correlation of BOLD activity between two particular regions. 

From these matrices, scientists can then examine properties that describe the inter-relatedness of many regions. These properties can be used to identify **network-level** inter-individual differences in a large cohort.

This page demonstrates how to generate functional connectivity matrices on brainlife.io. The goal of this tutorial is to show you how to generate functional connectivity matrices following fMRI preprocessing. This tutorial will be using [Multi-atlas Transfer Tool (MaTT)](https://brainlife.io/app/5aeb34f2f446980028b15ef0){target=_blank} to map the Glasser 180-node cortical atlas to preprocessed Freesurfer surfaces and [fMRI Connectivity Matrix Generation](https://brainlife.io/app/5c720cf63e2f2c0030a23486){target=_blank} to compute the correlation between each region in the atlas.

This tutorial will use a combination of skills developed in the [Introduction tutorial](/docs/tutorial/introduction-to-brainlife/) and presumes that you have data processed using [fMRIPrep](https://brainlife.io/app/5c61c69f14027a01b14adcb3){target=_blank}. If you haven't read our introduction to brainlife, or if you don't have preprocessed fMRI and Freesurfer outputs on brainlife.io, please go back through that tutorial or the [Functional MRI Preprocessing](/docs/tutorial/fmri-preprocessing-tutorial) tutorial before beginning this one.

### 1. Functional connectivity matrix generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating **functional connectivity matrices**! This is done by examing the fMRI BOLD activity in multiple regions across the brain by correlating the regions' BOLD activity throughout the entire acquisition. The reason we do this is because regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (node) with every other region (node) in the brain is with a **network matrix** -- note that **nodes** represent the brain regions here. Each point in the network matrix represents the correlation of BOLD activity between one region and another. We can then use these network matrices to examine properties of the network that describe how interrelated specific regions in the brain are working during the fMRI acquisition in either task-related or resting-state fMRIs. To generate our matrices, we can use the [fMRI Connectivity Matrix Generation](https://brainlife.io/app/5c720cf63e2f2c0030a23486){target=_blank} app.

Now, let's get to work! The following steps of this tutorial will show you how to:

1. ACPC-align the anatomical (T1w) image,
1. Generate Freesurfer surfaces and parcellations,
1. Map Glasser 180-node cortical atlas to the surface,
1. Preprocess fMRI data,
1. and generate network matrices from the regions of the Glasser 180-node atlas.

### ACPC-align anatomical (T1w) image.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'HCP ACPC Alignment (T1w)'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged anatomical (T1w) image generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For template, choose 'MNI152_1mm' by clicking the drop-down menu and finding the appropriate file
    * For reorient, make sure the box is checked.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'acpc_aligned'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
    * Only have the file titled 'out.nii.gz' selected in the viewer
1. You can also generate a QA image of the results by running the 'Generate images of T1' using the ACPC-aligned anatomical image generated above! Archive the results and save with the tag 'qa t1 acpc'.

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

### Map the Glasser 180-node atlas:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Multi-Atlas Transfer Tool'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged Freesurfer output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For 'space,' select 'hcp-mmp-b' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'Glasser'
    * Hit 'Submit'

Once the app is finished, you're ready to move onto the next step: preprocess the fMRI data!

### Preprocess your data with fMRIPrep:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Volume Output'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged ACPC-aligned anatomical (T1w) images, the freesurfer output, and the functional data by clicking the drop-down menu and finding the appropriate datasets.
    * For 'space,' select 'MNI152NLin6Asym' from the drop-down menu.
    * For 'resolution,' select 'original' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmriPrep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon next to the 'html' tagged output.
    * Choose the 'html' viewer

Once the app is finished, you're ready to move onto the final step: network matrix generation!

### Generate functional connectivity network matrices:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'fMRI to Connectivity Matrices'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the generated bold mask, preprocessed functional data, the Glasser parcellation-volume, and the preprocessed regressors by clicking the drop-down menu and finding the appropriate datasets.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'functional connectivity matrix'
    * Hit 'Submit'
    
Once complete, you can now convert the conmat datatype to a network dataytpe!

### Convert network matrices (contmat) to network datatype:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Conmat 2 Network'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the conmat datatype generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'functional connectivity networks'
    * Hit 'Submit'
    
Once complete, you can now visualize the network!
    
### Visualize functional  networks.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Network Visualization'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For conmat (preprocessed), select the network datatype for the functional network generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'functional visualization'
    * Hit 'Submit'
    
**Nice work! You've completed this tutorial. Now that the app is finished, you're ready to perform group analyses on your connectivity matrices and examine the network structure of your data!**
