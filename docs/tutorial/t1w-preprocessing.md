!!! warning
    This is a draft

## Anatomical (T1w) preprocessing.

This page highlights the most common steps used to preprocess anatomical magentic resonance imaging data (T1-weighted or T1w) on brainlife.io The goal of this pipeline is to process anatomical data for successive analyses – volumetric analyses from T1w meausres, combination of T1w and diffusion-weighted MRI (dMRI) or functional neuroimaging data (fMRI). pipelines. This pipelines combines functions from FSL, Automatic Registration Toolbox (ART), and Freesurfer.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hC0Ms3KWD8o" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### (1) Set the orientation of the brain.

The first step is to make sure the anatomy of the brain 'looks right.' This means that the coordinate system for the T1w image is stored rotated such that the brain appears up-right and centered when opened in the major visualization toolboxes that the scientific community uses (i.e., generally we look at images where the nose faces towards right-hand side in the sagittal plane, and forward in the axial and coronal planes). This rotation involves changing information in the T1w data files and headers. Beside agreeing to the standard convetions, this orientation is important because it assures that the data inside the T1w iamges will be "indexed,"  or read, appropirately allowing different software libraries for neuroimaging data analysis to  read the data, interpret the position and dimensions of individual three-dimensional pixels (i.e., voxels). During this process oftentimes the neck is removed from the original MRI data, so to fit the image within an adequate volume.  

Useful information about MRI image orientation can be found at: 
  - https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Orientation%20Explained 
  - http://gru.stanford.edu/doku.php/mrTools/coordinateTransforms
  
[brainlife.io](https://brainlife.io) provides a service (App) that can be run on raw T1w to rop and orient the image automatically: 

| |![crop-reorient](/docs/img/app.crop-reorient.bl.header.png)|
|------------------------------------|
| https://doi.org/10.25663/bl.app.15 |


#### (2) Anterior Commissure - Posterior Comissure (ACPC) alignment (Art,FSL).

The next step is to align the anatomical image to the point where two white matter tracts cross hemispheres: the anterior commissure and posterior commissure (ACPC). The anterior commissure connects the two temporal lobes and is located below the anterior portion of the corpus callosum (i.e. fornix), the large white matter tract connecting the two hemispheres. The posterior commissure is located just behind the cerebral aqueduct, an important connection between the third and fourth ventricles. This step will move the image in a way that effectively "centers" the image. Specifically, the image will be moved so that a horizontal line drawn from the front to the back of the brain will pass through both landmarks, and a horizontal line drawn from the temporal lobes and a vertical line from the top of the brain to the bottom bisect at the midpoint of the two landmarks. A majority of the following processing steps require the anatomical image to be aligned to the ACPC plane, including Freesurfer parcellation. There are two different apps to perform this step: one using the Automatic Registration Toolbox (ART), the other using FSL and the Human Connectome Project (HCP) preprocessing pipeline. We recommend using the HCP-based app as it is more modern and generally performs better than ART.
ART: https://doi.org/10.25663/bl.app.16
HCP: https://doi.org/10.25663/bl.app.99

![art](/docs/img/app.art.bl.header.png)
![hcp-acpc](/docs/img/app.hcp-acpc.bl.header.png)

#### 3. Brain Parcellation (FreeSurfer).

The next step is perform anatomical preprocessing and divide (i.e. parcellate) the brain into different regions and tissue types (i.e. white matter, gray matter) using Freesurfer. These regions represent different gray matter (i.e. cortical) landmarks such as the motor and somatosensory cortices, and are typically derived from histological properties of these regions. Sometimes, they are derived from functional activation (i.e. fMRI) patterns. Freesurfer will divide the anatomical image into different tissue types (i.e. white matter, gray matter), parcellate the gray matter into known anatomical landmarks, and output statistics regarding the volume and thickness of the different tissue types and anatomical landmarks. These regions are then used for a large number of downstream steps, including white matter tract segmentation. Information regarding the volume and thickness of each region can also be used for group analyses.
https://doi.org/10.25663/bl.app.0
![freesurfer](/docs/img/app.freesurfer.bl.header.png)

#### 4. Atlas transfer (Freesufer).

The final step in the anatomical preprocessing (T1w) pipeline is to fit different parcellations to the Freesufer-generated parcellation. This is useful as new parcellations come out relatively frequently, each generated based on different histological or functional properties. We recommend mapping the Glasser-derived 180 node parcellation, as it was derived from both histological and functional properties (Glasser MF, Coalson TS, Robinson EC, et al. A multi-modal parcellation of human cerebral cortex. Nature. 2016;536(7615):171–178. doi:10.1038/nature18933).
https://doi.org/10.25663/bl.app.23

![matt](/docs/img/app.matt.bl.header.png)

