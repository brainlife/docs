# **ezBIDS Tutorial**

The page provides a simple tutorial for using [ezBIDS](https://brainlife.io/ezbids). The goal of this tutorial is to demonstrate how to use ezBIDS in order to convert raw imaging data to BIDS. For more in-depth information regarding the functionality and feature of ezBIDS, please refer to the [user documentation](https://brainlife.io/docs/using_ezBIDS/).

Before we begin, we'll need some test data, which can be found [here](https://drive.google.com/drive/folders/18Y0SuSAevh2pu7Wuv3BFnbrLZNlVMeJr?usp=sharing). After a few minutes, the data will be downloaded to your Downloads folder, titled "ezBIDS_tutorial_data".

## About the Data

Here is a tree structure of the downloaded data:
```
ezBIDS_tutorial_data/
├── data
│   ├── sub-01
│   │   ├── anat-T1w_20200122142947_2.json
│   │   ├── anat-T1w_20200122142947_2.nii.gz
│   │   ├── fmap-SE-AP_20200122142947_3.json
│   │   ├── fmap-SE-AP_20200122142947_3.nii.gz
│   │   ├── fmap-SE-PA_20200122142947_4.json
│   │   ├── fmap-SE-PA_20200122142947_4.nii.gz
│   │   ├── func_task-bart_20200122142947_13.json
│   │   ├── func_task-bart_20200122142947_13.nii.gz
│   │   ├── func_task-bart_20200122142947_14.json
│   │   ├── func_task-bart_20200122142947_14.nii.gz
│   │   ├── func_task-bart_20200122142947_5.json
│   │   ├── func_task-bart_20200122142947_5.nii.gz
│   │   ├── func_task-bart_20200122142947_6.json
│   │   ├── func_task-bart_20200122142947_6.nii.gz
│   │   ├── func_task-rest_20200122142947_15.json
│   │   ├── func_task-rest_20200122142947_15.nii.gz
│   │   ├── func_task-rest_20200122142947_16.json
│   │   ├── func_task-rest_20200122142947_16.nii.gz
│   │   ├── func_task-rest_20200122142947_7.json
│   │   ├── func_task-rest_20200122142947_7.nii.gz
│   │   ├── func_task-rest_20200122142947_8.json
│   │   ├── func_task-rest_20200122142947_8.nii.gz
│   │   ├── localizer_20200122142947_1_i00001.json
│   │   ├── localizer_20200122142947_1_i00001.nii.gz
│   │   ├── localizer_20200122142947_1_i00002.json
│   │   ├── localizer_20200122142947_1_i00002.nii.gz
│   │   ├── localizer_20200122142947_1_i00003.json
│   │   └── localizer_20200122142947_1_i00003.nii.gz
│   └── sub-02
│       ├── anat-T1w_20200122142947_2.json
│       ├── anat-T1w_20200122142947_2.nii.gz
│       ├── fmap-SE-AP_20200122142947_3.json
│       ├── fmap-SE-AP_20200122142947_3.nii.gz
│       ├── fmap-SE-PA_20200122142947_4.json
│       ├── fmap-SE-PA_20200122142947_4.nii.gz
│       ├── func_task-bart_20200122142947_13.json
│       ├── func_task-bart_20200122142947_13.nii.gz
│       ├── func_task-bart_20200122142947_14.json
│       ├── func_task-bart_20200122142947_14.nii.gz
│       ├── func_task-bart_20200122142947_5.json
│       ├── func_task-bart_20200122142947_5.nii.gz
│       ├── func_task-bart_20200122142947_6.json
│       ├── func_task-bart_20200122142947_6.nii.gz
│       ├── func_task-rest_20200122142947_15.json
│       ├── func_task-rest_20200122142947_15.nii.gz
│       ├── func_task-rest_20200122142947_16.json
│       ├── func_task-rest_20200122142947_16.nii.gz
│       ├── func_task-rest_20200122142947_7.json
│       ├── func_task-rest_20200122142947_7.nii.gz
│       ├── func_task-rest_20200122142947_8.json
│       ├── func_task-rest_20200122142947_8.nii.gz
│       ├── localizer_20200122142947_1_i00001.json
│       ├── localizer_20200122142947_1_i00001.nii.gz
│       ├── localizer_20200122142947_1_i00002.json
│       ├── localizer_20200122142947_1_i00002.nii.gz
│       ├── localizer_20200122142947_1_i00003.json
│       └── localizer_20200122142947_1_i00003.nii.gz
└── logs
    ├── sub-01
    │   ├── task-bart_run-1.csv
    │   └── task-bart_run-2.csv
    └── sub-02
        ├── task-bart_run-1.csv
        └── task-bart_run-2.csv

6 directories, 60 files
```

As you can see, what we have is a standard collection of MRI data and corresponding timing data for two separate subjects. The MRI data, found in the `data` folder, includes localizers, an anatomical T1w, spin-echo field maps, and functional BOLD (and SBRef) sequences. The `logs` folder contains timing data corresponding to the runs of the bart task from each subjects' scans. 

!!! note 
    This sample data contains NIfTI/JSON file pairs, rather than the raw DICOMs themselves. ezBIDS accepts either option (DICOMs, NIfTI/JSON) when uploading data.

## Begin

Now, we are ready to use ezBIDS. All you need is a web browser (Chrome or Firefox preferred) and an internet connection. Copy and paste the ezBIDS URL (https://brainlife.io/ezbids) into your web browser.

### 1. Homepage

Upon reaching the ezBIDS homepage, click "LOG IN/REGISTER" in the upper right part of the web page to sign into your brainlife.io account. If you do not have a brainlife.io account, you will be guided through a quick setup. This process is simple and meant for authentication purposes.

!!! note 
    If you are already logged into your brainlife.io account, you will instead see and click "GET STARTED".

### 2. Upload Imaging Data

Click the "Choose Files" button and select the `data` folder to upload the imaging data. We will upload the `logs` folder later.

!!! note
    The tutorial should be in your Downloads folder, titled "ezBIDS_tutorial_data".

This upload process may take a couple minutes, depending on the strength of your internet connection. Once uploaded, you will see some text generated on the screen, indicating that ezBIDS is performing several automated, behind-the-scences processes to extract as much relevant BIDS information about your dataset as possible. This is meant to save you time later on. Once complete, you will see text reading "Analysis complete!", along with a list of your uploaded imaging data. From here, ezBIDS will guide you through several steps for you to verify and make any corrections or modifcations to ensure that the data is specified to your liking. Importantly, ezBIDS will alert you to any details that need to be addressed in order to ensure BIDS compliance. 

### 3. Dataset Description

This page allows you to specify information about the entire dataset. Notice how ezBIDS will autofill several fields for you. Required fields have been marked with a red asterisk, indicating that this information is absolutely needed in order to have a BIDS-compliant dataset. However, the information that ezBIDS provides may not always be appropriate, necessitating changes if you so choose. For example, the **Dataset Name** is specified as *Untitiled*, which isn't very informative. Let's change that field instead to *myData*. Additionally, the **Authors** field is currently blank, since ezBIDS couldn't determine that information and it's not a required field for BIDS compliance. Let's click the **Authors field and type our name (e.g. *John Doe*) and press the Return (or Enter) button on your keyboard. You are more than welcome to provide additional information on this page, but for this tutorial, we will move on by clicking the "Next" button at the bottom right side of the web page.

### 4. Subjects and Sessions

This page allows you to verify or modify the **subject** (and **session**) IDs for your data. ezBIDS provided the correct subject IDs (`01`, `02`) but you may modify them if you like. For the puposes of this tutorial though, let's leave them as they are. Note that the **session** fields for each subject are left blank. A session ID is meant to denote that an individual subject was scanned multiple times. This field should thus remain empty unless the individual subject was scanned more than once during separate visits. In the case of our data, each subject was scanned only once and thus no session ID should be specified. Since ezBIDS specified the correct subject IDs, we can click the "Next" button to proceed.

### 5. Series Mapping

This page allows you to verify and modify the identify of the imaging data, as well as provide additional information that will better describe the dataset. 

You will notice on the left side of the page that the data has been labeled and grouped into a list of "like-minded" groups, specified by a group ID value (e.g. `#1`). The purpose of this grouping is to organize data that are the same but collected across separate subjects (or sessions). For example, each subject has an antomical T1w sequence collected, containing the same scan parameters, so ezBIDS groups these two sequences together for now. The number of sequences in each group is also provided (e.g. `2 objs`). When you make a modification to a group, it will be applied to all files in the group. thereby saving you time having to modify each sequence individually, though that is an option later on.

!!! note
    It is best practice to make as many necessary edits on this page as possible. On upcoming pages, you will be able to see the changes for each individual file and make more surgical changes then.

Note that the data have been identified with a specific datatype and suffix pairing (e.g. the anatomical T1w data group is specified as `anat/T1w`). This is how BIDS standardizes the identity of data. For example, the this is anatomical (`anat`) data, but more specifically, it is a `T1w` sequence. Any additional labelling information for the data (known as "entities" in BIDS parlance) are also provided, if ezBIDS detects it. For example, the functional BOLD and SBRef data (`func/bold`) contain a `task` entity label (`bart`, `rest`).

If you need (or wish) to make changes to the identity and label(s) of the groups, this is done on the right side of the page. For example, if ezBIDS incorrectly guesses a datatype and suffix pairing for a group, you can select the proper identity in the **Datatype/Suffix** drop-down menu. If you do not know the identify of a group, or don't wish to convert that group's data anyway (e.g. the *localizer*) you can select *Exclude from BIDS conversion* at the top of the drop-down menu. This will excluded the specified data from being converted into a BIDS dataset.

 You can also specify entity labels for each group if desired, which we will now do. Click on the `anat/T1w` group on the left side of the page, and then add the term *mprage* to the **acquisition-** label over on the right. What we are doing is specifying that the anatomical T1w (`anat/T1w`) sequences in this group can be more thoroughly described as [Magnetization Prepared - RApid Gradient Echo (MPRAGE)](https://mriquestions.com/mp-rage-v-mr2rage.html) acquisitions, and applying the `acquisition-mprage` entity label to both sequences in the group. This has the important benefit of saving you time compared to typing *mprage* for each individual sequence in the group (image if there were 100!). 

 Lastly, you probably noticed the gold color oval next to the spin-echo field map groups (`fmap/epi`), signifiying a warning that you should examine. When you click on one of these groups, a warning message in gold will alert you that you should specify the **IntendedFor** information for this group. Doing so specifies which data these field maps will apply susceptibility distortion correction (SDC) to. To resolve this warning, go to the **IntendedFor** drop-down menu and select the `func/bold` groups (`#5`, `#7`). Doing so for each `fmap/epi` group will cause the warnings to disappear. 

 !!! note
    Warnings *should* be addressed, but you can ignore them if feel that they won't impact the quality of your data. Erorrs however, which appear as red ovals, indicate that there is incorrect or missing information that *must* be addressed, otherwise you will not have a BIDS-compliant dataset. The presence of errors means that you will not be able to proceed until resolved. ezBIDS will guide you on how to resolve any such errors.

Once all warnings (and errors) are resolved, you can proceed to the next page.

### 6. Events

We will now upload the timing files in the `logs` directory of our tutorial data. Simply click the "Select File" button and specify the `logs` directory (you will upload all contents in there) in your "ezBIDS_tutorial_data" folder. You will then see a list of header fields on the left (`Onset`, `Duration`, etc) with a corresponding drop-down menu. This is ezBIDS's way of asking you to specify which columns in the timing files correspond to the header fields on the left. For `Onset`, choose *tstart*, and for `Duration` choose *dur*. These two headers are the only required fields to proceed, the rest are optional and can be left empty for this tutorial. 

### 7. Dataset Review

On this page, you will see all your data labeled and organized by subject ID and in order of when they were collected during the scan. Importantly, the grouping has been removed so every sequence can be examined individually. Note that the `acquisition-mprage` addition we made back on the **Series Mapping** page has been applied to each `anat/T1w` sequence. You will also notice that the timing files have been identified as `func/events` and contain the same entity labels as their corresponding `func/bold` sequences (i.e. `task-bart`). Additionally, the `func/events` are placed directly underneath their corresponding `func/bold`, to better visualize the correspondence. As always, you are able to modify things as you see fit, but for this tutorial everything appears to be in good order (i.e. no errors or warnings) and so we will proceed.

### 8. Deface

This page allows you to deface (i.e. remove) the facial features from every anatomical (`anat`) sequence in the uploaded data, by selecting an option in the **Defacing Method** drop-down menu. The recommended option is to use the `quickshear` package because it is relatively quick and effective; however, you may elect for the `pydeface` package instead. Once a defacing procedure is selected, a screenshot of the original (un-defaced) data is shown on the left. Once defacing 9is complete, a screenshot of the defaced image is provided on the right, allowing you to see the quality of the resulting defacing. If the defacing is bad (i.e. part of the brain has been cut) you can either select the other defacing option or simply forgo defacing for that specific anatomical sequence. If you choose the latter in this situation, select the "Use Original" button above the original screenshot. 

!!! note
    Defacing is an optional procedure, so you can skip this step if you so choose. However, it is hightly recommended that you perform defacing to remove identifying information. This form of anonymization is particularly important if you plan on sharing data with colleagues or the broader imaging community.  

### 9. Participants Info

This page allows you to specify phenotype data pertaining to each subject. By default, ezBIDS will attempt to extract and provide the `sex` and `age` information, and you may include additional phenotype information if you wish (e.g. `handedness` `experiment_group`, etc) or even remove fields. In this example, ezBIDS was unable to determine the `sex` and `age` of the two subjects because this information could not be found in the data. It is generally good practice to provide some level of phenotypical information, thus, for `sub-01`, set the `sex` and `age` as *M* and *30* respectively. For `sub-02` do *F* and *26*.

### 10. Access BIDS data

Upon reaching this page, you've completed all the ezBIDS steps. What you'll now do is click on the green **Finalize** button so that ezBIDS can apply all the edits and modifications you made earlier. The result should be a dataset that is properly organized and labeled according to BIDS. If you do not wish for the previously excluded data to be included in the BIDS dataset (by default it gets placed in an `excluded` folder) you can uncheck the blue checkmark above the **Finalize** button.

Once finalized, you will see two boxes underneath. On the left is the **BIDS Structure**, which provides a tree structure of your BIDS data (notice the differences in this output compared to what we started with at the beginning of this tutorial). On the right is the **bids-validator output**, which runs the BIDS-validator as a final assessment to see whether your data is BIDS-compliant (i.e. passess validation). 

If any red text is present in the **bids-validator output**, this means that your data is **NOT** BIDS-compliant and requires some additional work. In such cases you might need to go back a few pages to correct the error(s). Once you return back to this page, click the **Rerun Finalize Step** to apply your new changes. 

If any gold text is present, this means that there is a warning and *should* be corrected, but you do not necessarily have to. You still have a BIDS-compliant dataset!

Once you have a BIDS-compliant dataset, you'll need to decide what to do with your data. If you'd like to download it to your local machine or server, select the blue **Download BIDS** button. If you'd like to upload your data to brainlife.io into a new or current project, click the **Send to Brainlife.io** button and choose the *Send to brainlife* option. A popup window you appear, asking whether you want this data in a new project or a project that you've already started. The latter case would occur if you're adding additional data to an already existing dataset that you have on brainlife.io. Alternatively, in lieu of uploading your data to brainlife.io, you can upload your data to OpenNeuro.org by clicking on the **Send to OpenNeuro** button. For the puposes of this tutorial, simply download the BIDS data back to your computer or server.

The last item of business is to click on the **Download configuration/template** button. Doing so will download an `ezBIDS_template.json` file containing information pertaining to your current ezBIDS session, mainly all the edits and modifications that were made. The next time you use ezBIDS, assuming you're converting data from the same study, you can include this json file with your MRI data (doesn't matter where you store it, so long as it gets included in the upload). This will then apply the changes you made before to your new data. For example, in this tutorial session we included the `acquisition-mprage` entity label for our `anat/T1w` sequences. The next time you use ezBIDS and provide the `ezBIDS_template.json`, ezBIDS will recognize this and automatically apply this information, thus saving you time without the need to build your own configuration file. 

!!! note 
    Providing an `ezBIDS_template.json` will not apply changes to the **Dataset Review** page, as each scan can be different, such as one participant needing to use the restroom during a scan, re-running a sequences due to excessive movement, running a scan protocol out of order, etc. This should not create undue work, it is simply a safer way of accounting for the idiosyncrasies that can occur with each scan. 

## Feedback

Congratulations, you've successully used ezBIDS to create a BIDS-compliant dataset! For questions and feedback you can reach us the following ways:
1. Brainlife.io slack (general channel)
2. Email (djlevitas208@gmail.com)
3. Github issues (https://github.com/brainlife/ezbids/issues)
