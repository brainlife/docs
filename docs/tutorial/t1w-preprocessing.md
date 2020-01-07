!!! warning
    This is a draft

## Anatomical (T1w) preprocessing
This page highlights the most common processing pipelines to preprocess anatomical (T1w) images as used and developed by the brainlife.io team. The goal of this pipeline is to process anatomical (T1w) data for volumetric analyses and future diffusion MRI (dMRI) pipelines. This pipelines combines functions from FSL, Automatic Registration Toolbox (ART), and Freesurfer.
<img src="https://some-img-host.com/1234567/image.png" width=300 align=right>(https://www.youtube.com/watch?v=hC0Ms3KWD8o "Anatomical Preprocessing")

##### 1. Crop & reorientation of T1w (FSL).
The first step is to make sure the anatomical (T1w) has been cropped to remove the neck and reoriented to the neurological orientation, which is FSL's and many other software's orientation of choice. This step does not need to be performed if the neck has already been removed and the image is in the proper orientation. If not sure if it's in the proper orientation, see https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Orientation%20Explained for help.
https://doi.org/10.25663/bl.app.15
![crop-reorient](/docs/img/app.crop-reorient.bl.header.png)

##### 2. Anterior Commissure - Posterior Comissure (ACPC) alignment (Art,FSL).
The next step is to align the anatomical image to the anterior commissure-posterior commissure (ACPC) plane. This is performed as most down-stream apps, especially those requiring Vistasoft (https://github.com/vistalab/vistasoft), require the data to be aligned to this plane. This step is also optional, but may lead to improper results further downstream. There are two different apps to perform this step: one using the Automatic Registration Toolbox (ART), the other using FSL and the Human Connectome Project (HCP) preprocessing pipeline. We recommend using the HCP-based app as it is more modern and generally performs better than ART.
ART: https://doi.org/10.25663/bl.app.16
HCP: https://doi.org/10.25663/bl.app.99
![art](/docs/img/app.art.bl.header.png)
![hcp-acpc](/docs/img/app.hcp-acpc.bl.header.png)

##### 3. Brain Parcellation (FreeSurfer).
The next step is perform anatomical preprocessing and parcellate the brain into different regions (i.e. parcellations) using Freesurfer. These regions are then used for a large number of downstream steps, including white matter tract segmentation. Information regarding the volume and thickness of each region is provided and can be used for group analyses.
https://doi.org/10.25663/bl.app.0
![freesurfer](/docs/img/app.freesurfer.bl.header.png)

##### 4. Atlas transfer (Freesufer).
The final step in the anatomical preprocessing (T1w) pipeline is to fit different parcellations to the Freesufer-generated parcellation. This is useful as new parcellations come out relatively frequently, each generated based on different anatomical or functional properties. We recommend mapping the Glasser-derived 180 node parcellation (Glasser MF, Coalson TS, Robinson EC, et al. A multi-modal parcellation of human cerebral cortex. Nature. 2016;536(7615):171–178. doi:10.1038/nature18933).
https://doi.org/10.25663/bl.app.23
![matt](/docs/img/app.matt.bl.header.png)
