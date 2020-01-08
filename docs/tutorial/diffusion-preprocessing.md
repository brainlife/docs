!!! warning
    This is a draft

## Diffusion-weighted MRI preprocessing.
This page highlights the most advanced processing pipeline to preprocess diffusion MRI (dMRI) images as used and develed by the brainlife.io team. The goal of this pipeline is to process dMRI data and fit diffusion-based models (i.e. diffusion tensor (DTI), constrained spherical deconvolution (CSD), neurite orientation dispersion density imaging (NODDI), and diffusion kurtosis (DKI)) to preprocessed dMRI data. The derivatives generated from this pipeline can be used in group analyses, or can be used in tractography pipelines. This pipeline combines functions from FSL, mrtrix, AMICO, and dipy. This pipeline is designed for multi-shell dMRI data, but can be modified for single-shell dMRI data.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

##### 1a. Topup & eddy (FSL).
The first step is to remove common issues with dMRI images, including excess motion (i.e. subject moves their head in the scanner), eddy currents (i.e. a current generated from the rotating magnetic field in the scanner), and susceptibility-related artifacts (i.e. blurring of the image based on how the image was acquired). A popular set of tools to do this is FSL's topup and eddy functions. These tools will remove these artifacts and return a cleaned dMRI image that can be used in future processing. 

[brainlife.io](https://brainlife.io) provides a service (App) that can be run on raw dMRI images to correct for motion, eddy, and susceptibility-related artifacts automatically: 

| ![topup-eddy](/docs/img/app.topup-eddy.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.155 |

##### 1b. DESIGNER (mrtrix3).
Although topup and eddy are essential to dMRI preprocessing, they do not clean up all of the common issues with dMRI imaging. Some of these issues include gibbs ringing (i.e. XXX) and biasing of the signal (i.e. XXXX). Also, topup and eddy does not align the dMRI to the anatomical T1w image, which is a crucial step to ensure biologically-plausible results. Without this alignment, it is not possible to ensure that a volumated pixel (i.e. voxel) in the dMRI image corresponds to the correct location in the anatomical (T1w) image. Because of these issues, a more advanced processing pipeline (DESIGNER) has been proposed and implemented on brainlife.io. This app will remove the common issues with dMRI images and aligns the dMRI image to the anatomical (T1w) image. For this tutorial, we will use the DESIGNER pipeline and forgo the 1a. However, the two apps can be used interchangedly.

[brainlife.io](https://brainlife.io) provides an app that can be run on raw dMRI images to correct for common artifacts and align the cleaned dMRI image to the anatomical (T1w) image automatically: 

| ![designer](/docs/img/app.designer.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.68 |

##### 2. Fitting diffusion tensor (DTI) model (FSL).
Once the dMRI data has been cleaned and aligned to the anatomical (T1w) image, the next step is to start fitting diffusion-based models to the data. The first and most widely reported model is the Diffusion Tensor (DTI) model, which attempts to model how much and in what direction water moves in the brain using a tensor (i.e. cigar shaped elipsoid). Regions in which water is moving in the same direction (i.e. anisotropically) will have a tensor that is very cigar-shaped. Regions in which water is moving equally in all directions (i.e. isotropically) will have a spherical-shaped tensor. This model outputs four different measurements: axial diffusivity (i.e. how strong water movement is in the primary direction of movement), radial diffusivity (i.e. how strong water movement is in the non-primary directions of movement), mean diffusivity (i.e. how strong water movement is in any direction), and fractional anisotropy (i.e. how strong water movement is and how directional that movement is). Fractional anisotropy (FA) and mean diffusivity (MD) are the most widely reported measures. These measures can be used for group anaylses and in tractography/tractometry pipelines, along with cortical white matter mapping pipelines.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI images to fit the DTI model automatically: 

| ![dtifit](/docs/img/app.dtifit.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.137 |

##### 3. Fitting diffusion kurtosis (DKI) model (dipy)
The DTI model assumes that the movement of water in the brain is gaussian (i.e. bell-shaped curve distribution). However, due to the presence of different cell and tissue types, including neurons, myelin, glial cells, neurites, cerebro-spinal fluid (CSF), and blood vessels. An extension of this model, the diffusion kurtosis (i.e. DKI) model, attempts to side-step this issue by calculating how far away the distribution of water movement is from gaussian (i.e. normal). By doing this, more precise and accurate measures of water movement and brain structure can be obtained. This model gives the same four measurements as the DTI model (i.e. AD, RD, MD, FA) along with measures of kurtosis. These measures can be used for group analyses and in tractography/tractometry pipelines.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI images to fit the DKI model automatically: 

| ![dki](/docs/img/app.dki.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.9 |

##### 4. Fitting constrained spherical deconvolution (CSD) model (mrtrix).
DTI/DKI are not the only diffusion-based models available, and actually are not the most accurate models of water movement. Specifically, the DTI model fails in regions where white matter fibers cross. Recent estimates find that about 90% of white matter contains crossing fibers suggesting that the DTI model fails to accurately model water movement in nearly all of the white matter. This failure is due to the fact that the DTI model can only fit a single tensor for every region in the brain. In regions where fibers cross, the tensor becomes more spherical (i.e. increases in MD) and does not accurately map water movement for each of the crossing fibers. Because of this, models have been proposed that can fit multiple distributions of water movement per location in the brain. The most common of these models is constrained spherical deconvolution (i.e. CSD). This model does not give measurements like the DTI model does, but is one of the primary inputs for tractography.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI images to fit the CSD model automatically: 

| ![csd](/docs/img/app.csd.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.238 |

##### 5. Fitting neurite orientation dispersion density imaging (NODDI) model (AMICO).
Both of the preceeding models attempt to characterize the movement of water using the full diffusion signal. However, this signal is comprised of multiple different cell and tissue types, including neurons, myelin, glial cells, neurites, cerebro-spinal fluid (CSF), and blood vessels. Each of these different cell types contributes differentially to the overall diffusion signal. Thus, the characterizations of water movements mapped by DTI and CSD are based on a "muddy" signal. This issue is known as partial-voluming. Because of this, more advanced models have been developed that separate the signal generated from axons and dendrites (i.e. neurites; anisotropic) from those generated from CSF, blood vessels, and glial cells (isotropic) and then map metrics of microstructural composition including the density of neurites in a given region (i.e. NDI) and the amount of "fanning" between neurites (i.e. ODI). Like the DTI model, these measures can be used in group analyses and in tractography/tractometry pipelines, along with cortical white matter mapping pipelines. This app requires multi-shell data for fitting the NDI measure accurately. However, ODI can be fit accurately using just a single-shell.

[brainlife.io](https://brainlife.io) provides an app that can be run on cleaned dMRI data to fit the NODDI model automatically: 

| ![noddi](/docs/img/app.noddi.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/brainlife.app.117 |
