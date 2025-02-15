!!! warning
    This is a draft. Comments are welcomed!

# Updating Datasets

Brainlife data-objects are immutable. Although you can not modify the content of the data, you can still update the metadata, tags, and description using `bl data update` CLI.

```bash

bl data update --id <dataid> --desc "My new description"
bl data update --id <dataid> --subject sub-reset1
bl data update --id <dataid> --session ses-reset1
bl data update --id <dataid> --run run-reset1

```

To update the data, you will need to know the data ID. Please see the previous tutorials for querying project ID, data ID, etc.

You can easly add/remove tags like so.

```bash
bl data update --id <dataid> --remove_tag oldtag
bl data update --id <dataid> --add_tag newtag
```

You can combine various parameters.

```bash
bl dataest update --id <dataid> --desc "Updating things" --add_tag newtag --subject sub-123
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

If you want to udpate tags on more specific data (datatype, existing tags, etc..) you can update the `bl data query` parameters.


