# Registering App

Once you have developed your app (on github), you can now register it on Brainlife so that you can run it through Brainlife UI and let other users discover your app.

Under [App page](https://brainlife.io/apps), click `Plus Button` at the bottom right corner of the page. App registration form should open.

Let's go through each sections.

## Detail

![dataset](/img/app.detail.png)

Enter `name` and `Git Repository Name` field. On `Git Repository Name`, please enter only the organization / repository name (like `brain-life/app-life`) not the full github repo URL.

All other fields in this section are optional, but you could populate following fields.

### Avatar 

You can enter `avatar` URL if you have URL for an avatar that you'd like to use for your app. Please choose a square image with `https://` URL (not http://). Avatar may sounds superflusou, but please keep in mind that, there are a lot of app registered in Brainlife and this might be the only visual queue that user could identify and look for among many names / descriptions for other apps.

### Project

By default, all app are *public*; anyone can find it and execute your app (if they have resource to run them). If you'd like to make your app only available within specific project, you can enter the `project name` and only the member of that project will be able to find and launch your app. This might be useful if you are still developing your app and wants to keep it hidden until you make a formal *release*. 

### Branch

If you don't specify the github repo's branch name, it uses `master` branch by default. As with any other project, you will most likely making changes to your `master` branch after you register your app, which means user won't be able to reproduce the output with exactly the same version of the code used to generate original output. Once you finish developing your app, you should consider creating a release branch (like `release_1.0`) and freeze the code which will be executed by the Brain-Life by specifying the branch name. Please see [Versioning Tip](/apps/versioning) for more info.

## Input Datasets

Here you can define input datasets with specific datatypes that your app is expecting.

![dataset](/img/input.datatype.form.png)

### Datatype Tags

Sometimes you want to be specific about the type of dataset within a particular datatype that your app can handle. For example, `neuro/anat/t1w` could be ACPC aligned, or not, `neuro/dwi` could be single-shell, or not, etc. Brainlife adds specificity to each datatype through *datatype tags*. Please enter any datatype tags that your app would require under `Datatype Tags` field. Brainlife will only allow users to select datasets for your app that meets specified datatype tags. 

!!! warning
    Please don't get confuse *Datatype tags* with *Dataset tags* that user can edit under dataset dialog. Dataset tags simply allows user to make it easier to search or bulk process subset of the datasets. Datatype tags, on the other hand is part of datatype and can not be modified once dataset is created. 

### File Mapping

Once you select the datatype / tags of your input data, you need to configure how you want it to be presented in the `config.json` by mapping the json key in `config.json` to a particular file / directory inside the datatype.

```
{
    "t1": "../path/to/t1.nii.gz"
}
```

For example, if you use `anat/t1w` as an input dataset, you can specify `config.json key` field with any json key (such as "t1") and selecting which file/dir within the datatype you want to reference (like.. "../path/to/t1.nii.gz").

You might be wondering why there is ID field at the top of the input form as well as the json key ID. The input ID at the top is mainly used internally by Brainlife UI for allowing user to select an input dataset. As an app developer, you really don't need to worry about the input ID.. as long as they are unique across all inputs. An input selection could be mapped to multiple fields in `config.json` which is what you probably most care about.

## Output Datasets

Similar to the input dataset, you can specify the datatypes of your output datasets here. It's up to your app to produce output in the correct file structure / file names to be compatible with specified datatype. (Please read [datatypes page](/user/datatypes) for more info.) 

### Datatype File Mapping

By default, Brainlife expects you to generate all files pertaining to a specific datatype on the current working directory. However, if you have more than 1 output datasets, or have multiple datasets with the same datatype, file name collision could occur. To avoid this, you will need to output each datasets under a different filename, or use sub-directories to store files for each datasets. 

For example, following example show that the app is outputing a file named `output.DT_STREAM.tck` which is treated as a `track` file output for `neuro/track` datatype.

![resources](/img/app.output.png)

Or, if you'd like to store the output files under a sub directory, you can specify the directory by doing following.

![resources](/img/app.output2.png)

### **raw** datatype

Many app creates datatypes that are not meant to be used by any other app. You can use `raw` datatypes to output and archive such outputs. You should avoid using `raw` input datatype as an input to another app. If you need to use do this, please contact the developer of the upstream app generating the `raw` output dataset and ask them to define a new datatype and use it instead of `raw` datatype.

## Configuration Parameters

Configuration parameters allows users to enter any number (integer/float), boolean(true/false) or string parameters as configuration to your app. You can also define enum parameter which lets users select option from multiple selections.

![resources](/img/app.config.png)

* **Placeholder** 

    For each input parameter, you can set a placeholder; a string displayed inside the form element if no value is specified. For example, you could use placeholder to let user know the behavior of the app if user doesn't specify any value for that parameter. 

* **Description** 

    Some parameter types let you specify a description which will be displayed next to the input parameter to show detailed explanation about the input parameter. Please provide enough details for both novice and experienced users of your app.

Finally, click `Submit`. Visit the apps page to make sure everything looks OK.

## README / Description / Topics

Brainlife uses as much information as possible from your github repo to pull information such as ..

* App Description / Topics

    Github's description is used to display Brainlife app description. Please be sure to enter description that shows what the app does, and what user can do with the output. Github topics are also used as Brainlife's app `categories`. Please look through the existing categories already registered in Brainlife, and use one or more of those categories to help users find your app more easily. 

    ![resources](/img/app.freesurfer.png)

* README.md

    Brainlife displays the copy of README.md content from your github repo. You can use any images, katex equations, and any other standard markdown content. 
    
    You could include information such as following in your README.md

    * What the app does, and how it's implemented (tools, libraries used)
    * What the app produces
    * Any diagrams / sample output images, etc.
    * What user can do with the output data.
    * Details on how the algorithm works.
    * Computational cost / resources required to run the app (how long does it take to run, minimum required memory / cpu cores, etc..)
    * How can other user contribute (are you accepting PR?)
    * References to other published papers, or list of contributors.

    Brainlife's target audiences are both novice and experienced neuroscience researchers. Please be sure to cater both audiences.

## Enabling app on resource

Once you registered your app on Brain-Life, you then need to have a resource where you can run your app. Resource could be any VM, HPC clusters, or public / private cloud resources. Only the resource owners can enable your app to run on their resource. Please contact the resource administrator for the resource where you'd like to submit your app.

If you have access to your own computing resources, you can register and run your apps there also. Please read [registering resource page](/resources/register.md) for more detail. Please keep in mind that, only the owner of the resource can run the apps on personal (un-shared) resources. To allow other users to run your app, you will need to enable the app on Brainlife's shared resources.

!!! note
    For all Brainlife default resources, please contact [Soichi Hayashi](mailto:hayashis@iu.edu) and request to enable your app on those resources.

Once your app is enabled on a resource, you should see a table of resources where the app can run under the app detail page.

![resources](/img/app.resources.png)