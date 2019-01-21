# BrainLife.io Tutorials

This page introduces a series of commonly used steps for data processing. 

## Diffusion-weighted MRI, Tractography and Tractometry.
### TractSeg Diffusion Processing
This pipeline combines processing proposed by Wasserthal et al., Neuroimage (2018).

1. Brain Parcellation (FreeSurfer).
https://doi.org/10.25663/bl.app.0

2. Segment major white matter tracts and volumes (TractSeg).
https://doi.org/10.25663/bl.app.95

3. Fit Diffusion Tensor model.
https://doi.org/10.25663/brainlife.app.137
https://doi.org/10.25663/bl.app.60

4. Measure Tract Profiles (VISTASOFT).
https://doi.org/10.25663/bl.app.43


This pipeline assumes that your data are compliant with the Human Connectome Project pipeline and that your data is in LAS and the BVECS / BVALS are normalized. If your data does not comply with the above you can run the following steps before Step 1.

0.1 Check BVECS and BVALS.
https://doi.org/10.25663/bl.app.85

0.2 Reorient BVECS and Normalize BVALS (if suggested by the previous step).
https://doi.org/10.25663/bl.app.4

0.3 Select a shell to run your tracking.
https://doi.org/10.25663/bl.app.17


### Ensemble tractography Single-Shell Diffusion and Tractography Processing.
This pipeline combines processing proposed by Caiafa and Pestilli, Scientific Reports (2017) and Takemura et al., PLoS Computational Biology (2016).

1. Brain Parcellation (FreeSurfer).
https://doi.org/10.25663/bl.app.0

2. Diffusion MRI Preprocessing (VISTASOFT).
https://doi.org/10.25663/bl.app.3

3. Generate Diffusion Tensor Model data.
https://doi.org/10.25663/bl.app.48 

4. Tractography (Ensemble Tractography).
https://doi.org/10.25663/bl.app.33

3. Segment white matter Tracts (AFQ).
https://doi.org/10.25663/bl.app.13

4. Measure Tract Profiles (VISTASOFT).
https://doi.org/10.25663/bl.app.43

This pipeline assumes that your data are compliant with the Human Connectome Project pipeline and that your data is in LAS and the BVECS / BVALS are normalized. If your data does not comply with the above you can run the following steps before Step 1.

0.1 Check BVECS and BVALS.
https://doi.org/10.25663/bl.app.85

0.2 Reorient BVECS and Normalize BVALS (if suggested by the previous step).
https://doi.org/10.25663/bl.app.4

0.3 Select a shell to run your tracking.
https://doi.org/10.25663/bl.app.17

### Ensemble tractography Multishell Diffusion Processing using DESIGNER
This pipeline combines processing proposed by Ades-Aron et al., Neuroimage (2018) and Takemura et al., PLoS Computational Biology (2016).

1. Brain Parcellation (FreeSurfer).
https://doi.org/10.25663/bl.app.0

2. Diffusion MRI Preprocessing (DESIGNER).
https://doi.org/10.25663/bl.app.68

3. Tractography (Ensemble Tractography).
https://doi.org/10.25663/bl.app.101
This also generates a DTI fit.

3. Segment white matter Tracts (AFQ).
https://doi.org/10.25663/bl.app.13

4. Measure Tract Profiles (VISTASOFT).
https://doi.org/10.25663/bl.app.43

5.1 Advanced: Generate NODDI model parameters.
https://doi.org/10.25663/brainlife.app.117

5.2 Advanced: Generate DKI model parameters.
https://doi.org/10.25663/bl.app.9
https://doi.org/10.25663/bl.app.70


## Network Neuroscience.
### Structural networks


### Functional networks.
