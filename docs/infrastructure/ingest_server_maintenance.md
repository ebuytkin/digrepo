---
layout: page
title: Ingest Server Maintenance
parent: Ingest Server Management
nav_order: 1
---

## Clear process files

1. Ensure that all parts of the ingest process on the server are currently stopped.
2. Move the following files to long-term storage
   - `process_list.txt`
   - `completed_containers.txt` and `failed_completed_containers.txt`
   - `uploaded_containers_*.txt`
   - the most recent orchestration database
   -

## Clear down logs

1. Delete logs from test ingest processes.
2. Move the logs from the production ingest processes to long-term storage.

## Check working storage utilization

1. Login to the server and run `df -h path/to/working/storage` or `df -h` if unsure of the working storage path
2. Determine if the server has sufficient free space.
3. To determine which folders contain large amounts of data, investigate their size using `du -sh /path/to/parent/*`. Repeat as necessary.

Discuss how to handle the data, depending on what is found.

1. If there are containers that were not uploaded, determine if they are valid containers
2. If they are valid, requeue for upload
3. If they are not valid, delete and repackage the source material. This is generally the better option.
