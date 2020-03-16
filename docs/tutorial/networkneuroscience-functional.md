!!! warning
    This is a draft

## Functional Connectivity.

Functional MRI measures the BOLD signal - a measure associated with neuronal activity - across the entire brain in order to identify functional characteristics of the brain. One particular type of analysis that describes the functional organization of the brain is functional connectivity analysis. Functional connectivity characterizes how in-sync, or out-of-sync, particular regions are by correlating their BOLD responses at every time point across an entire fMRI scan. The rationale is that regions that are highly correlated in their BOLD responses at a given time are likely to be working together for a particular task or activity. These correlation values are stored in what is known as a connectivity matrix. Each position in the matrix represents the correlation of BOLD activity between two particular regions. 

From these matrices, scientists can then examine properties that describe the inter-relatedness of many regions. These properties can be used to identify **network-level** inter-individual differences in a large cohort.

This page demonstrates how to generate functional connectivity matrices on brainlife.io. The goal of this tutorial is to show you how to generate functional connectivity matrices following fMRI preprocessing. This tutorial will be using [Multi-atlas Transfer Tool (MaTT)](https://brainlife.io/app/5aeb34f2f446980028b15ef0) to map the Glasser 180-node cortical atlas to preprocessed Freesurfer surfaces and [fMRI Connectivity Matrix Generation](https://brainlife.io/app/5c720cf63e2f2c0030a23486) to compute the correlation between each region in the atlas.

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) and presumes that you have data processed using [fMRIPrep](https://brainlife.io/app/5c61c69f14027a01b14adcb3). If you haven't read our introduction to brainlife, or if you don't have preprocessed fMRI and Freesurfer outputs on brainlife.io, please go back through that tutorial or the [Functional MRI Preprocessing](https://brainlife.io/docs/tutorial/fmri-preprocessing-tutorial) tutorial before beginning this one.

### 1. Functional connectivity matrix generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating **functional connectivity matrices**! This is done by examing the fMRI BOLD activity in multiple regions across the brain by correlating the regions' BOLD activity throughout the entire acquisition. The reason we do this is because regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (node) with every other region (node) in the brain is with a **network matrix** -- note that **nodes** represent the brain regions here. Each point in the network matrix represents the correlation of BOLD activity between one region and another. We can then use these network matrices to examine properties of the network that describe how interrelated specific regions in the brain are working during the fMRI acquisition in either task-related or resting-state fMRIs. To generate our matrices, we can use the [fMRI Connectivity Matrix Generation](https://brainlife.io/app/5c720cf63e2f2c0030a23486) app.

Now, let's get to work! The following steps of this tutorial will show you how to:

1. Map the Glasser 180-node atlas to the Freesurfer output,
1. and generate network matrices from the regions of the Glasser 180-node atlas.

### Map the Glasser 180-node atlas:

1. On the 'Archive' tab of your project, stage your fMRIPrep outputs and Freesurfer outputs to a new process in your project by clicking the box next to the appropriate outputs and clicking 'Stage to process'.
    * For 'Project', select your project
    * Select 'Create new process'
        * In the description field, enter 'Functional Connectivity Network Matrix Generation'
1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Multi-Atlas Transfer Tool'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged Freesurfer output by clicking the drop-down menu and finding the appropriate dataset.
    * For 'space,' select 'hcp-mmp-b' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'Glasser'
    * Hit 'Submit'

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
    
**Nice work! You've completed this tutorial. Now that the app is finished, you're ready to perform group analyses on your connectivity matrices and examine the network structure of your data!**
