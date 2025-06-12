---
layout: page
title: Metadata Reporting Workflow
nav_order: 0
parent: Reporting
---

# Metadata Reporting Workflow

This process utilizes an API call to Preservica to initiate the download of metadata information from specific or groups of packages already ingested. 

## Identify search parameters

Identify parameters for packages to be retrieved: 
+ Date Range
    + Start and end dates
    + Daily
+ Package ID
    + Dig Arch Collection ID
        >e.g. `M12345`
    + Dig Arch ER Package ID
        >e.g. `M12345_ER_1`
    + AMI Package ID
        >e.g. `123` (first three digits of ID)
## Run export workflow
1. Run export_metadata_only with the appropriate credentials and parameters, e.g.:
+ By date:
```sh
poetry run export_metadata_only --credentials creds --ami_ingest_start_date 2025-01-13 --ami_ingest_end_date 2025-02-24 --path ~
```
+ By package ID:
```sh
poetry run export_metadata_only --credentials creds --amipackage_id 123 --path ~
```
+ Identify the directory where the directory structure is located or will be built with the `--path` flag.
2. Failed downloads will be recorded in a separate folder, `/containers/failed_downloads/`.
    + To re-run the export_metadata_only process on the failed AMI uuids, use the --ami_rety argument, e.g.:
        
        {: .note }

        Please note the `--ami_retry` step is only for AMI reports, not DigArch reports. 
        
```sh
poetry run export_metadata_only --credentials creds --ami_retry
```
3. All successful exports will be downloaded as .zip files to `/containers/metadata_exports/`.

## Unpack zipped reports
Run unpack_reports to extract the .zip file to `/containers/Export_Target/`, e.g.:
```sh
poetry run unpack_reports
```
## Compile reports
Once all reports have been unpacked, run compile_reports. The final report will be downloaded to `/containers/combined_reports/`, e.g.:
```sh
poetry run compile_reports --path ~ -dr YYYYMMDD_YYYYMMDD
```
1. As with the export_metadata_only process, identify the directory structure used.
2. Include the date range `-dr` that the reports were run for. Format is startdate_enddate, YYYYMMDD_YYYYMMDD.

## Update the Reporting Database
Using the compiled report, run report_to_db. 
```sh
poetry run report_to_db --path ~ --ami (or --digarch)
```
1. Identify the directory structure used.
2. Run the process with the approriate flag for the type of reports that were run, `--ami` for AMI and `--digarch` for DigArch reports. 
