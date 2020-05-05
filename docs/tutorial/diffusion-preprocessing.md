!!! warning
    This is a draft

## Diffusion-weighted MRI preprocessing.

This page demonstrates common steps used to preprocess diffusion magnetic resonance imaging (dMRI) data on brainlife.io. The goal of this tutorial is to show you how to process diffusion data for successive analyses, including **microstructural region analysis**, **diffusion tractography**, and **structural connectivity**. This tutorial will be mostly be using [MrTrix3 Preproc](https://brainlife.io/app/5a813e52dc4031003b8b36f9) for diffusion preprocessing and [FSL DTIFit](https://brainlife.io/app/5c1d67383e9c170177733d3f) for microstructral modelling.

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) and the [Anatomical Preprocessing tutorial](https://brainlife.io/docs/tutorial/t1w-preprocessing/) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of diffusion preprocessing often involves processing the anatomical images. In order to guarantee that any generalizations regarding location made from the preprocessed diffusion data is anatomically-informed, we must have both of our anatomical (T1w or T2w) images and our diffusion MRI images **aligned**. One way we can make this easier for [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) is by aligning the anatomical images in such a way that the center of the brain is centered in the image. We refer to this as **ACPC-aligned**, as we are aligning the data to the **anterior commissure-posterior comissure plane**. This is the first step in dMRI preprocessing, and it is typically done with the [HCP ACPC Alignment (T1w)](https://brainlife.io/app/5c61c69f14027a01b14adcb3) app. Once we've centered our anatomical image, we can move onto diffusion MRI preprocessing.

### 2. Diffusion preprocessing 

The next step in diffusion preprocessing is to actually preprocess the diffusion data. dMRI data, like any other MRI dataset, is quite noisy. Just like fMRI, dMRI data is sensitive to **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition. Also, dMRI data is very susceptible to motion. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. Finally, movement of the magnetic gradients can induce electromagnetic currents which can affect the measurement in any given location. These electromagnetic currents are called **eddy currents**, and they are due to the fact that electricity and magnetism are part of the same field, otherwise known as the **electromagnetic field**. Thankfully, brainlife.io allows us to correct for all of these common artifacts using [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9).

In addition to these artifacts, dMRI has a very low **signal-to-noise ratio (SNR)**, which directly impacts how well you can fit microstructural models and analyze diffusion data. This means that in any given location, the amount of diffusion signal is very low compared to the "noise" of the scanner. There are methods to lower the amount of noise across the brain and thus increase the SNR. [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) has the capability to identify and remove noise using principal components analysis (PCA) and Rician analysis.

On top of these issues, dMRI data is sensitive to other artifacts and non-regularities, including **gibbs ringing and bias field inhomogeneities**, which can affect the quality of the data and how well measures can be derived from the data. [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) can deal with all of these issues for us!

Finally, once the dMRI data is preprocessed and cleaned, [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) will align the dMRI data to the anatomical data. This will ensure that any analyses we do with the dMRI data will be anatomically-informed and biologically-relevant.

Useful information about the preprocessing pipeline that [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) is designed to run -- Diffusion Parameter EStimation with Gibbs and NoisE Removal (DESIGNER) -- can be found in this [original Neuroimage paper](https://www.ncbi.nlm.nih.gov/pubmed/30077743). **(<---- fixed link, double-check this is the correct pub)**

### 3. Diffusion microstructural modelling

Once the dMRI data has been cleaned and aligned to the anatomical (T1w) image, the next step is to start fitting diffusion-based models to the data. The first, and most widely-reported model, is the Diffusion Tensor (DTI) model, which attempts to model how much and in what direction water moves in the brain using a **tensor**. Regions in which water is moving **anisotropically**, or in the same direction, will have a tensor that is very cigar-shaped. Regions in which water is moving **isotropically**, or equally in all directions, will have a spherical-shaped tensor. This model outputs four different measurements: **axial diffusivity** (how strong water movement is in the primary direction of movement); **radial diffusivity** (how strong water movement is in the non-primary directions of movement); **mean diffusivity** (how strong water movement is in any direction); and **fractional anisotropy** (how strong water movement is and how directional that movement is). Fractional anisotropy (FA) and mean diffusivity (MD) are the most widely reported measures. These measures can be used for group anaylses and in tractography/tractometry pipelines, along with cortical white matter mapping pipelines. For this tutorial, we will use [FSL DTIFit](https://brainlife.io/app/5c1d67383e9c170177733d3f) to fit the DTI model to our preprocessed data.

Now, let's get to work! The following steps of this tutorial will show you how to:

1. ACPC align the anatomical (T1w) image, 
2. preprocess the dMRI data using MrTrix3 Preprocessing,
3. and fit the diffusion tensor (DTI) model to the preprocessed dMRI data.

### Copy appropriate data over from a single subject in the Bl class test data project

1. Click the following link to go to the project's page for the 'Bl class test data' project: [https://brainlife.io/project/5e6ea1a48a2089fc6d8e9fc7](https://brainlife.io/project/5e6ea1a48a2089fc6d8e9fc7)
1. Click the 'Archive' tab at the top of the screen to go to the Archives page.
1. Select the following datatypes from one subject by clicking the boxes next to the data:
    * dwi
        * Both dwi files
    * anat/t1w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project,', select your project from the drop-down menu.
    * For 'Process,' select 'Create New Process' and title it "dMRI Prep Tutorial." Hit 'Submit.'
        * This will take you to the process on your Projects page
1. Archive the data in your project by clickin the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: ACPC alignment of the anatomical (T1w) image!

### ACPC-align anatomical (T1w) image.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'HCP ACPC Alignment (T1w)'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w) image by clicking the drop-down menu and finding the appropriate dataset.
    * For template, choose 'MNI152_1mm' by clicking the drop-down menu and finding the appropriate file
    * For reorient, make sure the box is checked.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'acpc_aligned'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
    * Only have the file titled 'out.nii.gz' selected in the viewer
1. You can also generate a QA image of the results by running the 'Generate images of T1' using the ACPC-aligned anatomical image generated above! Archive the results and save with the tag 'qa t1 acpc'.

Once you're happy with the alignment, you can move onto running mrtrix3 preproc!

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

Once you're happy with the results, you can move onto fitting the diffusion tensor (DTI) model!

### Fit the DTI model to the preprocessed dMRI data.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'FSL DTIFIT'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For dwi, select the preprocessed dMRI image generated above by clicking the drop-down menu and finding the appropriate dataset.
    For shell, type '1000' to fit the model on the b=1000 shell
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'acpc_aligned'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
1. You can also generate a QA image of the tensors by running the 'Generate images of tensor' using the DTI images generated above! Archive the results and save with the tag 'qa DTI'.

**If you're happy with the results, then you have successfully finished preprocessing your dMRI data! You're now ready to move onto the next tutorial: diffusion tractography!**
