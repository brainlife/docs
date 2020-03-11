!!! warning
    This is a draft

## Diffusion-weighted MRI, Tractography and Tractometry.
This page highlights some of the most common processing pipelines to analyze diffusion-weighted MRI and white matter as used and developed by the brainlife.io team.

### Ensemble tractography Single-Shell Diffusion and Tractography Processing.
The goal of this pipeline is to process dMRI and T1w data to generate tract profiles using the Diffusion Tensor (DTI) model. This pipeline combines processing proposed by Caiafa and Pestilli, Scientific Reports (2017), Yeatman et al., PloS One (2012), and Takemura et al., PLoS Computational Biology (2016).

The pipeline is optimized for single-shell dMRI data and should not be used for multishell data.

##### 1. Brain Parcellation (FreeSurfer).
The first step is to launch FreeSurfer, this will perform anatomical preprocessing and parcellate the brain into different regions: [https://doi.org/10.25663/bl.app.0](https://doi.org/10.25663/bl.app.0)

##### 2. Diffusion MRI Preprocessing (VISTASOFT).
A commonly used pipeline for dMRI processing is called dtiInit. Originally developed by VistaLab at Stanford Universty, this processing will align the dMRI data to the T1w data, reorient the BVECS accordingly, remove motion artifacts, and reduce Eddy- Current artifacts.
[https://doi.org/10.25663/bl.app.3](https://doi.org/10.25663/bl.app.3)

##### 3. Generate Diffusion Tensor Model data.
Once the dMRI data have been processed, we need to generate the parameters for the DTI model. These were generated in the previous step. The following app extracts the parameters and makes them available to build tract profiles.
[https://doi.org/10.25663/bl.app.48](https://doi.org/10.25663/bl.app.48)

##### 4. Tractography (Ensemble Tractography).
The ensemble tractography app will take the output of the Diffusion MRI preprocessing app and perform tracking using a variety of parameters, combining probabilistic as well as deterministic tracking. The pipeline is a simplified version of the procedure described by Takemura et al., PLoS Computational Biology (2016). 
[https://doi.org/10.25663/bl.app.33](https://doi.org/10.25663/bl.app.33)

##### 3. Segment white matter Tracts (AFQ).
Once the dMRI and T1w data are preprocessed, tracking has been generated. We can now segment the tractogram (whole brain fiber group) into major white matter tracts such as the Arcuate Fasciculus, Corticospinal Tract, Etc.
[https://doi.org/10.25663/bl.app.13](https://doi.org/10.25663/bl.app.13)

##### 4. Measure Tract Profiles (VISTASOFT).
Tract profiles will allow for measuring the properties of the brain tissue along the major white matter tracts that were generated in the previous step. These are ultimately the measurements the investigator will want to use to understand brain function by comparing the values across individuals.
[https://doi.org/10.25663/bl.app.43](https://doi.org/10.25663/bl.app.43)

##### Note. 
The above pipeline assumes that your data are compliant with VISTASOFT or the Human Connectome Project pipeline and that your data is in LAS and the BVECS and BVALS are normalized. If your data does not comply with the above, you can run the following steps before Step 1.

###### 0.1 Check BVECS and BVALS.
[https://doi.org/10.25663/bl.app.85](https://doi.org/10.25663/bl.app.85)

###### 0.2 Reorient BVECS and Normalize BVALS (if suggested by the previous step).
[https://doi.org/10.25663/bl.app.4](https://doi.org/10.25663/bl.app.4)

###### 0.3 Select a shell to run your tracking.
[https://doi.org/10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17)

### Ensemble tractography Multishell Diffusion Processing using DESIGNER
Today, some of the most common dMRI data are acquired with multiple shells. This allows fitting models to the diffusion signal with additional paramters to better capture the complexity of the signal in areas of fiber crossing. The pipeline described below will exploit this modern dMRI data type and performs tractometry.  

##### 1. Brain Parcellation (FreeSurfer).
The first step is to launch FreeSurfer, this will perform anatomical preprocessing and parcellate the brain into different regions: [https://doi.org/10.25663/bl.app.0](https://doi.org/10.25663/bl.app.0)

##### 2. Diffusion MRI Preprocessing (DESIGNER).
This is a series of processing steps that will remove motion artifacts, denoise the data, and generate files ready for tractography. The pipeline is described in detail by Ades-Aron et al., Neuroimage (2018).
[https://doi.org/10.25663/bl.app.68](https://doi.org/10.25663/bl.app.68)

##### 3. Tractography (Ensemble Tractography).
The ensemble tractography app will take as the output of the Diffusion MRI preprocessing app and perform tracking using a variety of parameters, combining probabilistic as well as deterministic tracking. The pipeline is a simplified version of the procedure described by Takemura et al., PLoS Computational Biology (2016). We note that the output of this step will also contain a diffusion tensr model fit (DTI).
[https://doi.org/10.25663/bl.app.101](https://doi.org/10.25663/bl.app.101)

##### 3. Segment white matter Tracts (AFQ).
Once the dMRI and T1w data are preprocessed, tracking has been generated. We can now segment the tractogram (whole brain fiber group) into major white matter tracts such as the Arcuate Fasciculus, Corticospinal Tract, Etc.
[https://doi.org/10.25663/bl.app.13](https://doi.org/10.25663/bl.app.13)

##### 4. Measure Tract Profiles (VISTASOFT).
Tract profiles will allow for measuring the properties of the brain tissue along the major white matter tracts that were generated in the previous step. These are ultimately the measurements the investigator will want to use to understand brain function by comparing the values across individuals.
[https://doi.org/10.25663/bl.app.43](https://doi.org/10.25663/bl.app.43)

##### Advanced white matter microscopic tissue properties.
A variety of methods and models for mapping white matter tissue microstructure are available as apps on brainlife.io. Below, we describe two additional apps that allow fitting more advanced models to dMRI data measured with multiple shells. These models can be used as an alternative to the DTI model, as well as in combination with the [Tract profiles app](https://doi.org/10.25663/bl.app.43) to measure white matter for each of the tract segmented.

###### 5.1 Advanced: Generate NODDI model parameters.
The Neurite Orientation Dispersion Diffusion model (NODDI) was originally proposed by [Zhang and colleagues Neuroimage (2012)](https://doi.org/10.1016/j.neuroimage.2012.03.072). The app below implements the model in a simplified (linearized) form as proposed by Daducci et al., Neuroimage (2015). NODDI provides additional measures of white matter tissue properties, such as Neurite Dispersion and intra-cellular volume fraction. 
[https://doi.org/10.25663/brainlife.app.117](https://doi.org/10.25663/brainlife.app.117)

###### 5.2 Advanced: Generate DKI model parameters.
The Diffusion Kurtosis Imaging (DKI; [Jensen et al., MRM 2005](https://doi.org/10.1002/mrm.20508)) model provides additional (high-order) parameters that describe components of the dMRI signal that cannot be accounted for by the single-gaussian (simpler) DTI model. By running the two apps below in the following sequence it is possible to generated DKI paramters to be used for generating advanced tract profiles.
[https://doi.org/10.25663/bl.app.9](https://doi.org/10.25663/bl.app.9)
[https://doi.org/10.25663/bl.app.70](https://doi.org/10.25663/bl.app.70)

### TractSeg Diffusion Processing, White matter Tracts and Tractography
This pipeline combines processing proposed by Wasserthal et al., Neuroimage (2018), with processing proposed by Yeatman et al., PLoS One (2012). The pipeline as described below is optimized for single-shell dMRI data.

##### 1. Brain Parcellation (FreeSurfer).
The first step is to launch FreeSurfer. This will perform anatomical preprocessing and parcellate the brain into different regions: [https://doi.org/10.25663/bl.app.0](https://doi.org/10.25663/bl.app.0)

##### 2. Segment major white matter tracts and volumes (TractSeg).
[Wasserthal and colleagues, Neuroimage (2018)](https://doi.org/10.1016/j.neuroimage.2018.07.070) proposed a single method to generate tract volumes from a pre-trained neural network trained on the Human Connectome Dataset. This app uses the pipeline presented in their article to segment the tracts and perform tracking.
[https://doi.org/10.25663/bl.app.95](https://doi.org/10.25663/bl.app.95)

##### 3. Fit Diffusion Tensor model.
Once the tracts are segmented we can use FSL ([Jenkinson et al., Neuroimage (2012)](https://doi.org/10.1016/j.neuroimage.2011.09.015)) to fit the DTI model and use that model for measuring tract profiles.
[https://doi.org/10.25663/brainlife.app.137](https://doi.org/10.25663/brainlife.app.137)

The following is an alternative app to make the DTI model compatible with the TractSeg pipeline: [https://doi.org/10.25663/bl.app.60](https://doi.org/10.25663/bl.app.60)

##### 4. Measure Tract Profiles (VISTASOFT).
Tract profiles will allow for measuring the properties of the brain tissue along the major white matter tracts that were generated in the previous step. These are ultimately the measurements the investigator will want to use to understand brain function by comparing the values across individuals.
[https://doi.org/10.25663/bl.app.43](https://doi.org/10.25663/bl.app.43)

##### Note. 
This pipeline assumes that your data are compliant with the Human Connectome Project pipeline and that your data is in LAS and the BVECS and BVALS are normalized. If your data does not comply with the above, you can run the following steps before Step 1.

###### 0.1 Check BVECS and BVALS.
[https://doi.org/10.25663/bl.app.85](https://doi.org/10.25663/bl.app.85)

###### 0.2 Reorient BVECS and Normalize BVALS (if suggested by the previous step).
[https://doi.org/10.25663/bl.app.4](https://doi.org/10.25663/bl.app.4)

##### 0.3 Select a shell to run your tracking.
[https://doi.org/10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17)
