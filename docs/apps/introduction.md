
## What is App?

Brainlife *Apps* are any code published on github that conforms to [ABCD specification](https://github.com/brain-life/abcd-spec) and registered on Brain-Life.org. *App* receives sets of input datasets where the format and structure of datatype conforms to registered datatypes on Brain-Life.org. 

At minimum, it should meet following requirements..

* Hosted on a public github repo.
* Provides an executable with a name `main` at the root of the repo which runs the rest of the app.
* App should receive all input parameters and paths to input files through `config.json`
* App should write all output files under the current directory set by Brainlife. You don't have to know where exactly the current directory be, as it will be different everytime your application is run.

Brain-Life's workflow service looks for a executable file called `main` which can be a PBS submit script, or any script written in bash, python, etc. The *App* then loads all input parameters (including paths to various input files) from `config.json` created by Brain-Life on the current directory. *App* then runs the algorithm and create any output files inside the current directory. *Apps* should exit with return code of 0 if successfully completed. Any non-0 exit code will be regarded as task being failed.

*App* should follow the principle of [Do One Thing and Do It Well](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well) where a complex workflow should be split into several apps (but no more than necessary nor practical to do so). 

*App* interfaces through Brain-Life *datatypes* which are pre-defined datatypes commonly used by various apps such as..

* anat/t1 (t1.nii.gz)
* anat/t2 
* dwi (diffusion data and bvecs/bvals)
* freesurfer (entire freesurfer output)
* tract (.tck file containing track data)

etc..

Your app should read from one or more of these common datatypes, and write to another set of common datatypes. This may mean you have to do an extra importing / exporting of your data, but it allows other apps to interoperate with your app and increases the chance of code-reuse. 

Before writing your apps, please browse [already registered apps](https://brainlife.io/warehouse/#/apps) and datatypes under Brain-Life.org to make sure that you are not reinventing parts of your application. If you find an app that is similar but does not exactly do what you need, please conctact the developer of the app and discuss if the feature you need can be added to the already existing app. 

## TODO

## config.json

Your app should read configuration from config.json in a local (working) directory. Here is an example of a sample config.json (https://github.com/brain-life/app-dipy-tracking/blob/master/config.json.template)

In Matlab, you can do something like following to read json
```
addpath(genpath('/N/u/hayashis/BigRed2/git/jsonlab'))
config = loadjson('config.json');
disp(config.some.param);
```

In Python, you can do..
```
import json
with open('config.json') as config_json:
    config = json.load(config_json)

# Load the data
print(config['data_file'])
```

jsonlab comes from `https://github.com/fangq/jsonlab.git`

Input parameter should include paths to various input files, or command line parameters passed to various sub-processes. By using config.json, it makes it easier to run it via Docker. (It also prepares your application to be more [ABCD Specification](https://github.com/soichih/abcd-spec) compatible.)

All input parameters are assumed to be text (char). You need to write your functions that are going to be MATLAB compiled with all the arguments as text. Arguments passing a number need to be given as text and within the function converted to integers values (str2num(), etc.). 


