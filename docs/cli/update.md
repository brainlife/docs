!!! warning
    This is a draft. Comments are welcomed!

# Updating data objects

Brainlife data-objects are immutable. Although you can not modify the content of the data, you can still update the metadata, tags, and description using `bl data update` CLI.

```bash

bl data update --id <dataid> --desc "My new description"
bl data update --id <dataid> --subject sub-reset1
bl data update --id <dataid> --session ses-reset1
bl data update --id <dataid> --run run-reset1

```

To find the data object ID, please see the previous tutorials for querying data objects.

You can easily add/remove tags like so.

```bash
bl data update --id <dataid> --remove_tag oldtag
bl data update --id <dataid> --add_tag newtag
```

You can combine various update requests.

```bash
bl dataest update --id <dataid> --desc "Updating things" --add_tag newtag --subject sub-123
```

Some users might be more familiar with the terminology "sidecar". On BIDS, what we call "metadata" is stored in a separate .json file called "sidecar". In sidecar, you can store any number of key/values pairs associated with the actual data (dwi, fmri/bold, eeg/fif, etc). You can upload the whole sidecar information to brainlife by using `-m <filepath to .json>` option like..

```bash
bl data update --id <dataid> -m /path/to/sidecar.json
```


## Bulk Update Tags

By combining with other CLIs and a bit of bash scripting, you can bulk update tags on multiple datas. 

Let's say you have a file with a list of subject names to update.

[S500.txt]

```
100307
100408
101006
101107
101309
101410
101915
102008
102311
102816
..
```

The following script will then iterate through this list and set "S500" tag on all datas with matching subjects.

```bash
#!/bin/bash
for subject in $(cat S500.txt)
do
    echo "finding subject:$subject"
    bl data query --project 59a09bbab47c0c0027ad7046 --subject $subject --json > list.json
    jq -r ".[]._id" list.json > ids.txt

    #iterate each datas for this subject and add some tags
    for id in $(cat ids.txt)
    do
        echo "  updating $id"
        bl data update --id $id --add_tag S500 &
    done

    wait
done

```

If you want to update tags on specific objects, you can update the `bl data query` parameters (on datatype, existing tags, etc..) to be more selective of which objects to update.

## Updating sidecar/metadata

You can update specific sidecar/medata fields by doing something like the following.

```
cat > append.json <<EOF
{
    "RepetitionTime": 1.970
}
EOF
bl data update --id 600f50b551942b71ad3d16e7 --meta append.json
```

`--meta` option will read specified JSON file and update key/value specified in the append.json in this sample. 

!!!note 
    At the moment, there is no way to *remove* an existing field. You could set it to `null` value, or you will have to edit it via brainlife UI.


