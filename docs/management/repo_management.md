---
layout: page
title: Preservica System Management
nav_order: 1
parent: Management
---

## System Upgrade Downtime Procedures
During Preservica system upgrades, maintenance periods, or other periods of ingest downtime, there are several tasks that should be completed: 

### Clearing out transient folders
Failed ingests are on occasion not deleted from the transient folders within Preservica. These should be deleted during downtime to free up space and to prevent workflow issues: 
1. Run the cleanup tool `clear_transient_folders` paying attention to use the appropriate arguments for `type` and `tenant`. Run the help flag for 

### Clearing out AWS S3 stub files
Similarly, AWS S3 buckets containing stubfiles may need to be cleared out: 
1. Use the AWS cli to locate any old stubfiles. Good practice is to delete files older than 30 days, but this timeframe is up to the discretion of Digital Preservation Staff and the current packaged backlog. 
2. Run the following command to create an executable to remove stubfiles located in s3 buckets based on the date specified. Replace "[ISO 8601 Date Format]" with the date of the files to be deleted (e.g., 2024-12-31), or any alternate pattern that targets the appropriate files for deletion. 
```
aws s3 ls s3://path/to/stubfiles/ --profile [aws-profile] | grep "[ISO 8601 Date Format]" | awk '{print "aws s3 --profile [aws-profile] rm s3://path/to/stubfiles/" $4}' > ~/s3_rm.sh
```
3. Run the executable.
```
sh ~/s3_rm.sh
``` 

### Clearing out Isilon folders
Containers found on NYPL packaging servers that failed to ingest or enter final deletion may also need to be cleared out. This is a manual process and is up to the discretion of Digital Preservation Staff and the current packaged backlog. Similar to AWS stubfiles, good practice is to not delete containers unless they are older than 30 days. 

{: .note }
This must be done for each packaging server. 

## Other Maintenance
### Finding duplicates
On occasion, duplicate packages may be found in Preservica. To avoid issues when running validation workflows, these should be investigated and corrected. 
1. Run the duplicate finder tool `find_duplicates` paying attention to use the appropriate argument for `parent` and the `move` flag if not running in dry-run mode. It is also recommended to run this tool with the `compare` flag to ensure the more complete instance of the package is kept. Run the help flag for other required and optional arguments. 