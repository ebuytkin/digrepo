---
layout: page
title: AMIP Ingest
parent: Ingest
nav_order: 
---
{: .development }
The following ingest process is under development.

# AMI Preservation Ingest Process
Material classified as AMIP follows the [General Ingest Process]() with minor adjustments listed below. 

## Packaging
### Large Bags
Bags larger than 170GB should be moved to the mount shared by `qu` and `ICA` to be packaged on a machine with available space.

### Trello Card Labelling 
AMIP material can be found on the `qu` and `ICA` servers. Due to the amount of material found in these locations it is important to label the source location when creating a Trello card. Bags located on `ICA` should be labeled `ICA_p` (this includes bags moved to `ICA` due to size). 

## AWS Upload
Succesfully ingested and validated batches should be uploaded to the designated S3 bucket into `DEEP_ARCHIVE` storage, e.g:
```sh
aws s3 sync /path/to/source/batch-name s3://s3-bucket-name/sub-folder/batch-name --profile profile-name --storage-class DEEP_ARCHIVE 
```
Once the upload is complete, move the source batch from the `_INGESTED/to_aws` folder, to the deletion folder.