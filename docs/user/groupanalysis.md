# Group Analysis
Brainlife.io's "Group Analysis" functionality allows for seamless project-level analysis directly on brainlife. You'll no longer have to download data to your local machine to perform your statistical analyses!

This tutorial will walk you through a short example of generating group-average tract profiles from the neuro/tractmeasures datatype. This is a simple walk-through, designed to be used in conjunction with this tutorial: . 

Specifically, four topics will be covered:

1. Installing modules and classes, and defining useful functions
2. Collecting subjects-information and tractmeasures data from the secondary warehouse
3. Printing and visualizing variables
4. Generating and saving figures
    
For this, we will use data from "Collegiate athlete brain data for white matter mapping and network neuroscience" (Caron et al, 2021, Nature Scientific Data).

# Installing modules and classes
As with any statistical software, it's important to load and install useful functions and software packages for managing and manipulating data. For this project, we will need to use a few commonly used software packages, including matplotlib, numpy, pandas, and seaborn. Most importantly, we will need to load the "requests" class, as this will be used to grab a data list from the secondary warehouse.

Like with any other notebook, installing modules is as simple as hitting 'Shift'+'Enter' in the cell containing the desired code.


```python
# Install modules and classes that will be useful for our profile analysis.
# To load, hit 'Ctrl'+'Shift' in this cell
import gc
import os,sys,glob
from matplotlib import colors as mcolors
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import requests
import json
import seaborn as sns
from scipy import stats
```

# Defining useful functions
Like with any other Jupyter notebook, defining functions on brainlife is the exact same process. Explaining the process of defining a script is beyond the scope of this tutorial, however if you'd like more information on functions in python, here's a useful tutorial:

In the following cells, we've defined some useful functions that will be used later for figure generation and data manipulation. There's small documentation on each function describing what the function does and the inputs. Like with installing modules, to define these functions in the kernel, hit 'Shift'+'Enter' in the following cells.

# Sample Group Analysis Code
```python
## Here we can define useful functions, just like in any python script or Juypter notebook. These functions in particular will be
## useful for generating tract profiles and group averages

# To define these within the kernel, hit 'Shift'+'Enter' in this cell

# simple display or figure save function
# dir_out is the directory where the images should be saved. if "", will just show the plot instead of saving
# x_measure is the measure on the x-axis of the plot
# y_measure is the measure on the y-axis of the plot
# img_name is a substring for the plot to use to differentiate different plots
def saveOrShowImg(dir_out,x_measure,y_measure,img_name):

    # save or show plot
    if dir_out:
        if not os.path.exists(dir_out):
            os.mkdir(dir_out)

        if x_measure == y_measure:
            img_name_eps = img_name+'_'+x_measure+'.eps'
            img_name_png = img_name+'_'+x_measure+'.png'
        else:
            img_name_eps = img_name+'_'+x_measure+'_vs_'+y_measure+'.eps'
            img_name_png = img_name+'_'+x_measure+'_vs_'+y_measure+'.png'

        plt.savefig(os.path.join(dir_out, img_name_eps))
        plt.savefig(os.path.join(dir_out, img_name_png))
    else:
        plt.show()

    plt.close()

### tract profile data
# uses matplotlib.pyplot's plot and fill_between functions to plot tract profile data from stat. useful for publication worthy figure
# requires stat to be formatted in way of AFQ_Brwoser and Yeatman et al 2018 () 'nodes.csv' files
# groups is a list array of names of groups found in 'classID' of stat to plot
# colors is a dictionary with the classID from groups set as the key and a color name as the value. will use these colors in profiles
# tracks is a list array that will be looped through to make plots. if only one track is wanted, set structures=['structure_name'], with 'structure_name' being the name of the track in the 'structureID' field of stat
# stat is the pandas dataframe with all of the profile data. each row is a node for a track for a subject
# diffusion_measures is a list array of the column measures found within stat. was developed with diffusion MRI metrics in mind, but can be any measure
# summary_method is a string of either 'mean' to plot the average profile data, 'max' to plot max, 'min' to plot min, and 'median' to plot median
# error_method is a string of either 'std' for the error bars to be set to the standard deviation or 'sem' for standard error of mean
# dir_out and imgName are the directory where the figures should be saved and the name for the image. will save .pdf and .png
# if want to view plot instead of save, set dir_out=""
def plotProfiles(structures,stat,diffusion_measures,summary_method,error_method,dir_out,img_name):

    # loop through all structures
    for t in structures:
        print(t)
        # loop through all measures
        for dm in diffusion_measures:
            print(dm)

            imgname=img_name+"_"+t+"_"+dm

            # generate figures
            fig = plt.figure(figsize=(15,15))
            fig.patch.set_visible(False)
            p = plt.subplot()

            # set title and catch array for legend handle
            plt.title("%s Profiles %s: %s" %(summary_method,t,dm),fontsize=20)

            # loop through groups and plot profile data
            for g in range(len(stat.classID.unique())):
                # x is nodes
                x = stat['nodeID'].unique()

                # y is summary (mean, median, max, main) profile data
                if summary_method == 'mean':
                    y = stat[stat['classID'] == stat.classID.unique()[g]].groupby(['structureID','nodeID']).mean()[dm][t]
                elif summary_method == 'median':
                    y = stat[stat['classID'] == stat.classID.unique()[g]].groupby(['structureID','nodeID']).median()[dm][t]
                elif summary_method == 'max':
                    y = stat[stat['classID'] == stat.classID.unique()[g]].groupby(['structureID','nodeID']).max()[dm][t]
                elif summary_method == 'min':
                    y = stat[stat['classID'] == stat.classID.unique()[g]].groupby(['structureID','nodeID']).min()[dm][t]

                # error bar is either: standard error of mean (sem), standard deviation (std)
                if error_method == 'sem':
                    err = stat[stat['classID'] == stat.classID.unique()[g]].groupby(['structureID','nodeID']).std()[dm][t] / np.sqrt(len(stat[stat['classID'] == stat.classID.unique()[g]]['subjectID'].unique()))
                else:
                    err = stat[stat['classID'] == stat.classID.unique()[g]].groupby(['structureID','nodeID']).std()[dm][t]

                # plot summary
                plt.plot(x,y,color=stat[stat['classID'] == stat.classID.unique()[g]]['colors'].unique()[0],linewidth=5,label=stat.classID.unique()[g])

                # plot shaded error
                plt.fill_between(x,y-err,y+err,alpha=0.2,color=stat[stat['classID'] == stat.classID.unique()[g]]['colors'].unique()[0],label='1 %s %s' %(error_method,stat.classID.unique()[g]))

            # set up labels and ticks
            plt.xlabel('Location',fontsize=18)
            plt.ylabel(dm,fontsize=18)
            plt.xticks([x[0],x[-1]],['Begin','End'],fontsize=16)
            plt.legend(fontsize=16)
            y_lim = plt.ylim()
            plt.yticks([np.round(y_lim[0],2),np.mean(y_lim),np.round(y_lim[1],2)],fontsize=16)

            # remove top and right spines from plot
            p.axes.spines["top"].set_visible(False)
            p.axes.spines["right"].set_visible(False)

            # save image or show image
            saveOrShowImg(dir_out,dm,dm,imgname)

## Here we define some useful scripts for collecting the data and computing on the data.

# collects data from warehouse.
# datatype is the datatype name
# datatype_tags are the datatype tags of the requested datatype
# tags are the tags of the requested datatype
# filename is the name of the file within the datatype to use
# subjects_data is a pandas dataframe of the subject data. useful for concatenation
# colors is a dictionary defining colors for groups
# outPath is the path where the data should be saved
def collectData(datatype,datatype_tags,tags,filename,subjects_data,colors,outPath):

    # grab path and data objects
    objects = requests.get('https://brainlife.io/api/warehouse/secondary/list/%s'%os.environ['PROJECT_ID']).json()

    # subjects and paths
    subjects = []
    paths = []

    # set up output
    data = pd.DataFrame()

    # loop through objects
    for obj in objects:
        if obj['datatype']['name'] == datatype:
            if datatype_tags in obj['output']['datatype_tags']:
                if tags in obj['output']['tags']:
                    subjects = np.append(subjects,obj['output']['meta']['subject'])
                    paths = np.append(paths,"input/"+obj["path"]+"/"+filename)

    # sort paths by subject order
    paths = [x for _,x in sorted(zip(subjects,paths))]

    for i in paths:
        tmpdata = pd.read_csv(i)
        if 'classID' in tmpdata.keys():
            tmpdata = pd.merge(tmpdata,subjects_data,on=['subjectID','classID'])
        else:
            tmpdata = pd.merge(tmpdata,subjects_data,on='subjectID')
        data = data.append(tmpdata,ignore_index=True)

    # replace empty spaces with nans
    data = data.replace(r'^\s+$', np.nan, regex=True)

    # output data structure for records and any further analyses
    # subjects.csv
    data.to_csv(outPath,index=False)

    return data

## will cut nodes on tract profiles for each participant and tract
# data is the pandas dataframe containing the profile data. needs a column called 'nodeID' describing the node position for each row
# num_nodes is the number of requested nodes. will cut the first and last residual nodes, leaving you a profile describing the central num_nodes
# outPath is the path where the data should be saved
def cutNodes(data,num_nodes,outPath):

    # identify inner n nodes based on num_nodes input
    total_nodes = len(data['nodeID'].unique())
    cut_nodes = int((total_nodes - num_nodes) / 2)

    # remove cut_nodes from dataframe
    data = data[data['nodeID'].between((cut_nodes)+1,(num_nodes+cut_nodes))]

    # replace empty spaces with nans
    data = data.replace(r'^\s+$', np.nan, regex=True)

    # output data structure for records and any further analyses
    # dataPath+'/'+foldername+'-'+savename+'.csv'
    data.to_csv(outPath,index=False)

    return data

## will compute mean tract data
# data is pandas dataframe containing profiles data. needs columns 'subjectID', 'classID', and 'structureID' containing information
# describing the subject, group, and tract
# outPath is the path where the data should be saved
def computeMeanData(data,outPath):

    # make mean data frame
    data_mean =  data.groupby(['subjectID','classID','structureID']).mean().reset_index()
    data_mean['nodeID'] = [ 1 for f in range(len(data_mean['nodeID'])) ]

    # output data structure for records and any further analyses
    #dataPath+'/'+foldername+'-'+savename+'.csv'
    data_mean.to_csv(outPath,index=False)

    return data_mean
```

# Setting up top-level variables
Like with any other project, it is important to define some top-level variables including the current working directory and directories for saving data and figures. On brainlife.io, this is done in the same way it's done in any python script or notebook. Here, we define the current working directory as "topPath", and define our data output and image output directories as "data_dir" and "img_dir". If these directories are not already created, it will make these directories inside the current working directory. These directories and their contents can then be downloaded to be used for figure generation!

Some other useful variables are defined here as well. Specficially, we define the group names as "groups", and set up a colors dictionary for each group. Specifically, in this study, there were three groups: football players (FB), cross-country runners (CC), and non-athletes (NonAth). We also set up the number of nodes we want to use in our profiles as "num_nodes".

```python
# To define these within the kernel, hit 'Shift'+'Enter' in this cell

#### set up paths
print("setting up variables")

# grab paths
topPath = './'
if not os.path.exists(topPath):
    os.chdir(topPath)
data_dir = topPath+'/data/' # output data directory
if not os.path.exists(data_dir):
    os.mkdir(data_dir)
img_dir = topPath+'/img/'
if not os.path.exists(img_dir):
    os.mkdir(img_dir)

## groups, colors, measures, domains, and covariates
groups = ['FBL','CC','NonAth']
colors_array = ['orange','pink','blue']
# diff_measures = ['ad','fa','md','rd','ndi','isovf','odi']
num_nodes = 180

## loop through groups and identify subjects and set color schema for each group
colors = {}

# this is based on group identifiers in the subject numbers: 1_ = football, 2_ = cross country, 3_ = non-athlete
for g in range(len(groups)):
    # set colors array
    colors_name = colors_array[g]
    colors[groups[g]] = colors_array[g]
print("setting up variables complete")
```

# Collecting data
Once our top-level variables are defined, we can start collecting our subject-information from our participants.json file and their tract profile data. The subject-information data is defined in the 'Detail' tab of each project, and can be accessed directly inside our kernel.

```python
# To load the data within this kernel, hit 'Shift'+'Enter' in this cell

#### loading subjects data
print("loading subject data")

if os.path.isfile(data_dir+'/subjects.csv'):
    subjects_data = pd.read_csv(data_dir+'/subjects.csv')
else:
    # load participants info
    import json
    with open("input/participants.json") as f:
        participants = json.load(f)

    with open("input/participants_column.json") as f:
        participants_column = json.load(f)

    # create subjects dataframe
    subjects_data = pd.DataFrame.from_dict(participants).T
    subjects_data.reset_index(inplace=True)
    subjects_data.rename(columns={'index': 'subjectID','group': 'classID'},inplace=True)
    subjects_data['colors'] = [ colors[c] for c in colors for f in subjects_data[subjects_data['classID'] == c]['classID'] ]
    subjects_data['num_labels'] = [ int(1) if f == 'FBL' else int(2) if f == 'CC' else int(3) for f in subjects_data['classID'] ]

    # output data structure for records and any further analyses
    subjects_data.to_csv(data_dir+'/subjects.csv',index=False)

#### loading tract profiles data
print("loading tract profile data")
if os.path.isfile(data_dir+'/tract_total_nodes.csv'):
    track_data = pd.read_csv(data_dir+'/tract_total_nodes.csv')
else:
    track_data = collectData('neuro/tractmeasures','macro_micro','designer','output_FiberStats.csv',subjects_data,colors,data_dir+'/tract_total_nodes.csv')

print("loading data complete")
```

# Printing and visualizing variables
You can easily look at the collected data by defining a new cell (the '+' button at the top of the notebook) and entering the variable name of interest.

```python
# To see the tract_data dataframe, hit 'Shift'+'Enter' in this cell
track_data
```

# Data manipulation and figure generation
Now that the subjects data and track_data are loaded, we can start manipulating the data and generating some figures! Specifically, we want to generate profiles with only a certain number of nodes ('num_nodes'). Once the profiles are cut, we can generate group-average profile!

For this tutorial, we will generate group-average tract profile data for measures FA and ODI within the forceps major tract.

```python
# To cut data and generate profiles, hit 'Shift'+'Enter' in this cell

## cut nodes
if os.path.isfile(data_dir+'/track_cut_nodes.csv'):
    track_data_cut = pd.read_csv(data_dir+'/track_cut_nodes.csv')
else:
    track_data_cut = cutNodes(track_data,num_nodes,data_dir+'/track_cut_nodes.csv')

# DTI/NODDI tract profiles (SD error bars)
diff_measures = ['fa','odi']
plotProfiles(['forcepsMajor'],track_data_cut,diff_measures,'mean','sem',"","")

# To save the figures, uncomment the following line
# plotProfiles(['forcepsMajor'],track_data_cut,diff_measures,'mean','sem',img_dir,"profiles")
```

Tutorial complete! You've now performed group analysis directly on brainlife.io!
