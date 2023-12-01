!!! warning
    This is a draft

## Diffusion-weighted MRI preprocessing.

This page demonstrates common steps used to perform diffusion tractography analyses on brainlife.io. The goal of this tutorial is to show you how to process anatomical and diffusion data to [generate tractograms](https://brainlife.io/app/5aac2437f0b5260027e24ae1), [segment white matter tracts](https://brainlife.io/app/5cc73ef44ed9df00317f6288), and [map microstructural measures](https://brainlife.io/app/5cc210ce4ed9df00317f61cf) for the major white matter tracks.

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/), the [Anatomical Preprocessing tutorial](https://brainlife.io/docs/tutorial/t1w-preprocessing/), and the [Diffusion MRI Preprocessing tutorial](https://brainlife.io/docs/tutorial/diffusion-preprocessing/) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of diffusion tractography often involves processing the anatomical images. To track in a biologically-informed manner, and to extract major white matter tracts, we must have both anatomical (T1w or T2w) images and diffusion MRI images **aligned**. One way we can make this easier for [QSIPrep](https://brainlife.io/app/5dc1c2e57f55b85a93bd3021) is by aligning the anatomical images so that the center of the brain is centered in the image. We refer to this as **ACPC-aligned**, as we are aligning the data to the **anterior commissure-posterior comissure plane**. This is the first step in dMRI preprocessing, and it is typically done with the [FSL Anat (T1)](https://brainlife.io/app/5e3c87ae9362b7166cf9c7f4) app. We will then need to generate cortical and white matter surfaces and brain region parcellations using [Freesurfer](https://brainlife.io/app/5fe1056057aacd480f2f8e48). These will be used for segmenting the major white matter tracts following tractography. Once we've processed our anatomical image, we can move on to diffusion MRI preprocessing.

### 2. Diffusion preprocessing

The next step in diffusion preprocessing is to actually preprocess the diffusion data. dMRI data, like any other MRI dataset, is quite noisy. Just like fMRI, dMRI data is sensitive to **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition. Also, dMRI data is very susceptible to motion. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. Finally, movement of the magnetic gradients can induce electromagnetic currents which can affect the measurement in any given location. These electromagnetic currents are called **eddy currents**, and they are due to the fact that electricity and magnetism are part of the same field, otherwise known as the **electromagnetic field**. Thankfully, brainlife.io allows us to correct for all of these common artifacts using [QSIPrep](https://brainlife.io/app/5dc1c2e57f55b85a93bd3021).

In addition to these artifacts, dMRI has a very low **signal-to-noise ratio (SNR)**, which directly impacts how well you can fit microstructural models and analyze diffusion data. This means that in any given location, the amount of diffusion signal is very low compared to the "noise" of the scanner. There are methods to lower the amount of noise across the brain and thus increase the SNR. [QSIPrep](https://brainlife.io/app/5dc1c2e57f55b85a93bd3021) has the capability to identify and remove noise using principal components analysis (PCA) and Rician analysis.

On top of these issues, dMRI data is sensitive to other artifacts and non-regularities, including **gibbs ringing and bias field inhomogeneities**, which can affect the quality of the data and how well measures can be derived from the data. [QSIPrep](https://brainlife.io/app/5dc1c2e57f55b85a93bd3021) can deal with all of these issues for us!

Finally, once the dMRI data is preprocessed and cleaned, [QSIPrep](https://brainlife.io/app/5dc1c2e57f55b85a93bd3021) will align the dMRI data to the anatomical data. This will ensure that any analyses we do with the dMRI data will be anatomically-informed and biologically-relevant.

Useful information about the [QSIPrep](https://brainlife.io/app/5dc1c2e57f55b85a93bd3021) preprocessing pipeline can be found in this [original Nature Methods paper](https://www.nature.com/articles/s41592-021-01185-5).

### 3. Diffusion modeling & anatomically-informed tractography.

Once the dMRI data has been cleaned and aligned to the anatomical (T1w) image, the next step is to map the underlying white matter anatomy using diffusion tractography. Diffusion tractography works by modeling how water is moving throughout the brain based on the principle of **anisotropy** -- that water will move unequally in a particular direction given a physical constraint. In the case of diffusion MRI, this constraint comes from the myelin wrapping the many axons arranged into bundles known as **fascicles**. Diffusion tractography generates **streamlines** that act as evidence of underlying white matter organization. We can bundle these streamlines into common white matter tracts digitally, just as we can bundle fascicles into **tracts** using histological methods.

The first step is to fit models of diffusion at each location in the dMRI image that can be used as a guide for the tractography algorithms. The most popular model for this is known as **constrained spherical deconvolution**, or **CSD**. CSD allows for the possibility of multiple fascicles entering a single location that may have different trajectories, which is a major advantage of this model over other models of diffusion. The CSD model at each location can tell the tracking algorithm _how strongly_ and in _what direction_ water is diffusing in the brain, and thus where organized and myelinated white matter is likely to be.

Once we know the direction of water movement using the CSD model, we can then perform **diffusion tractography** to map the underlying organized white matter. Diffusion tractography *sews* together regions with common directions of water movement into streamlines that provide evidence for organized, myelinated white matter. These operations can be done millions of times across the entire brain to form **tractograms**. There are many algorithms for doing this that fall into two main categories: **deterministic** and **probabilistic**. **Deterministic** algorithms work by strictly following the directions of water movement, while **probabilistic** algorithms infer a probability of different directions of water movement at any given location. Deterministic tractography tends to be an overly conservative representation of the white matter, as modeling is never perfectly accurate. Probabilistic tractography tends to provide *hairy* streamline representations, as the model allows for a probability of different directions of water movement at any given location. Recently, it was discovered that combining both algorithms, or **ensembling** them, provides the most anatomically-accurate tractograms.

On brainlife.io, we have many options for performing tractography! For this tutorial, we will focus on anatomically-constrained tractography, using the [mrtrix3 - WMC Anatomically Constrained Tractography (ACT)](https://brainlife.io/app/5e9dced9f1745d6994f692c0) app, which restricts streamline representations to those that are biologically-plausible (i.e. not crossing CSF or gray matter)! Within this app, both the CSD and DTI models will be fit to the diffusion data and returned as outputs!

More information on ensemble tractography can be found in this [PLOS Computational Biology article](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004692) paper. More information on **ACT** can be found in this [Neuroimage publication](https://www.ncbi.nlm.nih.gov/pubmed/22705374).

### 4. Major white matter tract segmentation.

Once our anatomical brain parcellations and whole-brain tractograms are generated, the next step is to segment the tractogram into known **major white matter tracts**. To do this, we need both information about the terminations of each streamline into the cortex and histologically-derived definitions for each major white matter tract. The histological definitions of major white matter tracts allow us to group the streamlines in our tractograms into major white matter tracts. On brainlife.io, we have developed an automated [white matter segmentation](https://brainlife.io/app/5cc73ef44ed9df00317f6288) app that will segment our whole-brain tractograms into 61 known major white matter tracts.

Following segmentation, it is common practice to remove outlier streamlines based on their distance away from the 'core' of the tract. On brainlife.io, we have developed an automated [remove outliers](https://brainlife.io/app/5cc9eca04b5e4502275edba6) app that will remove outlier streamlines based on their distance away from the center of the 'core' of the tract.

More information on the segmentation algorithm can be found in this [Brain Structure and Function paper](https://www.ncbi.nlm.nih.gov/pubmed/31342157).

### 5. Macro-and-Microstructural statistics of major white matter tracts.

Once we have segmented our tractograms into major white matter tracts and fit the DTI model to our dMRI data, we can now compute macrostructural (i.e. volume, length, streamline count) statistics of each tract and map the microstructural information to our major white matter tracts. For microstructural mapping, this is done by computing the average microstructural measure at multiple locations along each major white matter tract. This process generates a plot known as a **tract profile**, which provides microstructural information at different locations along a tract. These profiles can be used to examine microstructural properties along different tracts that might help distinguish different groups, such as those with a neurodegenerative disease versus those without the disease. We have developed an automated [microstructural mapping](https://brainlife.io/app/5ed02b780a8ed88a57482c92) app on brainlife.io that will map multiple microstructural measures. To compute macrostructural statistics, we have developed an automated [macrostructural statistics](https://brainlife.io/app/5cc9c6b44b5e4502275edb4b) app on brainlife that will compute multiple macrostructural measures.

More information on **tract profiles** can be found in this [PLOS One article](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0049790).

Now, let's get to work! The following steps of this tutorial will show you how to:

1. Preprocess the anatomical (T1w) image,
1. Generate Freesurfer Brain parcellation,
1. preprocess the dMRI data using QSIPrep,
1. fit the CSD & DTI models, perform tractography using MRTrix3 ACT,
1. segment major white matter tracts,
1. Remove outlier streamlines,
1. Map microstructural (DTI) measures along each tract,
1. Compute macrostructural statistics,
1. and compile macro-structural and micro-structural data into a single csv for analysis

### Copy appropriate data over from a single subject in the Tutorial data project

1. Click the following link to go to the project's page for the [Tutorial](https://brainlife.io/project/5ae916e7f446980028b15eb3)
1. Click the 'Archive' tab at the top of the screen to go to the Archives page.
1. Select the following datatypes from one subject by clicking the boxes next to the data for subject '10217':
    * neuro/dwi
    * anat/t1w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project', select your project from the drop-down menu.
    * For 'Process,' select 'Create New Process' and title it "dMRI Prep Tutorial." Hit 'Submit.'
        * This will take you to the process on your Projects page
1. Archive the data in your project by clicking the 'Archive' button next to each dataset.

Your data should now be staged for processing and archived in your projects page! You're now ready to move onto the first step: preprocessing of the anatomical (T1w) image!

### Preprocess anatomical (T1w) data using FSL.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'FSL Anat (T1)'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the staged raw anatomical (T1w) image by clicking the drop-down menu and finding the appropriate dataset.
    * Select the boxes for 'crop' and 'reorient'
    * For template, choose the 'MNI152_1MM' option from the drop-down menu.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'fsl_anat'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'fsleyes' as your viewer
1. You can also generate a QA image of the results by running the 'Generate images of T1' using the cropped and reoriented anatomical image generated above! Archive the results and save with the tag 'qa' and "fsl_anat".

Once you're happy with the alignment, you can move onto running Freesurfer!

### Generate surfaces using Freesurfer:

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer 7.3.2'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the ACPC aligned anatomical image generated above by clicking the drop-down menu and finding the appropriate datasets.
    * Select the boxes for 'hippocampal' and 'thalamicnuclei'
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
            * The aparc.a2009s parcellation should be overlaid on your inflated surface! Now, repeat the process on the other hemisphere.

Once you're happy with the surfaces, you can move onto preprocessing the diffusion data!

### Preprocess diffusion MRI data with MRTrix3 preproc.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'QSIPrep'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For anat/t1w, select the staged ACPC-aligned anatomical (T1w) image by clicking the drop-down menu and finding the appropriate dataset.
    * For dwi, select the dwi image by clicking the drop-down menu and finding the appropriate dataset.
    * For output_resolution, enter '1'
    * Under the 'Advanced Tab', select the following options:
        * For unringing method, select 'mrdegibbs' by clicking the drop-down menu and finding the appropriate option.
        * Select the check boxes for 'syn_sdc', 'force_syn', and 'xflip'
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
    * In the search bar, type 'RACE-Track | MRTrix3 Anatomical Informed Tractography '
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For dwi, select the preprocessed dMRI image generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For anat, select the ACPC-aligned T1w image generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For min_length, change it from '10' to '20'
    * For max_length, change it from '200' to '250'
    * For imaxs, enter '8'
    * For num_fibers, change it from '15000' to '17500'
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'whole_brain_tractography'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'mrview' as your viewer
        * This will overlay the tractogram on the generated fractional anistropy (FA) image
1. You can also generate a QA image of the whole-brain tractograms by running the 'Generate figures of whole-brain tractogram (tck)' using the whole-brain tractogram (tck) generated above! Archive the results and save with the tag 'qa whole brain tractogram'.

If you're happy with the results, you're ready to move onto segmenting major white matter tracts!

### Segment major white matter tracts.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'White Matter Anatomy Segmentation'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For freesurfer, select the Freesurfer output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For track/tck, select the whole brain tractogram generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'major_white_matter_tracts'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose the 'WMC Tract View' viewer

If you're happy with the results, you're ready to move onto mapping microstructural measures to each tract!

### Remove outlier streamlines.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Remove Tract Outliers'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For freesurfer, select the Freesurfer output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For track/tck, select the whole brain tractogram generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Leave all others as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'major_white_matter_tracts'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose the 'WMC Tract View' viewer

If you're happy with the results, you're ready to move onto mapping microstructural measures to each tract!

### Map microstructural measures along tracts.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Tract Analysis Profiles'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For tensor, select the tensor output generated in Step 4 by clicking the drop-down menu and finding the appropriate dataset.
    * For wmc, select the cleaned major white matter segmentation output generated in Step 6 by clicking the drop-down menu and finding the appropriate dataset.
    * For track/tck, select the whole brain tractogram generated in Step 4 by clicking the drop-down menu and finding the appropriate dataset.
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'DTI_tract_profiles'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose the 'Image Tile' viewer

### Compute macrostructural statistics of tracts.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Tractography Quality Check'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For wmc, select the cleaned major white matter segmentation output generated in Step 6 by clicking the drop-down menu and finding the appropriate dataset.
    * For track/tck, select the whole brain tractogram generated in Step 4 by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'macrostructural_statistics'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose the 'Image Tile' viewer

### Compile macro-structural and micro-structural properties to single csv.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Compile tract macro-structural and profile data'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For tractmeasures (profiles), select the tractmeasures output generated in Step 7 by clicking the drop-down menu and finding the appropriate dataset.
    * For tractmeasures (macro), select the tractmeasures output generated in Step 8 by clicking the drop-down menu and finding the appropriate dataset.
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'macro_micro'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose the 'Image Tile' viewer

**Once you're happy with the results, you're finished with this tutorial! It's time to move onto the next tutorial**
