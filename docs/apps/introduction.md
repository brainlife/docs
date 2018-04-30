# Introduction

## What is *App*?

Brainlife Apps are snippets of code comprising a (short) series of processing steps within a larger data analysis workflow. Apps are meant to be reusable by other users and not just by the App developer. Apps usage is value added to the work of the App Developer. So, the code in each App should use general tools and clarity in code writing so to make the App undeerstandable by other users. 

1. Apps are hosted on public [GitHub.com](https://github.com/search?q=org%3Abrain-life+app-) repositories. Apps can comprise any combination of MatLab, Python or other types code.
2. Apps must have a single executable file named `main` in the root directory of the git repository. In the most common case, `main` is a UNIX bash script that calls other code in the repository to run the algorithms for data analysis. The code for data analysis can be written in any language, or can be compiled binary code. 
3. Apps must read all input parameters and data files from a `config.json` file. `config.json` is created by Brainlife.io at runtime on the current working directory (`./`, [relative path](https://en.wikipedia.org/wiki/Path_(computing))) of the compute resource that your App will run on. But you do not have to think about this actually, just write a [relative path](https://en.wikipedia.org/wiki/Path_(computing)) in your code when loading files from the `config.json` file, no need for [absolute paths](https://en.wikipedia.org/wiki/Path_(computing)). 
4. Write all output files in the current directory (`./`), in a structure defined in a Brainlife [`datatype`](https://github.com/brain-life/datatypes). More information about [Brainlife datatypes](https://github.com/brain-life/datatypes) later.

Ideally, Apps should be packaged into [Docker containers](https://www.docker.com/what-container). But that is not a requirement. Apps Dockerizing will allow a broader App usage, because Apps can run on multiple compute systems and will most likely increase the impact of the code you write, with higher likelyhood of increasing the impact of your work as a Brainlife App developer. More information abotu [Apps Dockerization can be found here](https://brain-life.github.io/docs/apps/container/).

Brainlife Apps follow a technical specification called Application for Big Computational Data analysis or [ABCD](https://github.com/brain-life/abcd-spec)

## App Development Timeline

You would normally follow following steps to develop and register your App on Brainlife.

1. Develop an algorithm that runs on your laptop or local cluster with your test datasets.
2. Create a sample `config.json` but add it to `.gitignore` so that it won't be part of your repo. (You can include `config.json.sample` and symlink it to config.json)
3. Create `main` that parses `config.json` and pass it to your algorithm.
4. Publish it as public github repo.
5. Register your App on Brainlife. During this step, you can define what parameters and input file should be made available to your App via `config.json`.
6. Contact resource administrators and ask them to enable your App (more below). 

# Enabling App on a compute resource

App needs to be enabled on each compute resources to run. Each user will have a different set of resources that they have access to, but Brainlife provides default **shared** resources for all users. If you want anyone in the Brainlife to be able to run your App, you can [contact the resource administrators](brlife@iu.edu) of these default resources to enable your Apps.

You will need to discuss with resource administrators on how to handle any dependencies/libraries that your App might require. To make things easier and reproducible, you should consider Dockerizing you App's **dependencies** (but not the App itself) so that you can run your App through your container using [singularity](https://singularity.lbl.gov/) from your `main`. 

!!! hint
    Most compute resources now provide singularity which increases the number of resource where you might be able to run your Apps.

## App Launch Sequence

Brainlife executes an App in following steps.

1. A user requests to run your App through Brainlife.
2. Brainlife queries a list of compute resources that user has access to and currently available to run your App. Brainlife then determines the best compute resource to run your App.
3. Brainlife stages input datasets out of Brainlife's datasets archive, or transfer any dependent task's work directory that are required to run your App.
4. Brainlife creates a new working directory by git cloning your App on (normally) a resource's scratch disk space, and place `config.json` containing user specified configuration parameters and various paths the input files.
5. Brainlife then runs `start` hook installed on each compute resources as part of `abcd specification` (App developer should have to worry about this under the most circumstances).
6. On a PBS cluster, `start` hook then **qsub**s your `main` script and place it on the local batch scheduler queue.
7. Local job scheduler runs your `main` on a compute node and your App will execute your algorithm, and generate output on the working directory.
8. Brainlife periodically monitors your job and relay information back to the user.
9. Once job is completed, user archives an output dataset if the result is valid.

## Datatype

Different Brainlife Apps can exchange input/output datasets through Brainlife *datatypes* which are developer defined file/directory structure that holds specific set of data.

Here are some example of currently registered datatypes.

* neuro/anat/t1w (t1.nii.gz)
* neuro/anat/t2w (t2.nii.gz) 
* neuro/dwi (diffusion data and bvecs/bvals)
* neuro/freesurfer (entire freesurfer output)
* neuro/tract (tract.tck containing fiber track data)
* neuro/dtiinit (dtiinit output - dti output directory)
* generic/images (a list of images)
* raw (unstructured data often used during development)

Your App should read from one or more of these datatypes and write output data in a format specified by another datatype. By identifying existing datatypes that you can interoperate you can interface with datasets generated by other Apps and your output can be used by other Apps as their input.

We maintain a list of `datatypes` in our [brain-life/datatypes](https://github.com/brain-life/datatypes/tree/master/datatypes/neuro) repo. To create a new datatype, please open an issue, or submit a PR with a new datatype definition file (.json). We do not modify datatypes once it's published to preserve backward compatibility, but you can re-register new datatype under a different version.

!!! hint
    Brainlife app should follow the [Do One Thing and Do It Well](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well) principle where a complex workflow should be split into several smaller Apps (but no more than necessary nor practical) to promote code-reuse and help parallelize your workflow and run each App on the most appropriate compute resource available.

!!! hint
    Before writing your apps, please browse [currently registered Brainlife Apps](https://brainlife.io/warehouse/#/apps) and datatypes under Brainlife.io to make sure you are not reinventing Apps. If you find an App that is similar to what you need, please contact the developer of the App and discuss if the feature you need can be added to the App.
