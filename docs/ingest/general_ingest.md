---
layout: page
title: General Ingest Processes
parent: Ingest
nav_order: 
---
# Preservica Ingest Processes

This document outlines the standard ingest workflows for ingesting materials into Preservica, including the general pipeline and specific adjustments for AMI Preservation (AMIP) and external drive transfers. 

## 1. Locating & Preparing Packages
* Choose a batch of packages located on network storage to work with.
* Create a Trello ticket and name it based on the specific batch.
* Be sure to include the batch source location labels on the Trello card to make later validation easier.

## 2. External Drive Transfers
* External drives with json bags typically contain separated directories for different media types (e.g. Audio, Film, Data). These directories can be processed independently on different servers if space limitations require it.
* Check the size of all packages to ensure they are copied to the appropriate staging server.
* Using `rsync -aP`, copy the drive contents to the server. Exceptionally large packages should be directed to a mount point capable of accommodating their size during the packaging phase.


{: .note }
The following is for *new* drives received from AMIP only.
* Use the `copy_to_s3.py` tool using the `--check_and_upload` flag to copy service files from the drive directl to the appropriate AWS bucket/remote source. 

## 3. Linting & Repairs
* Run the appropriate linter on the batch to check for packages needing repairs.
* Problematic or invalid packages will be moved to a repair directory. Depending on the linter used, these packages may be moved automatically or may require manual removal.
* Quick repairs can be handled immediately or in larger batches. More complex repairs may require consultation with other staff. For more information on repairs, see [Bag Repair Tools]()
* Update the Trello card once the batch is fully linted and ready for packaging.

## 4. Queueing & Packaging
* Run the batch through the appropriate packager.
* Monitor the packaging process closely. 
* If the processing machine lacks sufficient storage space, the process will halt. Move these packages to another machine and resume the batch.
* Monitor the upload process as packages are moved to the Preservica ingest staging area after packaging.
* Ensure a steady stream of work is available by starting a new batch when one completes.
* If machines running multiple simultaneous packagers stalls on specific larger packages, temporarily adjust the packager's capacity configuration variables to allow the package through.
* Update the Trello card once the batch is fully packaged.

## 5. Monitoring Preservica Ingest
* Monitor the ingest progress via the orchestrator and the Preservica process monitor.
* Adjust the number of simultaneous workflows via the orchestrator INI file based on available server threads and whether processing capacity is needed for other Preservica-based projects.
* Note any failed ingests by examining error messages generated during the workflow steps. 
    - Automated scripts will catch and record complete ingest failures, however, details log messages will need to be pulled manually.
* Repair minor issues immediately. 
* Stop the ingest orchestrator if an issue appears to be pervasive.

## 6. Post-Ingest Validation and Cleanup
* Compare the items on network storage to the items ingested to ensure ingest has been attempted on all packages.
* Run the validation tool against the source directories to verify ingest was successful. For more information on the validation tool, see [Validation & Deletion](../)
* Follow the standard re-ingest workflows to correct any failed ingests.
* For AMIP materials, successfully ingested and validated batches must be uploaded to designated cloud storage using the deep archive storage classes.
* Once the entire batch is valid and any required cloud uploads are complete, the source files can be deleted.
* Update the Trello card once the entire batch has been successfully ingested.