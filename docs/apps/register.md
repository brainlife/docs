# Registering App

Once you have developed your app (on github), you can now register it on Brainlife so that you can run it through Brainlife UI and let other users discover your app.

First, go to Brainlife's [App page](https://brainlife.io/apps), then click the big `Plus Button` at the bottom right corner of the page. You should be seeing a form like this.

![app form](/img/app.form.png)


## Detail

Enter `name` and `Git Repository Name` field. 

!!! warning
    On `Git Repository Name`, please enter only the organization / repository name (like `brain-life/app-life`) not the full github repo URL.

All other fields in this section are optional.

### Avatar 

You can enter `avatar` URL if you have URL for an avatar that you'd like to use for your app. Please choose a square image with `https://` URL (not http://). Avatar may sounds superflusou, but please keep in mind that, there are a lot of app registered in Brainlife and this might be the only visual queue that user could identify and look for among many names / descriptions for other apps.

### Project

By default, all app are *public*; anyone can find it and execute your app (if they have resource to run them). If you'd like to make your app only available within specific project, you can enter the `project name` and only the member of that project will be able to find and launch your app. This might be useful if you are still developing your app and wants to keep it hidden until you make a formal *release*. 

### Branch

If you don't specify the github repo's branch name, it uses `master` branch by default. As with any other project, you will most likely making changes to your `master` branch after you register your app, which means user won't be able to reproduce the output with exactly the same version of the code used to generate original output. Once you finish developing your app, you should consider creating a release branch (like `release_1.0`) and freeze the code which will be executed by the Brain-Life by specifying the branch name. Please see [Versioning Tip](/apps/versioning) for more info.

## Configuration Parameters

Configuration parameters allows users to enter any number, boolean(true/false) or string parameters. You can also define enum parameter which lets users select one of from multiple string labels. 


* *Placeholder* For each input parameter, you can set a placeholder; a string displayed inside the form element if no value is specified. You can use placeholder to let user know the behavior of the app if you don't specify any value for that parameter. 
* *Description* Some parameter types let you specify a description which will be displayed next to the input parameter to show some detailed explanation about the input parameter.

For example, with following configuration parameter definition ..
![input parameters definition]({{ "/assets/s/input_param_def.png" | absolute_url }})
 
When users submit your app, they will be presented with following UI.
![input parameter]({{ "/assets/s/input_param.png" | absolute_url }})

Your app will receive something like following in `config.json` generated on the local directory of your app. You have to parse this in your app.

```json
{
    "param1": 100
}
```

## Input Datasets

Most app requires one or more input datasets to operate on. Similarly to configuration parameters, you can define input datasets with specific datatypes where users will be required to select when they submit your app.
 
Sometimes, you want to be more specific about the type of data within a particular datatype. For example, anat/t1w could be ACPC aligned, or not, dwi could be single-shell-ed, or not, etc.. Brain-Life adds context to each datatype through *datatype tags*. "ACPC Aligned" anat/t1w can have a datatype tag of "acpc_aligned". If you know a specific datatype tags to limit the type of datasets that user should be selecting, you can specify those tags under the *Datatype Tags* section.

Once you select the datatype / tags, you also need to map the *key* used in `config.json` to particular file / directory inside the datatype. You can do this by adding *File Mapping*.

![datasets]({{ "/assets/s/input_datasets.png" | absolute_url }})

Above configuration allows user to select anat/t1w datatype and your app will receive the path to the t1.nii.gz via a `config.json` key of `t1`

## Ouput Datasets

Most app produces output datasets which are then fed to another app as their input datasets. You can specify the datatypes of your output datasets here. It's up to your app to produce output in the correct file structure / file names to be compatible with specified datatype. Please consult with Brain-Life staff if you are not sure. 

### *raw* datatype

Many app creates datatypes that are not meant to be used by any other app ("terminal apps"). If you want your output to be archived to the Brain-Life warehouse, you can define the output datatype as `raw` with a good datatype tags to distinguish with other raw datatypes. 

Finally, click submit. Visit the apps page to make sure everything looks OK.

## Enabling app on resource

Once you registered your app on Brain-Life, you then need to have resources where you can run your app through Brain-Life. Resource could be any VM, HPC clusters, or public / private cloud resources. Only the resource owners can enable your app to run on their resource. Please contact the resource administrator for the resource where you'd like to submit your app . 

For all Brain-Life's shared resources, please contact [Soichi Hayashi](mailto:hayashis@iu.edu).

Once your app is enabled on various resources, you should see a table of resources where the app can run under the app detail page.

![resources]({{ "/assets/s/resources.png" | absolute_url }})
