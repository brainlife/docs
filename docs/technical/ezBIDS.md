# ezBIDS technical documentation

This document is meant for reseachers (or users) who wish to contribute to the ezBIDS project. A key feature of ezBIDS is its ability to parse incoming imaging data for BIDS relevant information, and organize information into a file that can be ready by the ezBIDS frontend for display on the web pages. This information file is known as the `ezBIDS_core.json`. Researchers interested in contributing to ezBIDS (e.g. adding coverage for M/EEG data) will need to understand the contents of the `ezBIDS_core.json` so that they can generate an identical file that is passed to the ezBIDS frontend.

## The ezBIDS Core

The following block code provides information pertaing to the contents of the `ezBIDS_core.json` file. Necessary keys are not indented and their data type and description are provided and indented on the following lines. If the key is a dictionary, a _fields_ key will be provided with how the dictionary key value should be specified. 

```yaml
# See https://bids-specification.readthedocs.io/en/stable/glossary.html#readme-files
"readme":
   "type": str
   "description": Details how the data(set) was converted. Default is "This data was converted using ezBIDS (https://brainlife.io/ezbids/). Additional information regarding this dataset can be entered in this file."
# See https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#dataset-description
"datasetDescription":
   "type": dict
   "description": Specific information detailing the dataset
   "fields":
      "Name":
         "type": str
         "description": Name of the dataset, default is “Untitled"
      "BIDSVersion":
         "type": str
         "description": BIDS standard used for conversion, default is "1.8.0"
      "DatasetType":
         "type": str
         "description": Interpretation of the dataset, default is "raw"
      "License":
         "type": str
         "description": License of the dataset, default is empty string
      "Authors":
         "type": list
         "description": List of authors who contributed to the dataset, default is empty list
      "Acknowledgements":
         "type": str
         "description": Text of contributors not in authors list, default is empty string
      "HowToAcknowledge":
         "type": str
         "description": How to acknowledge the authors, default is empty string
      "Funding":
         "type": list
         "description": Funding sources, default is empty list
      "EthicsApprovals":
         "type": list
         "description": Ethics committee approvals, default is empty list
      "ReferencesAndLinks":
         "type": list
         "description": References detailing dataset, default is empty list
      "DatasetDOI":
         "type": str
         "description": Digital object identifier (DOI) of dataset, default is empty string
      # See https://bids-specification.readthedocs.io/en/stable/glossary.html#objects.metadata.GeneratedBy
      "GeneratedBy": 
         "type": dict
         "description": Specifies provenance of dataset
         "fields":
            "Name":
               "type": str
               "description": Name of the BIDS conversion tool, default is "ezBIDS"
            "Version":
               "type": str
               "description": BIDS converter tool version, default is 1.0.0
            "Description":
               "type": str
               "description": Description of the tool, default is "ezBIDS is a web-based tool for converting MRI datasets to BIDS, requiring neither coding nor knowledge of the BIDS specification"
            "CodeURL":
               "type": str
               "description": BIDS converter tool URL, default is "https://brainlife.io/ezbids/"
            "Container":
               "type": dict
               "description": Specifies information regarding the conversion Container
               "fields":
                  "Type":
                     "type": str
                     "description": Container type, default is "docker"
                  "Tag":
                     "type": str
                     "description": Container tag, default is "brainlife/ezbids-handler"
      # See https://bids-specification.readthedocs.io/en/stable/glossary.html#sourcedatasets-metadata
      "SourceDatasets":
         "type": dict
         "description": Specifies the locations and relevant attributes of all source datasets
         "fields":
            "DOI": 
               "type": str
               "description": Digital object identifier (DOI), default is empty string
            # See https://bids-specification.readthedocs.io/en/stable/02-common-principles.html#bids-uri
            "URL":
               "type": str
               "description": Uniform Resource Identifier (URI)-formatted string, default is empty string
            "Version":
               "type": string
               "description": Version of the source dataset, default is empty string
# Subjects-specific information
"subjects":
   "type": list
   "description": List of dictionaries containing information pertaining to subjects
   "fields":
      "subject":
         "type": str 
         "description": subject ID for the BIDS dataset, minus the sub- portion (e.g., “01”)
      "PatientInfo":
         "type": list
         "description": List of dictionary with DICOM header information
         "fields":
            "PatientName":
               "type": str
               "description": Corresponds to DICOM field (0010,0010)
            "PatientID":
               "type": str
               "description": Corresponds to DICOM field (0010,0020)
            "PatientBirthDate":
               "type": str
               "description": Corresponds to DICOM field (0010,0030)
      "phenotype":
         "type": dict
         "description": Phenotype information
         "fields":
            "sex":
               "type": str
               "description": Sex of the patient, should be “n/a” if information unavailable
            "age":
               "type": int
               "description": Age of the patient, should be "n/a" if information unavailable
      "exclude":
         "type": boolean
         "description": Whether or not subject should be excluded from BIDS conversion
      "sessions":
         "type": list
         "description": list of dictionary (or dictionaries) pertaining to session information of specific subject
         "fields":
            "session":
               "type": str
               "description": Session ID, if applicable. Should be empty string if not
            "AcquisitionDate":
               "type": str
               "description": Information from the json sidecar generated by dcm2niix
            "exclude":
               "type": boolean
               "description": Whether or not session should be excluded from conversion
      "validationErrors":
         "type": list
         "description": List of any validation errors, default is empty list 


# Participants (subjects) Column information, see https://bids-specification.readthedocs.io/en/stable/glossary.html#participant_id-columns
"participantsColumn":
   "type": dict
   "description": Describes properties of participants such as age, sex, handedness, etc
   "fields":
      "sex":
         "type": dict
         "description": Long-form information pertaining to participant's sex
         "fields":
            "LongName":
               "type": str
               "description": Default value is "gender"
            "Description":
               "type": str
               "description": Default value is "generic gender field"
            "Levels":
               "type": dict
               "description": Levels of the sex (e.g. M or F)
               "fields":
                  "M":
                     "type": str
                     "description": Default is "male"
                  "F":
                     "type": str
                     "description": Default is "female"
      "age":
         "type": dict
         "description": Long-form information pertaining to participant's age
         "fields":
            "LongName":
               "type": str
               "description": Default is "age"
            "Units":
               "type": str
               "description": Default is "years"
# Participants (subjects) information
"participantsInfo":
   "type": dict
   "description": Some participants information
   "fields":
      "<index_value>":
         "type": dict
         "description": Index-value (beginning with 0) in string form
         "fields":
            "sex":
               "type": str
               "description": Sex of participant, should be "n/a" if unknown
            "age":
               "type": int
               "description": Age of participant, should be "n/a" if unknown
            "PatientName":
               "type": str
               "description": ParticipantName DICOM value, should be "n/a" if unknown
            "PatientID":
               "type": str
               "description": ParticipantID DICOM value, should be "n/a" if unknown
# Series-level information
"series":
   "type": list
   "description": List of dictionaries pertaining to information of data with unique series ID. Data having the four identical values in the (SeriesDescription, ImageType, EchoTime, RepetitionTime) DICOM fields are given the same series ID (numeric value beginning at 0)
   "fields":
      "SeriesDescription":
         "type": str
         "description": Corresponds to DICOM field (0008,103E)
      "EchoTime":
         "type": float
         "description": Corresponds to DICOM field (0018,0081)
      "ImageType":
         "type": list
         "description": Corresponds to DICOM field (0008,0008)
      "RepetitionTime":
         "type": float
         "description": Corresponds to DICOM field (0018,0080)
      "NumVolumes":
         "type": int
         "description": Number of volumes for the first instance data with the specific unique series ID
      "IntendedFor":
         "type": null
         "description": Null value placeholder for IntendedFor
      "B0FieldIdentifier":
         "type": null
         "description": Null value placeholder for B0FieldIdentifier
      "B0FieldSource":
         "type": null
         "description": Null value placeholder for B0FieldSource
      "nifti_path":
         "type": str
         "description": Absolute file path and name to the first instance data with the specific unique series ID
      "series_idx":
         "type": int
         "description": Unique series ID index value (begins at 0)
      "AcquisitionDateTime":
         "type": str
         "description": Corresponds to DICOM field (0008,002A). Should be "0000-00-00T00:00:00.000000" if unknown
      "entities":
         "type": dict
         "description": Key values from here (https://github.com/bids-standard/bids-specification/blob/master/src/schema/rules/entities.yaml) with empty string values
      "type":
         "type": str
         "description": BIDS DataType and Suffix pairing, separated by backslash character (e.g. "anat/T1w")
      "error":
         "type": str or None
         "description": Issue(s) caused by data conversion
      "message":
         "type": str
         "description": Description of how the type (datatype/suffix pairing) was identified, or how it couldn't be identified.
      "object_indices":
         "type": list
# Objects-level (i.e. all individual data) information 
"objects":
   "type": list
   "description": List of dictionaries, with each dictionary containing information from each individual data sequence
   "fields":
      "subject_idx":
         "type": int
         "description": Subject ID index numeric value
      "session_idx":
         "type": int
         "description": Session ID index numeric value
      "series_idx":
         "type": int
         "description": Unique Session ID index numeric value
      "AcquisitionDate":
         "type": str
         "description": Corresponds to DICOM field (0008,0022), YYYY-MM-DD format. Should be "0000-00-00" if unknown
      "AcquisitionTime":
         "type": str
         "description": Corresponds to DICOM field (0008,0032), formatted in hh-mm-ss. Should be "00:00:00.000000" if unknown
      "SeriesNumber":
         "type": int
         "description": Corresponds to DICOM field (0020,0011)
      "ModifiedSeriesNumber":
         "type": str
         "description": Modified SeriersNumber, with values <10 zero padded (e.g. 01, 02, etc)
      "IntendedFor":
         "type": null
         "description": Null value placeholder for IntendedFor
      "B0FieldIdentifier":
         "type": null
         "description": Null value placeholder for B0FieldIdentifier
      "B0FieldSource":
         "type": null
         "description": Null value placeholder for B0FieldSource
      "entities":
         "type": dict
         "description": Key values from here (https://github.com/bids-standard/bids-specification/blob/master/src/schema/rules/entities.yaml) with empty string values
      "items":
         "type": list
         "description": List of dictionaries pertaining to metadata and file paths of the individual data
         "fields":
            "path":
               "type": str
               "description": Absolute file path & name of the data json file
            "name":
               "type": str
               "description": Default is "json"
            "sidecar":
               "type": dict
               "description": The data's json metadata sidecar (see dcm2niix json output as example)
         "fields":
            "path":
               "type": str
               "description": Absolute file path and name of the data NIfTI file
            "name":
               "type": str
               "description": Default is "nii.gz"
            "pngPaths":
               "type": list
               "description": List of absolute paths and names of the screenshot png files created by createThumbnailsMovies.py
            "headers":
               "type": list
               "description": NIfTI header information from the nibabel command, str(nibabel.load(<nifti_path>.header)).splitlines()[1:]
      "PED":
         "type": str
         "description": The phase encoding value (e.g., AP, PA, RL, LR, IS, SI)
      "analysisResults":
         "type": dict
         "description": "More specific information regarding the individual data file"
         "fields":
            "NumVolumes":
               "type": int
               "description": Number of volumes for the data NIfTI file
            "errors":
               "type": list
               "description": Default is empty list of errors
            "warnings":
               "type": list
               "description": Default is empty list of warnings
            "filesize":
               "type": int
               "description": Size of the NIfTI file
            "orientation":
               "type": str
               "description": Orientation of the NIfTI file data (e.g. RAS)
            "section_id":
               "type": int
               "description": Which section the individual data is in, starts at 1 and progresses up when new localizers are detected
```

## Backend Workflow

### 1. Uploading Data
When a user begins uploading data, ezBIDS will create a new session using (post)/session API. A session organizes each ezBIDS upload/conversion process. For each session, a new DB/session collection is created with the mongo ID as session ID, and creates a unique working directory using the session ID on the backend server where all uploaded data is stored. Once all files are successfully uploaded, the client makes a (patch)/session/uploaded/:session_id API call and sets the session state to "uploaded" to let the ezBIDS handler knows that the session is ready to being preprocessing.

### 2. Preprocessing Data
The backend server polls for uploaded sessions, and when it finds "uploaded" session, it launches the preprocessing script, setting the session state to "preprocessing". This consists of several steps, including un-compressing all compressed folders/files, running *dcm2niix*, and creating a list file containing all the NIfTI and sidecar json files. The *ezBIDS_core.py* uses this to analyze the files and at the end create *ezBIDS_core.json*. When the preprocessing is complete, the session state will be set to "analyzed". The preprocessing step then loads *ezBIDS_core.json* json and copies the contents to DB/ezBIDS collection (not session collection) under an original key.

### 3. User interact with the session via web UI.
The web user interface (UI) detects the preprocessing completed by polling for session state, and loads the contents of ezBIDS_core.json via (get)/download/:session_id/ezBIDS_core.json API. User then view / correct the content of *ezBIDS_core.json*.

### 4. User request for defacing (optional)
Before the user finalizes editing the information, they are given chance to deface anatomical images. When requested, the UI will make a (post)/session/:sessino_id/deface API call with a list of images to deface. The backend stores this information as *deface.json* in the workding directory (workdir) and sets the session state to "deface". The backend defacing handler looks for the "deface" state session and sets it to "defacing" and launches the *deface.sh* script. Once defacing is completed, it will set the session state to "defaced".

*deface.sh* creates the following files under the workdir:

1. *deface.finished* (list the anatomical files successfully defaced)
2. *deface.failed* (list the anatomical files that failed defacing)

The UI polls these files to determine which files are successfully defaced (or not). The user can then choose whether they want to use the defaced anatomical file file or not (each object will have a field named defaceSelection set to either "original" or "defaced" to indicate which image to use).

### 5. Finalize
When the user clicks the "Finalize" button, the UI makes an API call for (post)/session/:session_id/finalize API. The UI passes the following content from the memory ($root).
```
{
    datasetDescription: this.datasetDescription,
    readme: this.readme,
    participantsColumn: this.participantsColumn,
    subjects: this.subjects, //for phenotype
    objects: this.objects,
    entityMappings,
}
```

The API then store this information as *finalized.json* in workdir, and copies the content to DB/ezBIDS collection under an updated key (to contrast with original key). The session status will be set to "finalized". This kicks off the finalized handler on the server side, and the handler resets the status to "bidsing" and runs the *bids.sh* script to generate the BIDS structure according to the information stored at the object level. Once finished, the session status will be set to "finished".

### 6. Access BIDS
Once session status becomes "finished", the user will be then allowed to download the final BIDS directory via the download API, or send to other cloud resources


## This repo was bootstrapped by

```
npm init vite@latest ui2 -- --template vue-ts
```

## Code Styling/Formatting Guide

This repo has a few guardrails in it to ensure that only clean, standardized, and uniform code is committed into the ezBIDS repository. It is suggested that VSCode be used when contributing to ezBIDS to make use of the Prettier VSCode extension for convenience.

There are a few safeguards active:

1. We use husky to run a git precommit hook and lint staged files using esLint
2. We use husky to run a git precommit hook and run a prettier style check on staged files
3. A `.vscode/settings.json` file is attached to this repository, which configures VSCode to allow formatting of files on save and on paste.

Make sure that you run npm install to install husky if you have not already.
You must run *npm run prepare-husky* the first time you touch the project in order to initialize git hooks.

You can run `npm run lint-staged` at any time in order to run a style check on the staged files.
`lint-staged` makes a call to prettier and eslint to check if there are any files that do not adhere to the code standard.
It does NOT overwrite any files.

NOTE: If you want to skip the hook for whatever reason, you can run `git commit --no-verify ...` 

VS Code Recommendations:

1. Install the Prettier VS Code extension `esbenp.prettier-vscode`. This will allow you to format files based on prettier rules.
2. Install the eslint VS Code extension `dbaeumer.vscode-eslint`. This will allow you to see lint errors as you're writing code.
3. Install the Volar extension `vue.volar`. This provides Vue language features.

NOTE: The recommended extensions to install for this project should appear as a notification in the bottom right corner of the VSCode screen the very first time you open the project. You can also open the command palette and go to "Show Recommended Extensions." Alternatively, you can directly navigate to `.vscode/extensions.json` and install the listed extensions.