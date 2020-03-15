!!! warning
    This is a draft

## Diffusion-weighted MRI preprocessing & microstructural modelling.

This page demonstrates common steps used to preprocess diffusion magnetic resonance imaging (dMRI) data on brainlife.io. The goal of this tutorial is to show you how to process diffusion data for successive analyses, including **microstructural region analysis**, **diffusion tractography**, and **structural connectivity**. This tutorial will be mostly be using [mrtrix3-preproc](https://brainlife.io/app/5c61c69f14027a01b14adcb3) for diffusion preprocessing and [mrtrix3-dtifit](https://brainlife.io/app/5c61c69f14027a01b14adcb3) for microstructral modelling.

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) and the [Anatomical Preprocessing tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of diffusion preprocessing often involves processing the anatomical images. In order to guarantee that any generalizations regarding location made from the preprocessed diffusion data is anatomically-informed, we must have both of our anatomical (T1w or T2w) images and our diffusion MRI images **aligned**. One way we can make this easier for [mrtrix3-preproc] is by aligning the anatomical images in such a way that center of the brain is centered in the image. We refer to this as **ACPC-aligned**, as we are aligning the data to the **anterior commissure-posterior comissure plane**. This is the first step in dMRI preprocessing, and is typically done with the [hcp-acpc-alignment](https://brainlife.io/app/5c61c69f14027a01b14adcb3) app. Once we've centered our anatomical image, we can move onto diffusion MRI preprocessing.

### 2. Diffusion preprocessing 

The next step in diffusion preprocessing is to actually preprocess the diffusion data. dMRI data, like any other MRI dataset, is quite noisy. Just like fMRI, dMRI data is sensitive to **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition. Also, dMRI data is very susceptible to **motion**. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. Finally, movement of the magnetic gradients can induce electromagnetic currents which can affect the measurement in any given location. This is called **eddy currents**, and is due to the fact that electricity and magnetism are part of the same field - i.e. the *electromagnetic field*. We can correct for all of these common artifacts using [mrtrix3-preproc].

In addition to these artifacts, dMRI has very low signal-to-noise ratio (SNR), which directly impacts how well you can fit microstructural models and analyze diffusion data. This means that in any given location, the amount of diffusion signal is very low compared to the "noise" of the scanner. There are methods to lower the amount of noise across the brain, and thus increase the SNR. [mrtrix3-preproc] has the capability to identify and remove noise using principal components analysis (PCA) and Rician analysis.

On top of these issues, dMRI data is sensitive to other artifacts and non-regularities, including **gibbs ringing and bias field inhomogeneities**, which can affect the quality of the data and how well measures can be derived from the data. [mrtrix3-preproc] can deal with all of these issues for us!

Finally, once the dMRI data is preprocessed and cleaned, [mrtrix3-preproc] will align the dMRI data to the anatomical data. This will ensure that any analyses we do with the dMRI data will be anatomically-informed and biologically-relevant.

Finally, images from the fMRI scanner might not be perfectly aligned to images collected before it, such as the anatomical images. To fix this, we need to register the functional volumes to the anatomical image and map the fMRI signal onto the surfaces generated in the anatomical preprocessing step -- fMRIPrep does this registration automatically!

Useful information about preprocessing pipeline [mrtrix3-preproc] is designed to run -- Diffusion Parameter EStimation with Gibbs and NoisE Removal (DESIGNER) -- can be found in this [original Neuroimage paper](https://pubmed.ncbi.nlm.nih.gov/30077743-evaluation-of-the-accuracy-and-precision-of-the-diffusion-parameter-estimation-with-gibbs-and-noise-removal-pipeline/).

Now, let's get to work! The following steps of this tutorial will show you how to:
1. ACPC align the anatomical (T1w) image, 
2. preprocess the dMRI data using mrtrix3-preproc,
3. fit the diffusion tensor (DTI) and neurite orientation dispersion density imaging (NODDI) models to the preprocessed dMRI data.

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


## Diffusion-weighted MRI preprocessing.
This page highlights the most advanced processing pipeline to preprocess diffusion MRI (dMRI) images as used and develed by the brainlife.io team. The goal of this pipeline is to process dMRI data and fit diffusion-based models (i.e. diffusion tensor (DTI), constrained spherical deconvolution (CSD), neurite orientation dispersion density imaging (NODDI), and diffusion kurtosis (DKI)) to preprocessed dMRI data. The derivatives generated from this pipeline can be used in group analyses, or can be used in tractography pipelines. This pipeline combines functions from FSL, mrtrix, AMICO, and dipy. This pipeline is designed for multi-shell dMRI data, but can be modified for single-shell dMRI data.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


##### 1b. DESIGNER (mrtrix3).
Although topup and eddy are essential to dMRI preprocessing, they do not clean up all of the common issues with dMRI imaging. Some of these issues include gibbs ringing (i.e. XXX) and biasing of the signal (i.e. XXXX). Also, topup and eddy does not align the dMRI to the anatomical T1w image, which is a crucial step to ensure biologically-plausible results. Without this alignment, it is not possible to ensure that a volumated pixel (i.e. voxel) in the dMRI image corresponds to the correct location in the anatomical (T1w) image. Because of these issues, a more advanced processing pipeline (DESIGNER) has been proposed and implemented on brainlife.io. This app will remove the common issues with dMRI images and aligns the dMRI image to the anatomical (T1w) image. For this tutorial, we will use the DESIGNER pipeline and forgo the 1a. However, the two apps can be used interchangedly.

[brainlife.io](https://brainlife.io) provides an app that can be run on raw dMRI images to correct for common artifacts and align the cleaned dMRI image to the anatomical (T1w) image automatically: 

| ![designer](/docs/img/app.designer.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.68 |


To perform the more advanced dMRI preprocessing pipeline (i.e. DESIGNER) on the raw dMRI images, follow the following steps:
1) Stage both dMRI datasets (for each phase encoding) to a new process in the project by selecting the checkbox next to the datatypes and clicking 'Stage to process'
2) In the processes tab, click 'Submit App', search for 'mrtrix3 preprocess', and then click the app card
3) Select inputs and configuration parameters
	* Perform ACPC alignment on the raw anatomical (T1w) image (see t1w-prerocessing tutorial for more information) and select that in the 'anat/t1w' field
	* For the first dMRI input (i.e. not preprocessed), select the raw dMRI image corresponding to the 'PA' phase encoding direction that was staged to the process. For the second dMRI input, select the raw dMRI image corresponding to the 'AP phase encoding direction that was staged to the process.
	* For 'rpe', select 'merged forward and reversed sequence (all)
	* For 'acqd', select 'posterior -> anterior (+y) (PA)' from the dropdown menu
	* Leave all the other parameters as defaults
	* Select the 'Archive all output datasets when finished' box 
4) Click submit
5) Once results finish, visualize the results by clicking the eye icon next to the output dataset and choose 'fsleyes' as the viewer

##### 2. Fitting diffusion tensor (DTI) model (FSL).
Once the dMRI data has been cleaned and aligned to the anatomical (T1w) image, the next step is to start fitting diffusion-based models to the data. The first and most widely reported model is the Diffusion Tensor (DTI) model, which attempts to model how much and in what direction water moves in the brain using a tensor (i.e. cigar shaped elipsoid). Regions in which water is moving in the same direction (i.e. anisotropically) will have a tensor that is very cigar-shaped. Regions in which water is moving equally in all directions (i.e. isotropically) will have a spherical-shaped tensor. This model outputs four different measurements: axial diffusivity (i.e. how strong water movement is in the primary direction of movement), radial diffusivity (i.e. how strong water movement is in the non-primary directions of movement), mean diffusivity (i.e. how strong water movement is in any direction), and fractional anisotropy (i.e. how strong water movement is and how directional that movement is). Fractional anisotropy (FA) and mean diffusivity (MD) are the most widely reported measures. These measures can be used for group anaylses and in tractography/tractometry pipelines, along with cortical white matter mapping pipelines.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI images to fit the DTI model automatically: 

| ![dtifit](/docs/img/app.dtifit.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.137 |

To fit the DTI model, follow the following steps:
1) Perform mrtrix3 preprocessing on the DWI data. 
2) In the processes tab, click 'Submit App', search for 'FSL DTIFIT', and then click the app card
3) Select inputs and configuration parameters
	* For the 'dwi' input, select the preprocessed dMRI iamge from step 1
	* Leave the 'mask' field empty.
	* Select the 'Archive all output datasets when finished' box 
4) Click submit
5) Once results finish, visualize the results by clicking the eye icon next to the output dataset and choose 'fsleyes' as the viewer










!-->
##### 3. Fitting diffusion kurtosis (DKI) model (dipy)
The DTI model assumes that the movement of water in the brain is gaussian (i.e. bell-shaped curve distribution). However, due to the presence of different cell and tissue types, including neurons, myelin, glial cells, neurites, cerebro-spinal fluid (CSF), and blood vessels. An extension of this model, the diffusion kurtosis (i.e. DKI) model, attempts to side-step this issue by calculating how far away the distribution of water movement is from gaussian (i.e. normal). By doing this, more precise and accurate measures of water movement and brain structure can be obtained. This model gives the same four measurements as the DTI model (i.e. AD, RD, MD, FA) along with measures of kurtosis. These measures can be used for group analyses and in tractography/tractometry pipelines.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI images to fit the DKI model automatically: 

| ![dki](/docs/img/app.dki.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.9 |

To fit the DKI model, follow the following steps:
1) Perform mrtrix3 preprocessing on the DWI data. 
2) In the processes tab, click 'Submit App', search for 'Fit DKI', and then click the app card
3) Select inputs and configuration parameters
	* For the 'dwi' input, select the preprocessed dMRI iamge from step 1
	* Leave the 'mask' field empty.
	* Leave all the other parameters as is.
	* Select the 'Archive all output datasets when finished' box 
4) Click submit
5) Once results finish, visualize the results by clicking the eye icon next to the output dataset and choose 'fsleyes' as the viewer

##### 4. Fitting constrained spherical deconvolution (CSD) model (mrtrix).
DTI/DKI are not the only diffusion-based models available, and actually are not the most accurate models of water movement. Specifically, the DTI model fails in regions where white matter fibers cross. Recent estimates find that about 90% of white matter contains crossing fibers suggesting that the DTI model fails to accurately model water movement in nearly all of the white matter. This failure is due to the fact that the DTI model can only fit a single tensor for every region in the brain. In regions where fibers cross, the tensor becomes more spherical (i.e. increases in MD) and does not accurately map water movement for each of the crossing fibers. Because of this, models have been proposed that can fit multiple distributions of water movement per location in the brain. The most common of these models is constrained spherical deconvolution (i.e. CSD). This model does not give measurements like the DTI model does, but is one of the primary inputs for tractography.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI images to fit the CSD model automatically: 

| ![csd](/docs/img/app.csd.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.238 |

To fit the CSD model, follow the following steps:
1) Perform mrtrix3 preprocessing on the DWI data. 
2) In the processes tab, click 'Submit App', search for 'mrtrix3 csd', and then click the app card
3) Select inputs and configuration parameters
	* For the 'dwi' input, select the preprocessed dMRI iamge from step 1
	* Leave the 'mask' and 'mask: 5tt_masks' fields empty.
	* Select the 'Archive all output datasets when finished' box 
4) Click submit

##### 5. Fitting neurite orientation dispersion density imaging (NODDI) model (AMICO).
Both of the preceeding models attempt to characterize the movement of water using the full diffusion signal. However, this signal is comprised of multiple different cell and tissue types, including neurons, myelin, glial cells, neurites, cerebro-spinal fluid (CSF), and blood vessels. Each of these different cell types contributes differentially to the overall diffusion signal. Thus, the characterizations of water movements mapped by DTI and CSD are based on a "muddy" signal. This issue is known as partial-voluming. Because of this, more advanced models have been developed that separate the signal generated from axons and dendrites (i.e. neurites; anisotropic) from those generated from CSF, blood vessels, and glial cells (isotropic) and then map metrics of microstructural composition including the density of neurites in a given region (i.e. NDI) and the amount of "fanning" between neurites (i.e. ODI). Like the DTI model, these measures can be used in group analyses and in tractography/tractometry pipelines, along with cortical white matter mapping pipelines. This app requires multi-shell data for fitting the NDI measure accurately. However, ODI can be fit accurately using just a single-shell.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI data to fit the NODDI model automatically: 

| ![noddi](/docs/img/app.noddi.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.117 |

To fit the NODDI model, follow the following steps:
1) Perform mrtrix3 preprocessing on the DWI data. 
2) In the processes tab, click 'Submit App', search for 'Noddi Amico', and then click the app card
3) Select inputs and configuration parameters
	* For the 'dwi' input, select the preprocessed dMRI iamge from step 1
	* Leave the 'mask' field empty.
	* Leave all the other parameters as is.
	* Select the 'Archive all output datasets when finished' box 
4) Click submit
5) Once results finish, visualize the results by clicking the eye icon next to the output dataset and choose 'fsleyes' as the viewer
<--
