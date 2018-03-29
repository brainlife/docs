
## What is App?

Brainlife *Apps* are any code published on github that conforms to [ABCD specification](https://github.com/brain-life/abcd-spec). 

In a nutshell.. apps should..

* Provides an executable with a filename of `main` at the root of the git repo which execute the rest of the app (`main` could be a shell script, python script, etc..) On a batch system, Brainlife runs `qsub main` or `sbatch main` to submit the app depending on which local batch system is runs on.
* Receive all input parameters and paths to input files through `config.json` created by Brainlifes at runtime on the current working directory of the app. At runtime, Brainlife creates an empty directory somewhere on a compute resource, git clone the app, and places `config.json` along with other files stored in the git repo.
* Writes all output files in the current directory, in a structure expected by various `datatype` (more later).

!!! note 
    Brainlife app should follow the principle of [Do One Thing and Do It Well](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well) where a complex workflow should be split into several apps (but no more than necessary nor practical). 

Brainlife apps interface with other apps through Brainlife *datatypes* which are developer defined file / directory structure to hold specific data.

For example, we have following datatypes currently registered on Brainlife.

* neuro/anat/t1w (t1.nii.gz)
* neuro/anat/t2w (t2.nii.gz) 
* neuro/dwi (diffusion data and bvecs/bvals)
* neuro/freesurfer (entire freesurfer output)
* neuro/tract (tract.tck containing track data in tck format)
* neuro/dtiinit (dtiinit output)

So on..

Your app should read from one or more of these datatypes, and write output data in a format specified by these datatypes. This may mean you have to do some extra importing of input data and exporting/reformatting of the output data. We maintain our list of `datatypes` in our [brain-life/datatypes](https://github.com/brain-life/datatypes/tree/master/datatypes/neuro) repo. To create a new datatype, please open an issue, or send us PR with a new datatype definition file (.json). We do not modify datatypes once it's published. 

!!! note
    Before writing your apps, please browse [already registered apps](https://brainlife.io/warehouse/#/apps) and datatypes under Brain-Life.org to make sure that you are not reinventing parts of your application. If you find an app that is similar but does not exactly do what you need, please contact the developer of the app and discuss if the feature you need can be added to the already existing app. 
