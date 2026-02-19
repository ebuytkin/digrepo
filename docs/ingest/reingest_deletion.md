---
layout: page
title: Deletion and Re-Ingest Workflow
nav_order: 2
parent: Ingest
---
# Deletion & Re-Ingest Workflows
This process identifies packages that have not been ingested into Preservica, sorts them for re-ingest, and deletes the original failed ingests if needed. 

## Moving & Deleting Failed Ingests
### Finding & Moving Failed Ingests
`compare_sources.py` is used to identify packages in a designated source path that have not been successfully ingested into Preservica and move them to the appropriate folder for reingest or validation. Using the `--deletion-parent-ref` argument will move failed Preservica ingests the assigned deletion folder. 
    
Required and optional arguments and flags: 
+ `--credentials`: appropriate Preservica credentials for API use.
+ `--prsvcheck`: used to compare bags located in the source directory against Preservica's contents only.
+ `--source`: path to top level directory containing bags to be audited.
+ `--movedir` or `--copydir`: path to location to move/copy packages to (default behavior is to move failed ingests)
+ `--mvingested`: flag to move successfully ingested bags rather than failed ingests. 
+ `--logpath`: path to save log file at.
+ `--deletion-parent-ref`: [optional] UUID of deletion folder within Preservica. 
+ `--srcindex`: [optional] if not index path is provided, indexes the source path for faster runs in the future.
```sh
poetry run compare_sources --credentials creds --prsvcheck --logpath /path/to/log/dir --deletion-parent-ref ref --movedir /path/to/reingest/or/validation/dir/ --source /path/to/source
```

### Manual Preservica Deletion Moves
Options for manually moving files for deletion within Preservica include:
+ Using the Preservica Explorer to drag and drop files into the appropriate deletion folder.
+ Using the Preservica API Documention interface. 
+ Running `prsv_move.py` as outlined below.
1. In Preservica, create a new folder within the `ToDelete` directory.
    + Use the naming convention `YYYY_MM_DD_reason_for_deletion`
        > e.g. `2025_10_09_failed_ingests`
    + Record the entity ref (UUID) of this folder.
2. Run `prsv_move` to move the failed ingests to the deletion folder that was created.
    Required and optional flags:
    + `--credentials`: appropriate Preservica credentials for API use.
    + `--pkgtitle`: ID of single package to move.
    + `--new-parent-ref`: UUID of deletion folder within Preservica. 
    + `--parent`: [optional] parent folder to search for object in, choice of `ingest`, `digami`, or `digarch` (default behavior checks all locations)
    + `--logpath`: path to save log file at.
    ```sh
    poetry run prsv_move --credentials creds --parent digami --new-parent-ref uuid-of-deletion-folder --use-file
    ```
3. Once the move process is complete, start a deletion workflow in Preservica for the entire folder created in step 1.

### Deletion Folder Creation and Removal
1. In Preservica, create a new folder within the `ToDelete` directory.
    + Use the naming convention `YYYY_MM_DD_reason_for_deletion`
        > e.g. `2025_10_09_failed_ingests`
2. Record the entity ref (UUID) of this folder for use in `compare_sources.py` and `prsv_move.py`.
3. Once all packages needing to be deleted have been moved to this folder, navigate to the `ToDelete` folder within the Preservica Explorer. Right-click on the deletion folder and select `Delete Folder`.
4. Notify the secondary-approver of the new deletion workflow to complete the process. 

## Linting & Ingest
1.  Run `move_linted_issues` on the `_reingest` folder to ensure all bags are still valid.
```sh
poetry run move_linted_issues --directory /path/to/_reingest/ --destination /path/to/repairs/folder
``` 
2. Begin the packaging and ingest process for the `_reingest` folder.

## Cleanup
After the ingest is complete, run `compare_sources` again to clean up the `_reingest` folder. Repeating this process as necessary.