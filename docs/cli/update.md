!!! warning
    This is a draft. Comments are welcomed!

# Updating Datasets

Brainlife dataset are immutable. Although you can not modify the content of the data itself, you can update its metadata, tags, and description using `bl dataset update` CLI.

To update the dataset, you will need to know the dataset ID. Please see previous tutorials for querying project, dataset. Once you know the ID, you can then run the CLI like so.

```bash

bl dataset update --id <datasetid> --desc "My new description"
bl dataset update --id <datasetid> --subject sub-reset1
bl dataset update --id <datasetid> --session ses-reset1
bl dataset update --id <datasetid> --run run-reset1

```

You can add/remove tags 

```bash
bl dataset update --id <datasetid> --remove_tag oldtag
bl dataset update --id <datasetid> --add_tag newtag
```

You can combine various parameters to update various items at once.

```bash
bl dataest update --id <datasetid> --desc "Updating things" --add_tag newtag --subject sub-123
```

By combining with other CLIs and a bit of bash scripting, you can bulk update tags on multiple datasets. For example, let's say you have a file with a list of subject names to update

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

The following script will then iterate each subjects and set "S500" tag on all datasets with matching subjects.

```bash
#!/bin/bash
for subject in $(cat S500.txt)
do
    #querying all datasets with $subject (you can add other query to be more specific)
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


