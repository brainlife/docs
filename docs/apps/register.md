
Once you have developed your app, you can register it on Brain-Life so that other users can discover and execute your app. 

First, head to [Brain-Life's Apps page](https://brainlife.io/warehouse/#/apps). Click on the `Plus Button` on the bottom right corner of the page. 
![plus]({{ "/assets/s/plus.png" | absolute_url }}) 

## Basic Info

On "New App" form, fill out name / description, and classification tags. Classification tag is used to organize apps into various groups to help user discover your app more easily.

You can leave `avatar` URL empty and Brain-Life will auto-generate an avatar for you.

`Project` field is optional, but if your app is written specifically for a particular project, you can select it here. If the select private project, your app will only be visible to the member of the project.

`Source Code` is where you enter the Github repository name. Please enter only the organization / repository name (like `brain-life/app-life`) not the full github repo URL.

You should leave the `Max Retry` field empty.

## Branch

If you don't specify the github repo's branch name, it uses `master` branch by default. As with any other project, you will most likely making changes to your `master` branch after you register your app, which means user won't be able to reproduce the output with exactly the same version of the code. Once you finish developing your app, you should consider creating a release branch (like `release_1.0`) and freeze the code which will be executed by the Brain-Life by specifying the branch name.

> Once you create a new branch, any bug fixes you are making on `master` branch should be back-ported to other branches. You should, however, not make any changes to branches unless absolutely necessary as it would break the data reproducibility. 

> If you make a backward incompatible changes and/or any changes that requires different set of configuration parameters / inputs, you should register entirely new app on Brain-Life with that new version.

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
