
# **ezBIDS User Documentation**

[ezBIDS](https://brainlife.io/ezbids) is a web-based BIDS conversion tool that requires neither installation, programming proficiency, nor knowledge of the Unix terminal and BIDS specification. The term BIDS is an acronym that stands for the [Brain Imaging Data Structure](https://bids-specification.readthedocs.io/en/stable/). More specifically, BIDS is a format for describing and organizing (primarily neuroimaging) data, greatly enhancing data sharing within the scientific community, as well as enabling access to a suite of processing and analysis tools that can be automatically executed on BIDS data, known as [BIDS apps](https://bids-apps.neuroimaging.io/). This tutorial describes how to use ezBIDS to easily convert imaging data to BIDS.

## A Quick Primer

The process of converting raw imaging data to BIDS is often time-consuming. Fortunately, there exist many open source tools to aid researchers in this process. For a thorough list of BIDS conversion tools, see [here](https://github.com/rordenlab/dcm2niix#links) and [here](https://bids.neuroimaging.io/benefits.html#mri-and-pet-converters). ezBIDS contains four unique features that differentiate it from these other currently available tools:
1. No installation or programming required
2. Semi-automated inference and guidance for BIDS adherence
3. Conversion of task event/timing files associated with functional BOLD (`func/bold`) sequences
4. Multiple data management options:
    1. Download converted BIDS data to local computer or server
    2. Upload converted BIDS data to open-science platforms such as [OpenNeuro](https://openneuro.org/) and [brainlife.io](https://brainlife.io/about/)

ezBIDS attempts to do the heavy lifting for users, discerning the necessary descriptions of uploaded data as required by BIDS, and how the data is to be organized. Importantly however, users have the ability to make edits/modifications on the ezBIDS web pages to ensure that the provided information is appropriate. Upon reaching the ezBIDS homepage, users are guided through several steps (i.e. web pages) to ensure that data is accurately described, organized, and ultimately, converted. These steps are described below.

## ezBIDS Walkthrough

### 1. Upload Imaging Data

Upon reaching ezBIDS (https://brainlife.io/ezbids), users will see the following:

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS0_homepage.png"/>
</td></tr></table>
<br>
<!-- ![Home Page](./img/ezbids/Levitas_etal_figureS0_homepage.png) -->

For ezBIDS to access users' imaging data, the data must first be uploaded to ezBIDS's secure and encrypted JetStream2 server (ezBIDS itself runs on a secure VM running on Jetstream cloud; HIPAA aligned cloud computing infrastructure). There are two accepted upload methods:
1. **Raw DICOM data** from the scanner or imaging device.
2. **NIfTI/JSON data** converted from DICOMs using [dcm2niix](https://github.com/rordenlab/dcm2niix). If using this option, it is best if the [most recent](https://github.com/rordenlab/dcm2niix/releases) dcm2niix version was used. 

!!! warning
    Uploaded data can only be accessed through the unique URL with your session ID. Furthermore, uploaded data is purged from the ezBIDS system after 5 days.

ezBIDS prefers that non-anonymized data is uploaded (e.g. with the `-ba y` flag option in dcm2niix), as this makes it easier to discern the subject (and session, if applicable) mapping of the data and doesn’t require organizing the raw data in any specific manner. When non-anonymized, the data contains important metadata information such as the *AcquisitionDateTime*, *PatientName*, and *PatientID*, which help inform ezBIDS of the subject and session mappings. If however anonymized data are uploaded, which do not contain this metadata information, users will need to organize their data such that data from individual subject (and session) scans are in separate folders. To improve performance, these folder names should explicitly specify the subject (and session, if applicable) IDs desired (e.g., `sub-001`, `sub-001_ses-pre`, etc). Users are not required to adhere to this naming convention, however, in such cases ezBIDS will simply use the folder(s) name(s) as a placeholder for the subject (and session) IDs.

!!! note Non-anonymized Data
    If non-anonymized data is provided, ezBIDS will eventually anonymize the data (i.e., remove identifying metadata) before converting to BIDS.

Users may upload compressed imaging data using formats such as *.tar.xz*, *.tar*, *.tgz*, *.gz*, *.7z*, *.bz2*, *.zip*, and *.rar*.

Once data is uploaded, ezBIDS perform several backend operations to gather and identify as much relevant BIDS information as possible, as well as general information to help users see what data they’re examining (e.g., image screenshots, metadata, etc). All this information is then presented to users on the subsequent pages. Users may then edit or modify BIDS-specific information as they so choose. 

!!! info Upload Speed and Time
    The amount of time to upload data is dependent on the size of data being uploaded and internet connection strength. 
    
ezBIDS has been primarily tested on the Chrome and Firefox browsers; it is preferable that one of these browsers be used. 

### 2. Dataset Description

This page allows users to provide general information regarding their dataset, such as authors, funding sources, etc. 

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS2_dataset_description.png"/>
</td></tr></table>
<br>
<!-- ![Dataset Description](./img/ezbids/Levitas_etal_figureS2_dataset_description.png) -->

Users only need to provide information here the first time using ezBIDS with data from a new dataset. When using ezBIDS with newly acquired data from the same dataset as before, this information is already provided and is therefore redundant to specify again.

!!! warning Progress checks
    For this and subsequent ezBIDS web pages, certain information is required by BIDS, indicated on each page with a red asterisk or circle. These fields must be entered, otherwise users will be unable to progress to the next page. This provides users with real-time assistance on what information is required in order to have BIDS compliant data, rather than going through the entire process only to learn afterwards that there is an issue to resolve. 

### 3. Subjects/Sessions

On this page, ezBIDS provides the subject (and session, if applicable) IDs of the uploaded data. Users may edit or modify these fields as they see fit. This can either be done manually, or by selecting an option within the "Reset Subject Mapping" button.

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS3_subjects_sessions.png"/>
</td></tr></table>
<!-- ![Subjects/Sessions](./img/ezbids/Levitas_etal_figureS3_subjects_sessions.png) -->

### 4. Series Mapping

Here, ezBIDS organizes all uploaded data into specific series/group IDs. Imaging sequences are grouped into the same series ID if they contain identical information in the following metadata fields: *SeriesDescription*, *ImageType*, *EchoTime*, and *RepetitionTime*. ezBIDS assumes that data with the same series ID will contain the same [entities](https://bids-specification.readthedocs.io/en/stable/appendices/entities.html) that go into the converted file names.

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS4_series_mapping_all.png"/>
</td></tr></table>
<br>
<!-- ![Series Mapping](./img/ezbids/Levitas_etal_figureS4_series_mapping.png) -->

 This grouping procedure is meant to save users time rather than having to make edits/modifications for each specific uploaded data file. For example, suppose a user uploads data from 10 subjects, each containing an anatomical T1w sequence. Assuming each was collected with identical scan parameters, all 10 anatomical T1w sequences would be given the same series ID. Then, if a user wishes to specify that these images are MPRAGE acquisitions, they can add this to the acquisition (acq) entity provided on the page (i.e. `acq-mprage`). This edit is then applied to all images in the group, since ezBIDS assumes that these images will have the same entity labels (with the possible exceptions of subject and session). Once the data is converted to BIDS, these entity labels will appear in the corresponding file names. 

When examining these series IDs (left side of page), users will notice that ezBIDS has attempted to identify the datatype/suffix pairing of each series group. A message (in yellow) towards the top of the page will explain how ezBIDS identified the datatype/suffix pairing of the data in the group. If this is incorrect and needs fixing, users can click on it and then select the correct datatype/suffix pair under the drop-down "Datatype/Suffix" menu on the right side of the screen. If ezBIDS cannot identify the data from the group, it will set its datatype/suffix pairing to `exclude`, indicating that it will not be converted to BIDS. Users may adjust this if necessary, or set this “exclude” tag on data that they do not wish to convert anyway (e.g., localizer sequences). 
	
!!! note Viewing all series ID sequences
    Once a series ID is clicked, users can scroll down on the right side of the page to see all sequences that are associated with that specific series ID. ezBIDS specifies the numbers of sequences associated with a specific series ID with the `objs` label. For example, `2 objs` specifies that there are two sequences in the uploaded data with the specific series ID. 
    
In addition to identifying the datatype/suffix pairing, ezBIDS also attempts to discern any required entity label(s) for specific datatype/suffix pairings; these too can be edited/modified. If a specific entity label is required but not specified, ezBIDS will alert users to this error (red circle and asterisk) and require addressing before proceeding to the next page. 

On subsequent pages, users will be able to view all imaging sequences and edit individual ones if necessary, however, the bulk of edits/modifications should occur on this page. 

!!! note The run entity label
    ezBIDS will later specify the run entity label if needed, and therefore does not need to be specified on this page. 

If users collected and uploaded fieldmap (fmap) data, ezBIDS will provide a warning (yellow circle) saying that it is recommended that users specify which data the fieldmaps are meant to be applied to, known as the "IntendedFor" field. On the right side of the screen, users can click this IntendedFor drop-down menu and select the series IDs that the fieldmaps are meant for. Alternatively, users may specify a B0FieldIdentifier & B0FieldSource mapping, but this should only be done if users know what they’re doing, as it is an advanced BIDS feature. 

### 5. Events

If users collected functional BOLD (`func/bold`) data and have the corresponding timing files, these timing files can be uploaded here, otherwise this page can be skipped. ezBIDS accepts several timing file formats, including `.csv`, `.tsv`, `.txt`, `.out`, and `.xlsx`. Additionally, ezBIDS can parse E-Prime timing files, though this is a beta-feature.

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS5_events.png"/>
</td></tr></table>
<br>
<!-- ![Events](./img/ezbids/Levitas_etal_figureS5_events.png) -->

!!! warning 
    When uploading timing files, it is **crucial** that the following entity labels be explicitly specified: *subject*, (*session*, if applicable), *task*, and *run*. These must match the corresponding `func/bold` sequences. This information can either be provided in the file path or as columns in the timing data themselves. Failure to do so will result in ezBIDS creating a placeholder value for any mismatched entity labels that will later require manual intervention.

Once uploaded, ezBIDS parses the timing files for specific columns and their row values. BIDS requires that timing files be translated into *events.tsv* format, with several required and optional columns. Required columns include the *onset* and *duration*. ezBIDS has the ability to assist users in cases where uploaded timing files do not have a column that specifically pertains to *onset* and *duration*. For example, rather than a column specifying trial durations, a file might contain a column for trial_onset and another column for trial_offset. Users can specify an arithmetic method (Subtract, Add) and choose the two columns that when subtracted or added create the duration value. It should be noted that this arithmetic approach only applies to event columns involving time-based values. Additionally, users can specify for timed-based columns whether the values are in seconds or milliseconds (BIDS requires that these values be reported in seconds). 

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS6_events_mapping.png"/>
</td></tr></table>
<br>
<!-- ![Events Mapping](./img/ezbids/Levitas_etal_figureS6_events_mapping.png) -->

!!! warning Timing files for different tasks
    Currently, ezBIDS can only handle timing files that have identical column names. In theory, users can upload timing files pertaining to different `func/bold` tasks, but only if all files contain identical column names. 

### 6. Dataset Review

Upon reaching this page, all individual imaging sequences are provided in the order they were collected for each subject’s (and session’s) scan. Here, users can make edits/modifications to individual files (e.g., set a sequence to `exclude` due to known participant motion, etc) that was otherwise not possible back on the Series Mapping page.

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS7_dataset_review.png"/>
</td></tr></table>
<br>
<!-- ![Dataset Review](./img/ezbids/Levitas_etal_figureS7_dataset_review.png) -->

All edits/modifications made on the Series Mapping and Events pages are applied and show for each indivudal imaging sequence. ezBIDS applies a run entity label to sequences if there were multiple occurrences during the scan. Additionally, ezBIDS applies a volume threshold on 4D data (e.g., `func/bold`) to check for sequences that might have had to be restarted during the scan. The threshold is calculated based on the expected numbers of volumes collected during a 60-sec period, given the TR (`60/TR`). If a 4D sequence does not meet this threshold, ezBIDS sets the datatype/suffix pair to “exclude”, unless modified by the user.

If timing files were uploaded and properly matched, their datatype/suffix pair is set to `func/events` and placed underneath the corresponding `func/bold` sequence. If however, ezBIDS could not determine the match, these timing events files are placed at the bottom of the list with entity placeholder values (`XX1`, `XX2`, etc). Once manually corrected, they are automatically placed underneath their corresponding `func/bold`.

### 7. Deface

Users have the option (recommended) to deface all anatomical images in order to further anonymize the data. ezBIDS provides two defacing procedures: [Quickshear](https://github.com/nipy/quickshear) and [pyDeface](https://github.com/poldracklab/pydeface). Depending on the number of anatomical files in the uploaded data, this process may take several minutes, though ezBIDS can parallelize this process with six concurrent defacing processes at a given time. If the defacing is suboptiomal (i.e. defacing cuts into the brain itself), users may try again with the other defacing option or simply specify that they do not wish to deface the anatomical data.

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS8_defacing.png"/>
</td></tr></table>
<br>
<!-- ![Defacing](./img/ezbids/Levitas_etal_figureS8_defacing.png) -->

### 8. Participants Info

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS9_participant_info.png"/>
</td></tr></table>
<br>
<!-- ![Participant Info](./img/ezbids/Levitas_etal_figureS9_participant_info.png) -->

For each subject ID specified, users may specify phenotypical information pertaining to each participant. By default, ezBIDS attempts to provide the *sex* and *age* of participants if this information is present in the metadata (typically only specified if non-anonymized data is uploaded). Users may add additional columns (e.g., *handedness*) or delete existing columns.

### 9. Get BIDS

At this stage, users can click on the green "Finalize" button for ezBIDS to apply all user edits/modifications to create a BIDS-compliant dataset. Users may elect to keep any data that was set to “exclude” if they so choose.

<table><tr><td>
    <img src="./img/ezbids/Levitas_etal_figureS10_get_bids.png"/>
</td></tr></table>
<br>
<!-- ![Get BIDS](./img/ezbids/Levitas_etal_figureS10_get_bids.png) -->

 Once complete, two screens are presented. On the left is a Linux tree structure showing the BIDS dataset organization and file names. On the right is a screen output of the [bids-validator](https://bids-standard.github.io/bids-validator/), which provides a final check to ensure that the data is BIDS compliant. Any validator warnings appear in yellow and can be ignored (but are good to resolve later), and errors appear in red, indicating that the dataset is not BIDS-compliant for some specified reason and requires correction. If users need or wish to return to previous pages for additional edits/modifications they may do so. Then, upon returning to this page, users must click the green "Rerun Finalize Step" to apply all new changes. Once a BIDS-compliant dataset is generated, users can click on the blue "Get BIDS" button to download their zipped data to their local computer/server, and/or send their BIDS data to [OpenNeuro](https://openneuro.org/) or [brainlife.io](https://brainlife.io/about/). Users may also download a Subject Mapping JSON file detailing the mapping between the original subject names and their new BIDS subject IDs. Additionally, users may download a configuration/template JSON file that details all information and changes made during the ezBIDS session. This file can be uploaded with subsequent uploads to reduce time spent on edits/modifications.

### 10. Feedback

This page details the ezBIDS developers, funding sources, and where to leave questions, comments, or suggestions. 

## Additional ezBIDS features

### 1. Configuration template

Once ezBIDS has converted uploaded data to BIDS, users have the option to download a configuration/template file (*finalized.json*), which contains all information, including user edits/modifications, from the ezBIDS session. When this file is uploaded with subsequent data (of the same dataset), the information in the configuration file is applied to the current ezBIDS session, reducing future time spent on future edits/modifications. The information from the configuration file is applied on the following pages:

1. **Dataset Description** - Any information here is automatically generated.
2. **Subjects/Sessions** - ezBIDS will attempt to determine the new subject ID(s) and session ID(s), though this depends on how easily ezBIDS could determine this information, and may still require edit(s).
3. **Series Mapping** - All edits made during the first use are applied here.
4. **Events** - If timing files associated with `func/bold` sequences were uploaded in previous and current ezBIDS sessions, the *events.tsv* column mappings are applied to the current timing file(s) data.
5. **Participants Info** - Any newly specified columns (e.g., *handedness*) are applied here as well. 

If new/different data is uploaded which was not present in the previous upload, ezBIDS will proceed as usual. The configuration also enables many-to-one or one-to-many mapping, meaning that the *finalized.json* information can pertain to a single subject but be applied to a current upload with multiple subejcts/sessions. The *finalized.json* file can be renamed if desired, however it needs to end in "finalized.json" in order for ezBIDS to know what to look for. 

!!! note
    Uploading a *finalized.json* file does not mean that ezBIDS runs automatically from start to finish. When converting data to BIDS, user approval is judicious.

### 2. dcm2niix error alerts

ezBIDS, like most BIDS converters, users [dcm2niix](https://github.com/rordenlab/dcm2niix) to convert DICOM files to NIfTI and JSON (and bval/bvec) formatted files. These files are required by BIDS and used by many MRI processing & analysis tools. If dcm2niix generates an error for a specific or series of DICOMS during this conversion process, ezBIDS will display the message for users.

<table><tr><td>
    <img src="./img/ezbids/ezBIDS_dcm2niix_error.png"/>
</td></tr></table>
<br>

It is recommended that users open an issue on the dcm2niix [issues page](https://github.com/rordenlab/dcm2niix/issues) to resolve any detected dcm2niix errors. However, users may still proceed with ezBIDS, as the error does not pertain to the entire uploaded data but rather a specific DICOM file(s). It should be noted though that the offending file(s) might result in an improper or corrupted NIfTI file that doesn't properly convert to BIDS. 

### 3. Visualizing Imaging data

In addition to providing screenshots of imaging data, ezBIDS comes with the [NiiVue](https://github.com/niivue/niivue) package, a web-based visualization tool for neuroimaging that can run on any operating system and any web device (phone, tablet, computer). On each image screenshot, users may click on the "NiiVue" button to open NiiVue and view their data.

### 4. Installable version of ezBIDS

In the coming months, an installable version of ezBIDS will be made available through Docker, negating the need for data upload. With this, data will instead be accessible locally.

## Conclusions

Any ezBIDS-related questions can be posted/asked at the following places:
1. As an issue on the ezBIDS [GitHub repository](https://github.com/brainlife/ezbids)
2. The brainlife.io slack channel
3. [NeuroStars](https://neurostars.org/)
4. Direct email (dlevitas@iu.edu)

If you've reached this point, you're all set to begin converting your imaging data with ezBIDS!