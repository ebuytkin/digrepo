---
layout: page
title: Validation & Deletion
parent: Ingest
nav_order: 
---
# Post-Ingest Validation and Deletion

This document outlines the full validation and deletion workflow, including post-validation cleanup and automation.

## Validation

Validation verifies that packages have been successfully ingested into Preservica and that the data is not corrupted or missing. Below are the mechanisms used by the validation tool to verify the integrity of the ingested packages, and the post-validation workflows that can be triggered. 

### Core Validation Mechanisms

* **Local File Discovery:** Scans the source directories to map local files, resolve symlinked pathways, and parse local checksum manifests.
* **Remote System Querying:** Queries Preservica via API to retrieve the corresponding ingested file bitstreams, fixity data, and structural metadata.
* **Comparison Criteria:** Cross checks the two systems to determine a successful match, including file presence, exact byte size, and checksum verification.

### Handled Exceptions and Edge Cases

* **Format Transcoding:** Accounts for acceptable file extension and format changes that occurred during the initial lint and/or repair process (e.g., automatically matching uncompressed source files to their resulting lossless preservation formats).
* **Metadata Fuzzy Matching:** Uses fuzzy matching to validate metadata files that may have minor, allowable variations due to technical metadata changes resulting from the aforementioned transcoding.
* **Ignored Artifacts:** Ignores hidden, temporary, or specific structural bag files that are intentionally excluded from the strict one-to-one validation check.

### Post-Validation Actions

* **Validation Tracking:** Records the batch name, validation status, timestamps, and specific failure reasons into a centralized tracking database.
* **Local Package Routing:** The optional automated sorting of the local source directories into designated `valid` and `invalid` staging folders based on the validation outcome.
* **Remote Deletion Queuing:** The optional process for automatically moving packages that failed validation directly into a designated Preservica deletion folder for subsequent cleanup.

### Logging and Reporting

* **Verbose Manifests:** Detailed file-level comparison logs utilized for deep troubleshooting of mismatched packages.
* **Summary Outputs:** Description of the final aggregated summaries detailing total valid packages, invalid packages, as well as the paths to said packages for easy reingest. Also includes a list of any invalid packages in Preservica preservation directory and/or any valid packages in deletion directories.

## Deletion
The following deletion workflow is for packages that have failed ingest validation. 

### Moving Failed Ingests
1. After running the validation tool, if any packages are flagged as invalid, move the source packages to the appropriate folder to repeat the ingest process. These should exist in directories with the same batch name as the original ingest attempt. 
    + Packages within the same batch that pass validation can be moved to the appropriate validation folder for potential deletion.
2. Using the move entity tool with the invalid package titles, move failed ingests to the designated Preservica deletion folder.
    + Create a new deletion folder with the appropriate permissions if needed. This can be done manually within the Preservica interface. 

### Preservica Deletions
1. Once all failed ingests have been moved, manually delete the deletion folder via the Preservica interface. 
2. Notify the secondary-approver of the new deletion workflow to complete the process. 
3. Begin the ingest process for the moved failed packages.

