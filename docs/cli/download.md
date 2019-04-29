!!! warning
    This is a draft. Comments are welcomed!

# Downloading Datasets

Before downloading a dataset, let's see what datasets are available for you to download.

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

### Querying Datasets

```

$ bl dataset query -p 5a022fc99c0d250055709e9c
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

2708 total datasets, showing first 100. To view the next 100, run 'bl dataset query --skip 100'
```

### Downloading a single dataset

Finally, now that you know the dataset ID to download, you can ..

```
$ bl dataset download -i 5a050ad5d76a2a002737e572
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

#cache the list of datasets that we could download
if [ ! -f all.json ]; then
    bl dataset query --limit 10000 --project $project --datatype $datatype --json > all.json
fi

#enumerate subjects
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
            bl dataset download $id --directory $outdir
        fi
    done
done

```

## Download published datasets

Brainlife allows users to permanently publish datasets from a project. You can query published datasets by first querying the publication ID.

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

You can then use the publication release ID to query all datasets that belongs to this release (and filter by subject) and download each datasets.


```bash

for id in $(bl dataset query --limit 200 \
  --pub 5bab993aa918ae0027024192 \
  --subject 0001 \
  --json | jq -r ".[]._id"); do
	# download the data
	bl dataset download $id
done
```

