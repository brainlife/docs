!!! warning
    This is a draft. Comments are welcomed!

# Updating Datasets

Brainlife datasets are immutable. Although you can not modify the content of the data, you can still update the metadata, tags, and description using `bl dataset update` CLI.

```bash

bl dataset update --id <datasetid> --desc "My new description"
bl dataset update --id <datasetid> --subject sub-reset1
bl dataset update --id <datasetid> --session ses-reset1
bl dataset update --id <datasetid> --run run-reset1

```

To update the dataset, you will need to know the dataset ID. Please see the previous tutorials for querying project ID, dataset ID, etc.

You can easly add/remove tags like so.

```bash
bl dataset update --id <datasetid> --remove_tag oldtag
bl dataset update --id <datasetid> --add_tag newtag
```

You can combine various parameters.

```bash
bl dataest update --id <datasetid> --desc "Updating things" --add_tag newtag --subject sub-123
```

## Bulk Update Tags

By combining with other CLIs and a bit of bash scripting, you can bulk update tags on multiple datasets. 

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

The following script will then iterate through this list and set "S500" tag on all datasets with matching subjects.

```bash
#!/bin/bash
for subject in $(cat S500.txt)
do
    echo "finding subject:$subject"
    bl dataset query --project 59a09bbab47c0c0027ad7046 --subject $subject --json > list.json
    jq -r ".[]._id" list.json > ids.txt

    #iterate each datasets for this subject and add some tags
    for id in $(cat ids.txt)
    do
        echo "  updating $id"
        bl dataset update --id $id --add_tag S500 &
    done

    wait
done

```

If you want to udpate tags on more specific dataset (datatype, existing tags, etc..) you can update the `bl dataset query` parameters.


