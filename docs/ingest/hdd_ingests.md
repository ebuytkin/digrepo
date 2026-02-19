---
layout: page
title: External Drive Ingest Workflow
nav_order: 2
parent: Ingest
---
# External Drive Ingest Process
This process transfers and ingests content from external drives into Preservica.
{: .note }
HDDs should have one or both of the following directories (names may vary): Film and/or Audio. These directories can be processed on different mounts if only one contains bags too large for the `qu` server. 

## Transfer drive contents to server
1. Determine the size of the bags to be ingested, e.g.:
```sh
du -sh /path/to/mounted/HDD/Video/or/Audio/* | sort -rh
```
2. Run a rsync command to the appropriate destination path.  
    + If all bags are smaller than 180GB, copy contents to the `0_copied_not_linted` directory on the `qu` server, e.g.:
    ```sh
    rsync -aP /path/to/mounted/HDD/Video/or/Audio /path/to/destination/1_copied_not_linted
    ```
    + If the HDD contains bags are larger than 170GB, copy contents to mountpoint shared by a server large enough to package these bags in the `0_linting` directory, e.g.:
    ```sh
    rsync -aP /path/to/mounted/HDD/Video/or/Audio /path/to/destination/0_linting
    ```
    
## Linting
Once copied to appropriate location, run `move_linted_ami` from `prsv_tools` to sort out invalid bags into `1_needs_repairs` (if on `qu`), e.g.:
```sh
poetry run move_ami_linted_issues --directory /path/to/copied/HDD/ --destination /path/to/1_needs_repairs
```

## Packaging
If working with a HDD on the appropriate `qu` server, package bags on the the `03` server. Otherwise, package bags on a size appropriate simplivity machine. 

## Post-ingest tasks
After the bags are ingested, run `compare_sources` from `repair_tools` on each directory in the HDD to sort out bags that need to be reingested, e.g.:
```sh
poetry run compare_sources --credentials creds --prsvheck --source /path/to/HDD/Film/or/Audio --movedir /path/to/_reingest --deletion-parent-ref ref
```
Follow reingest workflow for items not successfully ingested.

## Trello card process
1. At the start of this process, create a new trello card with the name of the HDD.
    + If separating the Video/Audio directories, name the card accordingly, e.g.:
    `HDD1234_Video`
2. As the contents of the HDD progress through the ingest process, move this card to the appropriate column in order to accurately track its progress.