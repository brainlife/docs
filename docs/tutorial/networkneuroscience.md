## Network connectivity.

Networks, broadly, describe how centers (i.e. nodes) of a mechanism interact with one another and are connected (i.e. edges). One example of a network is the internet, with computers connecting to, and sharing information amongst, each other. Specifically in neuroscience, these networks describe how specific cortical regions in the brain are connected, either in terms of correlated BOLD signal (fMRI) or streamline density (dMRI). From this information, it’s possible to model how the brain is organized, both on global and local scales. For example, Sporns et al (2000) were able to model the organization of the white matter connections in the macaque visual cortex, providing supporting evidence of specific anatomical subgroupings within the visual cortex. This information is stored in the form of a matrix, with each cell in the matrix corresponding to the connectivity between two specific cortical regions. Specifically, these matrices can expresses either the presence of a connection (i.e. binary network) or the strength of a connection (i.e. weighted network). The data in these matrices can be viewed in multiple graphical ways which may illuminate specific communities, or interconnected regions within the global network, that might not be readily present in the matrix.

# Functional network connectivity

Functional MRI measures the BOLD signal - a measure associated with neuronal activity - across the entire brain in order to identify functional characteristics of the brain. One particular type of analysis that describes the functional organization of the brain is functional connectivity analysis. Functional connectivity characterizes how in-sync, or out-of-sync, particular regions are by correlating their BOLD responses at every time point across an entire fMRI scan. The rationale is that regions that are highly correlated in their BOLD responses at a given time are likely to be working together for a particular task or activity. These correlation values are stored in what is known as a connectivity matrix. Each position in the matrix represents the correlation of BOLD activity between two particular regions. 

From these matrices, scientists can then examine properties that describe the inter-relatedness of many regions. These properties can be used to identify **functional network-level** inter-individual differences in a large cohort.

# Structural network connectivity

Diffusion MRI measures how **anistropic** the movement of water is, with the basic principle that myelinated tissue will create more **anistropic** water movement.. One particular type of analysis that describes the structural organization of the white matter using this data is **structural connectivity**. **Structural connectivity** characterizes connected different regions are by computing the number of **diffusion tractography streamlines** that terminate into a particular region. The rationale is that regions that have greater density of streamline terminations have greater connectivity with the regions where those streamlines also terminate. These density values are stored in what is known as a connectivity matrix. Each position in the matrix represents the connectivity between two particular regions. 

From these matrices, scientists can then examine properties that describe the inter-relatedness of many regions. These properties can be used to identify **structural network-level** inter-individual differences in a large cohort.

This page demonstrates how to generate both **functional and structural connectivity matrices** on brainlife.io. The goal of this tutorial is to show you how to generate functional and structural connectivity matrices following anatomical (T1w), fMRI, and dMRI preprocessing. 

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/), [Anatomical (T1w) Preprocessing](https://brainlife.io/docs/tutorial/anatomical-preprocessing/), [fMRI Preprocessing](https://brainlife.io/docs/tutorial/functional-preprocessing/), and the [dMRI Preprocessing](https://brainlife.io/docs/tutorial/diffusion-preprocessing/) tutorials. If you are not comfortable archiving, staging, and running apps on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of structural network generation often involves processing the anatomical images. In order to guarantee that any generalizations regarding location made from the preprocessed diffusion data is anatomically-informed, we must have both of our anatomical (T1w or T2w) images and our diffusion MRI images **aligned**. One way we can make this easier for [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) is by aligning the anatomical images in such a way that center of the brain is centered in the image. We refer to this as **ACPC-aligned**, as we are aligning the data to the **anterior commissure-posterior comissure plane**. This is the first step in dMRI preprocessing, and is typically done with the [HCP ACPC Alignment (T1w)](https://brainlife.io/app/5c61c69f14027a01b14adcb3) app. In order to map the Glasser 180-node atlas using the [Multi-Atlas Transfer Tool](https://brainlife.io/app/5aeb34f2f446980028b15ef0) and compute the streamline density between each region, we first need to generate cortical and white matter surfaces, and brain region parcellations, using [Freesurfer](https://brainlife.io/app/58c56d92e13a50849b258801). Once our anatomical image is aligned, our surfaces are generated, and the Glasser 180-node cortical atlas is mapped, we can move onto diffusion preprocessing.

### 2. Functional preprocessing.

The second step of fMRIPrep involves processing the functional (fMRI) images. These can include both task-related (a person performing a task in the scanner) or resting-state (a person does no task in the scanner) fMRIs. Like any image directly from the scanner, there are common artifacts and issues with fMRI data. One of the biggest problems is **motion**. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. fMRIPrep will estimate the motion that occurred across all of the fMRI volumes and correct for it. This will align all of the volumes perfectly to ensure motion is not altering the data. 

Another common artifact in fMRI data is **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition -- fMRIPrep will also automatically correct for this! 

fMRIPrep can additionally fix the lag in the BOLD signal and the fact that we need to acquire data in non-contiguous slices. This is referred to as **slice-timing**, or the order in which slices are acquired. Without fMRIPrep, our data would not be ordered properly and we wouldn't have contiguous brain slices.

Finally, images from the fMRI scanner might not be perfectly aligned to images collected before it, such as the anatomical images. To fix this, we need to register the functional volumes to the anatomical image and map the fMRI signal onto the surfaces generated in the anatomical preprocessing step -- fMRIPrep does this registration automatically!

### 3. Functional connectivity network matrices generation.

Once the anatomical and fMRI data is preprocessed with fMRIPrep, we can now examine the functional network organization by generating **functional connectivity matrices**! This is done by examining the fMRI BOLD activity in multiple regions across the brain by correlating the regions' BOLD activity throughout the entire acquisition. The reason we do this is because regions that are active in similar ways at similar time points are more likely to be working with each other to perform a specific task. The way we typically represent the correlation coefficients, or weights, of each region (node) with every other region (node) in the brain is with a **network matrix** -- note that **nodes** represent the brain regions here. Each point in the network matrix represents the correlation of BOLD activity between one region and another. We can then use these network matrices to examine properties of the network that describe how interrelated specific regions in the brain are working during the fMRI acquisition in either task-related or resting-state fMRIs.

### 4. Diffusion preprocessing 

The next step is to preprocess the diffusion data. dMRI data, like any other MRI dataset, is quite noisy. Just like fMRI, dMRI data is sensitive to **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition. Also, dMRI data is very susceptible to **motion**. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. Finally, movement of the magnetic gradients can induce electromagnetic currents which can affect the measurement in any given location. This is called **eddy currents**, and is due to the fact that electricity and magnetism are part of the same field - i.e. the *electromagnetic field*. We can correct for all of these common artifacts using [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9).

In addition to these artifacts, dMRI has very low signal-to-noise ratio (SNR), which directly impacts how well you can fit microstructural models and analyze diffusion data. This means that in any given location, the amount of diffusion signal is very low compared to the "noise" of the scanner. There are methods to lower the amount of noise across the brain, and thus increase the SNR. [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) has the capability to identify and remove noise using principal components analysis (PCA) and Rician analysis.

On top of these issues, dMRI data is sensitive to other artifacts and non-regularities, including **gibbs ringing and bias field inhomogeneities**, which can affect the quality of the data and how well measures can be derived from the data. [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) can deal with all of these issues for us!

Finally, once the dMRI data is preprocessed and cleaned, [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) will align the dMRI data to the anatomical data. This will ensure that any analyses we do with the dMRI data will be anatomically-informed and biologically-relevant.

Useful information about preprocessing pipeline [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) is designed to run -- Diffusion Parameter EStimation with Gibbs and NoisE Removal (DESIGNER) -- can be found in this [original Neuroimage paper](https://pubmed.ncbi.nlm.nih.gov/30077743-evaluation-of-the-accuracy-and-precision-of-the-diffusion-parameter-estimation-with-gibbs-and-noise-removal-pipeline/).

### 5. Diffusion modelling & anatomically-informed ensemble tractography.

Once the dMRI data has been cleaned and aligned to the anatomical (T1w) image, the next step is to map the underlying white matter anatomy using diffusion tractography. Diffusion tractography works by modelling how water is moving throughout the brain based on the principle of **anisotropy** -- that water will move unequally in a particular direction given a physical constraint. In the case of diffusion MRI, this constraint comes from the myelin wrapping the many axons arranged into bundles known as **fasicles**. There are common groupings of these **fasicles**, known as **white matter tracts**, based on their terminations into the cortex. Diffusion tractography generates **streamlines** that act as evidence of underlying white matter organization. We can bundle these **streamlines** into common **white matter tracts** *digitally*, just as we can bundle **fasciles** into **tracts** using histological methods.

The first step is to fit models of diffusion at each location in the dMRI image that can be used as a guide for the tractography algorithms. The most popular model for this is known as **constrained spherical deconvolution**, or **CSD**. **CSD** allows for the possibility of multiple **fasicles** entering a single location that may have different trajectories, which is a major advantage of the model over other models of diffusion. The **CSD** model at each location can tell the tracking algorithm **how strongly** and in **what direction** water is diffusing in the brain, and thus where organized and myelinated white matter is likely to be.

Once we know the direction of water movement using the **CSD** model, we can then perform **diffusion tractography** to map the underlying organized white matter. **Diffusion tractography** *sews* together regions with common directions of water movement into **streamlines** that provide evidence for organized, myelinated white matter. These operations can be done millions of times across the entire brain to form **tractograms**. There are many algorithms for doing this that fall into two main categories -- **deterministic** and **probabilistic**. **Determinstic** algorithms work by strictly following the directions of water movement, while **probabilistic** algorithms infer a probability of different directions of water movement at any given location. **Deterministic** tractography tends to be an overly conservative representation of the white matter, as modelling is never perfectly accurate. **Probabilistic** tractography tends to provide *hairy* streamline representations, as the model allows for a probability of different directions of water movement at any given location. Recently, it was discovered that combining both algorithms -- i.e. **ensembling** them -- provides the most anatomically-accurate **tractograms**.

On brainlife.io, we have combined **ensemble tractography** with **anatomically-constrained tractography (ACT)**, which restricts streamline representations to those that are biologically-plausible, in a single [Ensemble ACT Tractography](https://brainlife.io/app/5aac2437f0b5260027e24ae1) app! Within this app, the **CSD** model will be fit and **tractography** will be performed generating **whole-brain tractograms**. In combination, a model of white matter microstructure -- diffusion tensor (DTI) -- will be fit and maps of fractional anisotropy (FA), mean diffusivity (MD), radial diffusivity (RD), and axial diffusivity (AD) will be generated.

More information on **ensemble tractography** can be found in this [PLOS Computational Biology](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004692) paper. More information on **ACT** can be found in this [Neuroimage](https://pubmed.ncbi.nlm.nih.gov/22705374-anatomically-constrained-tractography-improved-diffusion-mri-streamlines-tractography-through-effective-use-of-anatomical-information/) article.

### 6. Generate structural connectivity matrices.

Once the whole brain tractograms have been generated, we can now compute the streamline density between each region and generate our **structural connectivity matrices**. These matrices can then be used to derive **network-related** measures of the organization of the white matter of an individual, or common properties amongst groups. For this tutorial, we will use [network matrices](https://brainlife.io/app/5bcdefeadc47f70026e8c6ac) to generate our structural connectivity matrix.

Now, let's get to work! The following steps of this tutorial will show you how to:

1. ACPC-align the anatomical (T1w) image,
1. Generate Freesurfer surfaces and parcellations,
1. Map Glasser 180-node cortical atlas to the surface,
1. Preprocess fMRI data
1. Preprocess dMRI data,
1. Perform diffusion tractography,
1. and generate network matrices (functional and structural) from the regions of the Glasser 180-node atlas.

### Copy appropriate data over from a single subject in the InterTVA project

1. Click the following link to go to the project's page for the 'InterTVA' project: https://brainlife.io/project/5c8415aa34225c0031027372
1. Click the 'Archive' tab at the top of the screen to go to the archive's page.
1. Select the following datatypes from one subject by clicking the boxes next to the data:
    * func/task rest
    * anat/t1w
    * neuro/dwi (both AP and PA)
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project', select your project from the drop-down menu.
    * For 'Process', select 'Create New Process' and title it "Networks tutorial". Hit 'Submit'.
        * This will take you to the process on your Project's page
1. Archive the data in your project by clicking the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: align the anatomical image to the ACPC plane!

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

Once the app is finished, you're ready to move onto the next step: fMRI Preprocessing!

### Preprocess fMRI data with fMRIPrep:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'fmriPrep - Volume Output'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the ACPC-aligned anatomical (T1w) image, the freesurfer output, and the functional data by clicking the drop-down menu and finding the appropriate datasets.
    * For 'space,' select 'MNI152NLin6Asym' from the drop-down menu.
    * For 'resolution,' select 'original' from the drop-down menu.
    * Select the box for 'Archive all output datasets' when finished
        * For 'Dataset Tags,' type and enter 'fmriPrep'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon next to the 'html' tagged output.
    * Choose the 'html' viewer

### Generate functional connectivity network matrices:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'fMRI to Connectivity Matrices'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the generated bold mask, preprocessed functional data, the Glasser parcellation-volume, and the preprocessed regressors by clicking the drop-down menu and finding the appropriate datasets.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'connectivity_matrix'
    * Hit 'Submit'
    
### Preprocess diffusion MRI data with mrtrix3 preproc.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'mrtrix3 preproc'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For anat/t1w, select the staged ACPC-aligned anatomical (T1w) image by clicking the drop-down menu and finding the appropriate dataset.
    * For dwi, select the dwi image with the tag "PA" by clicking the drop-down menu and finding the appropriate dataset.
    * For dwi (reverse), select the dwi image with the tag "AP" by clicking the drop-down menu and finding the appropriate dataset.
    * for acqd, choose 'posterior -> anterior (y) (PA) by clicking the drop-down menu and finding the appropriate choice.
    * Leave all the other options as default
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'acpc_aligned'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
    * Only have the file titled 'dwi.nii.gz' selected in the viewer
1. You can also generate a QA image of the DWI-T1 alignment by running the 'Generate images of DWI overlaid on T1' using the ACPC-aligned anatomical image and the preprocessed dMRI image generated above! Archive the results and save with the tag 'qa dmri-t1 overlay'.

Once you're happy with the results, you can move onto fitting the CSD, DTI, and performing tractography!

### Fit the CSD & DTI models, perform diffusion tractography.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'mrtrix3 Anatomically Constrained Tractography (ACT)'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For dwi, select the preprocessed dMRI image generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For tensor_fit, type '999' to fit the model on the b=1000 shell
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'whole_brain_tractography'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'mrview' as your viewer
        * This will overlay the tractogram on the generated fractional anistropy (FA) image
1. You can also generate a QA image of the whole-brain tractograms by running the 'Generate figures of whole-brain tractogram (tck)' using the whole-brain tractogram (tck) generated above! Archive the results and save with the tag 'qa whole brain tractogram'.

If you're happy with the results, you're ready to move onto structural connectivity generation!

### Generate structural connectivity matrix.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Network Matrices'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For parcellation/volume, select the Glasser volume generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For track/tck, select the whole brain tractogram generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For tensor, select the DTI tensor output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Leave mask empty
    * For infl, leave as 2
    * For microdat, select 'FA' from the drop-down menu
    * Select true for compshape, compmicro, and comptprof by clicking the box next to each option
    * Leave nnondes as 100
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'structural matrix'
    * Hit 'Submit'
    
Once complete, you're now ready to convert the matrices to the proper data format!

### Convert structural network matrices to conmat.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Convert network neuro matrix to conmat'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For parcellation/volume, select the Glasser volume generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For raw (networkmatrices), select the network matrices output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For measure, select 'density' by clicking the drop-down menu and finding the appropriate measure.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'structural matrix density'
    * Hit 'Submit'
    
Once complete, you're now ready to visualize the networks generated above!
    
### Visualize functional and structural networks.

1. On the 'Process' tab, click 'Submit App' to submit a new application for each network (structural and functional).
    * In the search bar, type 'Network Visualization'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For conmat (preprocessed), select either the conmat (structural matrix density) output to generate the structural network or the conmat (functional connectivity matrix) for the functional network generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'matrix visualization'
    * Hit 'Submit'
    
Nice work! You've completed this tutorial. Now you've preprocessed a subject all the way through functional and structural networks! You're now ready to take on your entire study!


