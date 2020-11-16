!!! warning
    This is a draft. Comments are welcomed!

# Downloading Data-Object

Before downloading a data-object, let's see what data-objects are available for you to download.

### Querying project ID

```
$ bl project query -q o3d
Id: 5a022fc99c0d250055709e9c
Name: O3D
Admins: hayashis, avesani, frakkopesto
Members: hayashis, avesani, frakkopesto
Guests: 
Access: Access: public
Description: O3D (Open Diffusion Data and Derivative) is a reference repository for precision connectome mapping, white matter tracts analysis and algorithmic development.

(Returned 1 result)

```

### Querying Data-Object

```

$ bl data query -p 5a022fc99c0d250055709e9c
...

Id: 5bb5713a12512a003f32e9b6
Project: O3D
Admins: hayashis, avesani, frakkopesto
Members: hayashis, avesani, frakkopesto
Guests: 
Subject: 0008
Session: 
Datatype: neuro/wmc<dt_stream, afq>
Description: AFQ with no Life (dt_stream) run1
Create Date: 10/3/2018, 9:47:38 PM (4 months ago)
Storage: wrangler
Status: stored
Tags: dt_stream, pre_life, run1

2708 total data objects, showing first 100. To view the next 100, run 'bl data query --skip 100'
```

### Downloading a single data-object

Finally, now that you know the data-object ID to download, you can ..

```
$ bl data download -i 5a050ad5d76a2a002737e572
Download dataset to 5a050ad5d76a2a002737e572
100% done

$ ls 5a050ad5d76a2a002737e572
t1.nii.gz

```

## Bulk Downloading

You can bulk download a certain datatype from a selected project with something like a following bash script.

```bash
set -e

project=5b0dad8041711001e958b519
datatype="neuro/tractprofile"

#only need to run this once
bl data query --limit 10000 --project $project --datatype $datatype --json | jq . > all.json

for subject in $(jq -r '.[].meta.subject' all.json | sort -u)
do
    echo "downloading subject:$subject ---------------"
    mkdir -p 2019_tractprofile/$subject/
    ids=$(jq -r '.[] | select(.meta.subject == '\"$subject\"') | ._id' all.json)
    for id in $ids
    do
        echo $id $tags
        tags=$(jq -r '.[] | select(._id=='\"$id\"') | .tags | join(".")' all.json)
        outdir=2019_tractprofile/$subject/$tags
        if [ ! -d $outdir ]; then
            bl data download $id --directory $outdir
        fi
    done
done

```

## Download published data-objects

Brainlife allows users to permanently publish data-objects from a project. You can query published data-objects by first querying the publication ID.

```bash
bl login

bl pub query -q o3d --json | jq .[0].releases
[
  {
    "removed": false,
    "_id": "5bab993aa918ae0027024192",
    "name": "1",
    "create_date": "2018-09-01T00:00:00.000Z"
  },
  {
    "removed": true,
    "_id": "5bb2437c19fdb40027307f7b",
    "name": "",
    "create_date": "2018-10-01T00:00:00.000Z"
  },
  {
    "removed": false,
    "_id": "5c0ff604391ed50032b634d1",
    "name": "2",
    "create_date": "2018-12-11T00:00:00.000Z"
  }
]

```

You can then use the publication release ID to query all data-objects that belongs to this release (and filter by subject) and download each data-objects.


```bash

for id in $(bl data query --limit 200 \
  --pub 5bab993aa918ae0027024192 \
  --subject 0001 \
  --json | jq -r ".[]._id"); do
	bl data download $id
done
```

## Downloading from existing processes.

So far we have described how you can upload / download data from brainlife.io which is stored in Brainlife's project archive. On brainlife.io, you run
Apps inside "process" and data generated there can be "archived" to the brainlife.io's project archive. Sometimes, you want to download files generated
inside a process, which may contain extra files that are not archived in project archive. You might also want to access proceess data as you can specify 
which files / directory to download - rather than the entire `.tar.gz` content from project archive.

The following python script demonstrates how you can query for existing processes / tasks, and download content stored under each tasks. 

```python
#!/usr/bin/python3

import requests
import os
import json

# load the jwt token (run bl login to create this file)
jwt_file = open(os.environ['HOME']+'/.config/brainlife.io/.jwt', mode='r')
jwt = jwt_file.read()

# query datasets records
find = { 
    '_group_id': '851', #see project detail page 
    'service': 'brainlife/app-freesurfer', 
    #'service_branch': '0.0.5',
    'status': 'finished' }
params = { 
    'limit': 500, 
    'select': 'config._inputs.meta', # for subject id
    'find': json.JSONEncoder().encode(find) }
res = requests.get('https://brainlife.io/api/amaretti/task', params=params, headers={'Authorization': 'Bearer '+jwt})

if res.status_code != 200:
    raise Exception("failed to download datasets list:"+res.status_code)

# loop over each task and download "output/stats" directories.
tasks = res.json()["tasks"]
for task in tasks:
    taskid=task["_id"]
    subject=task["config"]["_inputs"][0]["meta"]["subject"]
    print(taskid, subject)
    url = 'https://brainlife.io/api/amaretti/task/download/'+taskid+'/freesurfer/output/stats?at='+jwt
    res = requests.get(url, allow_redirects=True)
    open(subject+'.tar.gz', 'wb').write(res.content)

```





