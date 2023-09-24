!!! warning
    This is a draft

## Functional Connectivity.

Functional MRI measures the BOLD signal - a measure associated with neuronal activity - across the entire brain in order to identify functional characteristics of the brain. One particular type of analysis that describes the functional organization of the brain is functional connectivity analysis. Functional connectivity characterizes how in-sync, or out-of-sync, particular regions are by correlating their BOLD responses at every time point across an entire fMRI scan. The rationale is that regions that are highly correlated in their BOLD responses at a given time are likely to be working together for a particular task or activity. These correlation values are stored in what is known as a connectivity matrix. Each position in the matrix represents the correlation of BOLD activity between two particular regions.

From these matrices, scientists can then examine properties that describe the inter-relatedness of many regions. These properties can be used to identify **network-level** inter-individual differences in a large cohort.

This page demonstrates how to generate functional connectivity matrices on brainlife.io. The goal of this tutorial is to show you how to generate functional connectivity matrices following fMRI preprocessing. This tutorial will be using [Multi-atlas Transfer Tool (MaTT)](https://brainlife.io/app/6016dcdc321a4aafce6decd5) to map the hcp-mmp-b cortical atlas to preprocessed Freesurfer surfaces and [fMRI Connectivity Matrix Generation](https://brainlife.io/app/5c720cf63e2f2c0030a23486) to compute the correlation between each region in the atlas.

This tutorial will use a combination of skills developed in the [Introduction tutorial](/docs/tutorial/introduction-to-brainlife/) and presumes that you have data processed using [fMRIPrep](https://brainlife.io/app/5c61c69f14027a01b14adcb3). If you haven't read our introduction to brainlife, or if you don't have preprocessed fMRI and Freesurfer outputs on brainlife.io, please go back through that tutorial or the [Functional MRI Preprocessing](/docs/tutorial/fmri-preprocessing-tutorial) tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of functional connectivity analyses often involves processing the anatomical images. In order to guarantee that any generalizations regarding location made from the preprocessed diffusion data is anatomically-informed, we must have both of our anatomical (T1w or T2w) images and our diffusion MRI images **aligned**. One way we can make this easier for [fMRIPrep](https://brainlife.io/app/5c61c69f14027a01b14adcb3) is by aligning the anatomical images in such a way that the center of the brain is centered in the image. We refer to this as **ACPC-aligned**, as we are aligning the data to the **anterior commissure-posterior comissure plane**. This is the first step in fMRI preprocessing, and it is typically done with the [FSL Anat (T1w)](https://brainlife.io/app/5e3c87ae9362b7166cf9c7f4) app. Once we've centered our anatomical image, we can move onto functional MRI preprocessing.


### 2. Functional preprocessing.

The second step of functional connectivity analyses involves processing the functional (fMRI) images. These can include both task-related (a person performing a task in the scanner) or resting-state (a person does no task in the scanner) fMRIs. Like any image directly from the scanner, there are common artifacts and issues with fMRI data. One of the biggest problems is **motion**. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. fMRIPrep will estimate the motion that occurred across all of the fMRI volumes and correct for it. This will align all of the volumes perfectly to ensure motion is not altering the data.

Another common artifact in fMRI data is **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition -- fMRIPrep will also automatically correct for this!

fMRIPrep can additionally fix the lag in the BOLD signal and the fact that we need to acquire data in non-contiguous slices. This is referred to as **slice-timing**, or the order in which slices are acquired. Without fMRIPrep, our data would not be ordered properly and we wouldn't have contiguous brain slices.

Finally, images from the fMRI scanner might not be perfectly aligned to images collected before it, such as the anatomical images. To fix this, we need to register the functional volumes to the anatomical image and map the fMRI signal onto the surfaces generated in the anatomical preprocessing step -- fMRIPrep does this registration automatically!

Useful information about fMRIPrep anatomical preprocessing can be found in this [original Nature paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6319393/#S13title).

There are two versions of the brainlife.io fMRIPrep app. One generates outputs mapped to the volumes ([fMRIPrep-volume](https://brainlife.io/app/5c61c69f14027a01b14adcb3)) and the other generates outputs mapped to the surfaces ([fMRIPrep-surface](https://brainlife.io/app/5dfceebd32bff0640ce27bbd)).

For this tutorial, we will use the volume-based version.

### 3. Functional connectivity matrix generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating **functional connectivity matrices**! This is done by examing the fMRI BOLD activity in multiple regions across the brain by correlating the regions' BOLD activity throughout the entire acquisition. The reason we do this is because regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (node) with every other region (node) in the brain is with a **network matrix** -- note that **nodes** represent the brain regions here. Each point in the network matrix represents the correlation of BOLD activity between one region and another. We can then use these network matrices to examine properties of the network that describe how interrelated specific regions in the brain are working during the fMRI acquisition in either task-related or resting-state fMRIs. To generate our matrices, we can use the [fMRI Timeseries Extraction](https://brainlife.io/app/5ed05fe8e3f4538080268585) app to extract the timeseries, and the [Time series to network](https://brainlife.io/app/60b961b10ad40d58e1ca7fe7) app to generate the matrices. Then, we can use the [Network Preprocess](https://brainlife.io/app/5e76ea90de643b5e7c2a9404) to preprocess the networks to transform the weights to the 'absolute value' (i.e. remove the sign to study magnitude of the connection), and the [Network Measurements](https://brainlife.io/app/5ea70853f1745db7d9f7b0dc) to compute local and global network properties of the preprocessed network. Finally, we can use the [Time series to network](https://brainlife.io/app/649f24c53a63cf17374a4595) to extract useful data (including the connectivity matrices and the global/local network measures) into tidy dataframes

Now, let's get to work! The following steps of this tutorial will show you how to:

1. Preprocess the anatomical (T1w) data,
1. Generate Freesurfer surfaces and parcellations,
1. Map hcp-mmp-b cortical atlas to the surface,
1. Preprocess fMRI data,
1. Extract timeseries from the regions of the hcp-mmp-b atlas,
1. Convert the timeseries to a network
1. Preprocess the generated network,
1. Compute local and global network properties,
1. and extract the connectivity matrix, and local and global dataframes into individual csvs for analysis

### Copy appropriate data over from a single subject in the Tutorial data project

1. Click the following link to go to the project's page for the [Tutorial](https://brainlife.io/project/5ae916e7f446980028b15eb3)
1. Click the 'Archive' tab at the top of the screen to go to the archive's page.
1. Select the following datatypes from one subject by clicking the boxes next to the data for subject '10217':
    * func/task rest
    * anat/t1w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project', select your project from the drop-down menu.
    * For 'Process', select 'Create New Process' and title it "Network Neuroscience - functional tutorial". Hit 'Submit'.
        * This will take you to the process on your Project's page
1. Archive the data in your project by clicking the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: preprocessing the anatomical data!

### Preprocess anatomical (T1w) data using FSL:

#### T1w data
1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'FSL Anat (T1w).'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w) image by clicking the drop-down menu and finding the appropriate datasets.
    * Select the boxes for 'crop' and 'reorient'. 
    * Leave all other defaults the same.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'fsl_anat'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'FSLeyes' as your viewer
        * This will load the the preprocessed T1w data.

### Generate surfaces using Freesurfer:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer 7.3.2'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged preprocessed, 'acpc_aligned'-tagged anatomical (T1w) image by clicking the drop-down menu and finding the appropriate datasets.
    * Select the boxes for 'hippocampal' and 'thalamicnuclei'. Leave all other defaults the same
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'fsl_anat'
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

Once you're happy with the surfaces, you can move onto mapping the Yeo17dil atlas!

### Map the hcp-mmp-b atlas:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Multi-Atlas Transfer Tool'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged Freesurfer output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For 'atlas,' select 'hcp-mmp-b' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'hcp-mmp-b'
    * Hit 'Submit'

Once the app is finished, you're ready to move onto the next step: Computing anatomical statistics!

### Preprocess your data with fMRIPrep:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Volume Output'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged preprocessed, 'acpc_aligned'-tagged anatomical (T1w) image, the freesurfer output, and the functional data by clicking the drop-down menu and finding the appropriate datasets.
    * For 'space,' select 'T1w' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmriprep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon next to the 'html' tagged output.
    * Choose the 'html' viewer

Once the app is finished, you're ready to move onto the final step: network matrix generation!

### Extract functional timeseries:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'fMRI Timeseries Extraction'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the generated bold mask, preprocessed functional data, the hcp-mmp-b parcellation-volume, and the preprocessed regressors by clicking the drop-down menu and finding the appropriate datasets.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'hcp-mmp-b'
    * Hit 'Submit'

Once complete, you can now convert the conmat datatype to a network dataytpe!

### Convert timeseries data to connectivity matrices:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Time series to network'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the timeseries datatype generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For method, select 'correlation'
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'hcp-mmp-b'
    * Hit 'Submit'

Once complete, you can now preprocess your networks!

### Preprocess the generated network:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Network Preprocess'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the network datatype generated in Step 6 by clicking the drop-down menu and finding the appropriate dataset.
    * For transform, select 'absolute'
    * Select the box for 'retain-weights'
    * For threshold, enter '0.1'
    * Leave all other defaults the same.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'abs_thr_0p1'
    * Hit 'Submit'

Once complete, you can now compute global and local network measures from your preprocessed network!

### Compute local and global network properties:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Network Measurements'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the preprocessed network datatype generated in Step 7 by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'abs_thr_0p1'
    * Hit 'Submit'

Once complete, you can now extract all of the useful network data into tidy csvs!

### Extract the connectivity matrix, and local and global dataframes into individual csvs for analysis:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Extract network statistics and connectivity matrices from network datatype'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the preprocessed network datatype, with the associated network measurements, generated in Step 8 by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'abs_thr_0p1'
    * Hit 'Submit'

**Nice work! You've completed this tutorial. Now that the app is finished, you're ready to perform group analyses on your connectivity matrices and examine the network structure of your data!**
