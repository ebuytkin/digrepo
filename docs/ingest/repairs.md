---
layout: page
title: Bag Repair Tools
parent: Ingest
nav_order: 
---
# Invalid Bag Repair Tools
For bags that do not pass linting and are sorted into the appropriate repair folder:
* Repair small problems immediately, e.g. deleting an empty folder
* More complex problems, or batches with a high count of invalid bags can utilize the repair_tools project. 
* If further help is needed, contact other Digital Preservation staff.

## Missing Service Copies
For bags missing service copy files or the entire folder, use the `download_sc_mp4` tool:
1. Using the CLI, log into your AWS account, e.g:
```sh
aws sso login —-profile profile-name
```  
2. Run `download_sc_mp4` with the appropriate arguments. This tool checks the default S3 bucket (unless specified with the `--bucket` argument) for existing service copy files. If matching files exists, they are downloaded to the `ServiceCopies` folder. If none are found, service copies will be transcoded from the existing preservation files. Basic usage of this tool looks like:
```sh
poetry run download_sc_mp4 --profile profile-name --directory /path/to/create_scs/123
```
> {: .note}
> Transcoding of `.iso` preservation files is currently under construction and has been turned off via the `ENABLE_ISO_TRANSCODE` variable. S3 downloads for matching service copies is still active. 
3. Bags that were successfully processed (ie. all preservation files have a matching service copy) will be moved to a new folder, `_fixed`, within the same repair folder, eg. `/path/to/create_scs/_fixed/123/12345`. If the directory path that was passed to `download_sc_mp4` is empty once all the bags have been processed, it will be deleted. 

## Hidden Files
To avoid unecessary system files created during bag processing being ingested into Preservica, run `delete_hidden_files` on any bag sorted into the `has_hidden_files` repair folder, e.g.:
```sh
poetry run delete_hidden_files --directory  /path/to/has_hidden_files/123
```
{: .warning}
This tool will delete *all* hidden files found. If the bags being processed have necessary hidden files please process those manually. 

# 
{: .development }
The following repair processes are under development.

## Parts in Media Files
## Uncompressed Media Files
## Invalid Bag Strucutre
## Streams in Media Files
## Quick Fixes / One-Offs

# Post-Repair Process
