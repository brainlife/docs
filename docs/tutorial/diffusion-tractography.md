!!! warning
    This is a draft

## Diffusion-weighted MRI preprocessing.

This page demonstrates common steps used to perform diffusion tractography analyses on brainlife.io. The goal of this tutorial is to show you how to process anatomical and diffusion data to generate **tractograms**, **segment major white matter tracts**, and **map microstructural measures** to the major white matter tracks. This tutorial will use a variety of brainlife.io applications to [generate tractograms](https://brainlife.io/app/5aac2437f0b5260027e24ae1), [segment white matter tracts](https://brainlife.io/app/5cc73ef44ed9df00317f6288), and [map microstructural measures](https://brainlife.io/app/5cc210ce4ed9df00317f61cf) to these tracts.

This tutorial will use a combination of skills developed in the [Introduction tutorial](https://brainlife.io/docs/tutorial/introduction-to-brainlife/), the [Anatomical Preprocessing tutorial](https://brainlife.io/docs/tutorial/t1w-preprocessing/), and the [Diffusion MRI Preprocessing tutorial](https://brainlife.io/docs/tutorial/diffusion-preprocessing/) you recently completed. If you haven't read our introduction to brainlife, or if you're not comfortable staging, processing, archiving, and viewing data on brainlife.io, please go back through that tutorial before beginning this one.

### 1. Anatomical preprocessing.

The first step of diffusion tractography often involves processing the anatomical images. In order to track in a biologically-informed manner and to extract major white matter tracts, we must have both of our anatomical (T1w or T2w) images and our diffusion MRI images **aligned**. One way we can make this easier for [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) is by aligning the anatomical images in such a way that the center of the brain is centered in the image. We refer to this as **ACPC-aligned**, as we are aligning the data to the **anterior commissure-posterior comissure plane**. This is the first step in dMRI preprocessing, and it is typically done with the [HCP ACPC Alignment (T1w)](https://brainlife.io/app/5c61c69f14027a01b14adcb3) app. We will then need to generate cortical and white matter surfaces and brain region parcellations using [Freesurfer](https://brainlife.io/app/58c56d92e13a50849b258801). These will be used for segmenting the major white matter tracts following tractography. Once we've processed our anatomical image, we can move on to diffusion MRI preprocessing.

### 2. Diffusion preprocessing 

The next step in diffusion preprocessing is to actually preprocess the diffusion data. dMRI data, like any other MRI dataset, is quite noisy. Just like fMRI, dMRI data is sensitive to **susceptibility distortions**. This is when regions of the brain look "wavy" or fold inward or outward on itself in images. This is due to how the scanner sweeps across the entire brain during acquisition. Also, dMRI data is very susceptible to motion. Because some scans take a long time, it is nearly impossible for a participant to lie completely motionless. Even the most subtle movements can alter the results in a specific region and make your data less accurate. Finally, movement of the magnetic gradients can induce electromagnetic currents which can affect the measurement in any given location. These electromagnetic currents are called **eddy currents**, and they are due to the fact that electricity and magnetism are part of the same field, otherwise known as the **electromagnetic field**. Thankfully, brainlife.io allows us to correct for all of these common artifacts using [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9).

In addition to these artifacts, dMRI has a very low **signal-to-noise ratio (SNR)**, which directly impacts how well you can fit microstructural models and analyze diffusion data. This means that in any given location, the amount of diffusion signal is very low compared to the "noise" of the scanner. There are methods to lower the amount of noise across the brain and thus increase the SNR. [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) has the capability to identify and remove noise using principal components analysis (PCA) and Rician analysis.

On top of these issues, dMRI data is sensitive to other artifacts and non-regularities, including **gibbs ringing and bias field inhomogeneities**, which can affect the quality of the data and how well measures can be derived from the data. [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) can deal with all of these issues for us!

Finally, once the dMRI data is preprocessed and cleaned, [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) will align the dMRI data to the anatomical data. This will ensure that any analyses we do with the dMRI data will be anatomically-informed and biologically-relevant.

Useful information about the preprocessing pipeline that [MrTrix3 Preprocessing](https://brainlife.io/app/5a813e52dc4031003b8b36f9) is designed to run -- Diffusion Parameter EStimation with Gibbs and NoisE Removal (DESIGNER) -- can be found in this [original Neuroimage paper](https://www.ncbi.nlm.nih.gov/pubmed/30077743). **(<---- fixed link, double-check this is the correct pub)**

### 3. Diffusion modeling & anatomically-informed ensemble tractography.

Once the dMRI data has been cleaned and aligned to the anatomical (T1w) image, the next step is to map the underlying white matter anatomy using diffusion tractography. Diffusion tractography works by modeling how water is moving throughout the brain based on the principle of **anisotropy** -- that water will move unequally in a particular direction given a physical constraint. In the case of diffusion MRI, this constraint comes from the myelin wrapping the many axons arranged into bundles known as **fasicles**. _( ---> question here, which are known based on their termination? There are common groupings of these fasicles, known as **white matter tracts**, based on their terminations into the cortex.)_ Diffusion tractography generates **streamlines** that act as evidence of underlying white matter organization. We can bundle these streamlines into common white matter tracts digitally, just as we can bundle fasciles into **tracts** using histological methods.

The first step is to fit models of diffusion at each location in the dMRI image that can be used as a guide for the tractography algorithms. The most popular model for this is known as **constrained spherical deconvolution**, or **CSD**. CSD allows for the possibility of multiple fasicles entering a single location that may have different trajectories, which is a major advantage of this model over other models of diffusion. The CSD model at each location can tell the tracking algorithm _how strongly_ and in _what direction_ water is diffusing in the brain, and thus where organized and myelinated white matter is likely to be.

Once we know the direction of water movement using the CSD model, we can then perform **diffusion tractography** to map the underlying organized white matter. Diffusion tractography *sews* together regions with common directions of water movement into streamlines that provide evidence for organized, myelinated white matter. These operations can be done millions of times across the entire brain to form **tractograms**. There are many algorithms for doing this that fall into two main categories: **deterministic** and **probabilistic**. **Determinstic** algorithms work by strictly following the directions of water movement, while **probabilistic** algorithms infer a probability of different directions of water movement at any given location. Deterministic tractography tends to be an overly conservative representation of the white matter, as modeling is never perfectly accurate. Probabilistic tractography tends to provide *hairy* streamline representations, as the model allows for a probability of different directions of water movement at any given location. Recently, it was discovered that combining both algorithms, or **ensembling** them, provides the most anatomically-accurate tractograms.

On brainlife.io, we have combined **ensemble tractography** with **anatomically-constrained tractography (ACT)**, which restricts streamline representations to those that are biologically-plausible, in a single [Ensemble ACT Tractography](https://brainlife.io/app/5aac2437f0b5260027e24ae1) app! Within this app, the CSD model will be fit and tractography will be performed to generate **whole-brain tractograms**. That means a model of white matter microstructure -- diffusion tensor (DTI) -- will be fit and maps of fractional anisotropy (FA), mean diffusivity (MD), radial diffusivity (RD), and axial diffusivity (AD) will be generated.

More information on ensemble tractography can be found in this [PLOS Computational Biology article](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004692) paper. More information on **ACT** can be found in this [Neuroimage publication](https://www.ncbi.nlm.nih.gov/pubmed/22705374).  _(<--- fixed link, make sure it's correct)_

### 4. Major white matter tract segmentation.

Once our anatomical brain parcellations and whole-brain tractograms are generated, the next step is to segment the tractogram into known **major white matter tracts**. To do this, we need both information about the terminations of each streamline into the cortex and histologically-derived definitions for each major white matter tract. The histological definitions of major white matter tracts allow us to group the streamlines in our tractograms into major white matter tracts. On brainlife.io, we have developed an automated [white matter segmentation](https://brainlife.io/app/5cc73ef44ed9df00317f6288) app that will segment our whole-brain tractograms into over 70 known major white matter tracts.

More information on the segmentation algorithm can be found in this [Brain Structure and Function paper](https://www.ncbi.nlm.nih.gov/pubmed/31342157).   _(<--- fixed link, make sure it's correct)_

### 5. Microstructural mapping to major white matter tracts.

Once we have segmented our tractograms into major white matter tracts and fit the DTI model to our dMRI data, we can now map the microstructural information to our major white matter tracts. This is done by computing the average microstructural measure at multiple locations along each major white matter tract. This process generates a plot known as a **tract profile**, which provides microstructural information at different locations along a tract. These profiles can be used to examine microstructural properties along different tracts that might help distinguish different groups, such as those with a neurodegenerative disease versus those without the disease. We have developed an automated [microstructural mapping](https://brainlife.io/app/5cc210ce4ed9df00317f61cf) app on brainlife.io that will map multiple microstructural measures.

More information on **tract profiles** can be found in this [PLOS One article](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0049790).

Now, let's get to work! The following steps of this tutorial will show you how to:

1. ACPC align the anatomical (T1w) image,
1. Generate Freesurfer Brain parcellation,
1. preprocess the dMRI data using MrTrix3 Preprocessing,
1. fit the CSD & DTI models, perform tractography using MrTrix3 ACT,
1. segment major white matter tracts,
1. and map microstructural (DTI) measures along each tract.

### Copy appropriate data over from a single subject in the Bl class test data project

1. Click the following link to go to the project's page for the 'Bl class test data' project: [https://brainlife.io/project/5e6ea1a48a2089fc6d8e9fc7](https://brainlife.io/project/5e6ea1a48a2089fc6d8e9fc7)
1. Click the 'Archive' tab at the top of the screen to go to the Archives page.
1. Select the following datatypes from one subject by clicking the boxes next to the data:
    * dwi
        * Both dwi files
    * anat/t1w
1. Click the 'Stage to process' button on the right side of the screen
    * For 'Project,' select your project from the drop-down menu.
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

Once you're happy with the alignment, you can move onto running mrtrix3 preproc!

### Generate surfaces using Freesurfer:

1. On the 'Process' tab of your project, click 'Submit App' to submit a new application.
    * In the search bar, type 'Freesurfer.'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For input, select the ACPC aligned anatomical (T1w) image generated above by clicking the drop-down menu and finding the appropriate dataset.
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
            
Once you're happy with the surfaces, you can move onto preprocessing your dMRI data!

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

Once you're happy with the results, you can move onto fitting the CSD, DTI, and performing tractography!

### Fit the CSD & DTI models, perform diffusion tractography.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'mrtrix3 Anatomically Constrained Tractography (ACT)'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For dwi, select the preprocessed dMRI image generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For tensor_fit, type '1000' to fit the model on the b=1000 shell
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'whole_brain_tractography'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose 'mrview' as your viewer
        * This will overlay the tractogram on the generated fractional anistropy (FA) image

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

### Map microstructural measures along tracts.

1. On the 'Process' tab, click 'Submit App' to submit a new application.
    * In the search bar, type 'Tract Analysis Profiles'
    * Click the app card.
1. On the 'Submit App' page, select the following:
    * For tensor, select the tensor output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For wmc, select the major white matter semgentation output generated above by clicking the drop-down menu and finding the appropriate dataset.
    * For track/tck, select the whole brain tractogram generated above by clicking the drop-down menu and finding the appropriate dataset.
    * Leave all other options as defaults
    * Select the box for 'Archive all output datasets when finished'
        * For 'Dataset Tags,' type and enter 'DTI_tract_profiles'
    * Hit 'Submit'
1. Once the app is finished running, view the results by clicking the 'eye' icon to the right of the dataset
    * Choose the 'Image Tile' viewer
    
**Once you're happy with the results, you're finished with this tutorial! It's time to move onto the next tutorial: structural network generation!**


