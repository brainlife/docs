# Clairvoy Tutorial

## Welcome to Clairvoy.  

Clairvoy is an experimental [brainlife.io](https://brainlife.io) App.  The Clairvoy App utilizes a machine learning, prognostic model to predict whether someone who has experienced a concussion is at risk for slow recovery (defined as not cleared for full return to sport at greater than 28 days). The diffusion-weighted and T1-weighted MRI should be collected 24-48 hours after a diagnosed concussion. This experimental tool is entirely based on the publication of Bertó et al. 2024 (Neuroimage: Clinical, In Press). Clairvoy was funded by the U.S. Department of Defense, award W81XWH-20-1-0717, PI: Nicholas L Port. The dataset analyzed by Bertó et al. is from the NCAA/DOD CARE Consortium and was jointly funded by the NCAA and DOD, award W81XWH-14-2-0151, PI: Thomas W McAllister.  

We encourage clinicians and scientists to try Clairvoy and appreciate any feedback on how to refine the app. 
Drs. [Port](mailto:nport@iu.edu) and [Pestilli](mailto:pestilli@utexas.edu) are actively seeking funding to conduct a clinical trial to validate Clairvoy, but, at present, Clairvoy has not been validated for clinical use.  Thank you for trying Clairvoy and helping us improve it.

## Directions

The Clairvoy App lives inside the [brainlife.io](https://brainlife.io) platform, an easy-to-use, web-based, Open Science data analysis platform focused on neuroimaging.  Free and open to anyone in the world, Brainlife has over 1,000 users across dozens of countries. Brainlife offers free access to the super-computer built into the platform. If you become a regular user, you can connect your (super-)computer to BrainLife, which may significantly speed up your analyses. Before using Cairvoy, you will need a free Brainlife login. We recommend reading the [brainlife.io getting started guide](https://brainlife.io/docs/user/started/). There are also many helpful videos on the [brainlife.io YouTube channel](https://www.youtube.com/@brainlifeio/videos).

Clairvoy uses the web-based ezBIDS tool (built into Brainlife) to import Dicom files, convert them automatically to NIfTI format, and then create a BIDS project (Brain Imaging Data Structure). You can learn more about ezBIDs at [this link](https://brainlife.io/docs/using_ezBIDS/) and watch a video at [this link](https://www.youtube.com/watch?v=KvhIHxzHsl4). The dMRI and T1 protocols can be found in Berto, et al. (Neuroimage: Clinical, In Press) 2024 and Nencka, et al. 2018 (Brain Imaging Behav 12:1121-1140). 

### Step-by-step directions for Clairvoy:

A narrated video of how to use Clairvoy is available at [this link](https://youtu.be/TvVmsXITP_0).

### Step 1.  Upload files into a BIDS project
1. Login into ezBIDS with your free Brainlife account.[^1] The homepage will now open to the `Upload Imaging Data` on the left side Tab. 
2. Drag and drop, or browse, for the files you wish to upload. Zip’ing the files may increase upload speed. Fill in the relevant details of the `Dataset Description`: `Dataset Name` (e.g., “DWI_workflow”, Authors (eg. your name).  Some elements will be extracted from the DICOM, e.g., `Subject` and `Session`. If the data is not deidentified, you may wish to change the subject name to a private subject ID number. In `BIDS Datatype`, choose `IntendedFor` to `dwi/dwi`. 
5. It is best practice to deface the subjects. To do so, choose `QuickShear` method and run defacing (this will remove any recognizable face and ear features). When finished, visually confirm the brain is still in the image and select `Use Defaced`. In `Participants Info`, you may want to choose the sex and age of the subject (this is not HIPAA-protected information).
6. Select `Finalize` to convert to BIDS.[^2]
7. Click `Send to Brainlife.io` and select `Run DWI Pipeline`.
8. Click `Submit`, this will automatically create a private `Project` on [brainlife.io](https://brainlife.io), and the `DWI Pipeline` will process the data inside the project.  

### Step 2.  Switch to Brainlife
1. Navigate to [brainlife.io/projects](https://brainlife.io/projects). 
2. In `Projects`, search for a `Project` named `Dataset Name` (or use the name selected in Step 1, e.g., DWI_workflow).  
3. The `Pipeline` Tab should show the pipeline is “active” and “processing.” Wait for process completion[^3].
  
### Step 3.  Run the Clairvoy App
After pipeline completion:
1. Choose the `Archive` Tab in your project,
2. Select `tractmeasures`, and
3. Choose `Stage to process`.  Give the process the name “Clairvoy” and click `Submit`.
4. In the `Preprocess` Tab, choose `Clairvoy` and click `Submit App`.
5. Choose the `Clairvoy App` from the list of Apps suggested by [brainlife.io](https://brainlife.io) and click submit. This should only take a minute or so to complete.
6. When finished, the `Clairvoy App` output will state either: **This injury is at high risk of developing PPCS** in red letters, or **This injury is predicted to recover in under 28 days** in green letters.

**[Here is a narrated YouTube video about how to run Clairvoy](https://youtu.be/TvVmsXITP_0)**

[^1]: For security reasons, it will require that you create a user account on [brainlife.io](https://brainlife.io).

[^2]: It is best practice to save the download `configuration/template` to your computer, which can be reused later for other similar datasets. 

[^3]: Time to completion depends on the currently available computing resources on [brainlife.io](https://brainlife.io). 
If you have access to a (super-)computer, time to completion may be reduced to less than an hour. 
If you use a free, public [brainlife.io](https://brainlife.io) supercomputer, the pipeline may take over 24 hours to complete.

