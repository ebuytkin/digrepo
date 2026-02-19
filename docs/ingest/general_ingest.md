---
layout: page
title: General Ingest Process
parent: Ingest
nav_order: 1
---
## Ingest Process
This document describes the general ingest process for Preservica.
That process is based on the current packaging specifications for each workstream.
Differences for each workstream are documented on their respective page.
Additional remediation is needed to update legacy projects to current packaging specifications before beginning ingest.
Those steps are documented on their respective pages.

### Locate packages
1. Choose a batch of packages to work with. One batch consists of a folder that contains packages of digitized objects, electronic records, or other units of work. A batch should already be located on network storage.
2. Create a Trello ticket to log the work. Name the ticket based on the batch. For example, `326` for a folder of AMI with IDs 326xxx, `M18600` for the electronic records of collection `M18600`, or `NYPL23245-Audio` for the Audio folder from the hard drive labeled NYPL23245.

### Validate readiness for ingest
1. Lint the batch for any inconsistencies or missing files using the appropriate linter, e.g.
    ```sh
    poetry run move_ami_linted_issues --directory /path/to/batch --destination /path/to/_repairs
    ```
2. Problematic bags will be sorted into folders within the destination path based on the first condition failed while linting. For more information on the repair process, see [Bag Repair Tools]().
3. Update the Trello card once the batch is ready for packaging.

### Queue for ingest
1. Run the batch through the packager for the workstream.
2. Monitor the packaging process.
    * The packaging process will halt, if the machine does not have enough storage space to process it.
    Move these packages to packaging run on another machine, and resume the batch.
3. Monitor the upload process. Packages are moved to the ingest staging area after packaging.
4. Update the Trello card once the batch is packaged.
5. Run a new batch when one packaging process completes.
The ingest orchestrator should have at least 3 days worth of completed packages ready at all times.

>{: .note}
>
>Some machines run two packagers simultaneously. The packager `.ini` for these machines has a `PackageSizeMultiplier` variable set to `2` by default. In the event a bag is smaller than the available disc size but is not processing:
>
>1. Pause the other running packager.
>2. Set the `PackageSizeMultiplier` variable to `1`. 
>3. Remove the package ID from the the process list. 
>4. Restart the currently packager without clearning the process list.
>5. Once that package has completed, pause the running packager. 
>6. Reset the `PackageSizeMultiplier` variable to `2`.
>7. Restart the other packager. 

### Monitor ingest
1. Monitor ingest via the Orchestrator script and the dashboard on the Classic interface.
2. Adjust the number of simultaneous workflows as needed via the Orchestrator `.ini` file.
    * Use the maximum number of workflows if no other work is being performed within Preservica.
    The absolute maximum number of workflows is the total number of threads available on the Job Queue servers.
    * Reduce the number of workflows if processing capacity is needed for other projects, such as access representation creation.
3. Note any failed ingests. Examine error message from the sub-process steps on the workflow page.
    * Repair small problems immediately
    * If further help is needed, discuss with other Digital Preservation staff
    * Halt ingest via the Orchestrator `.ini` file if the issue appears to be pervasive.
    * See the [Deletion and Re-Ingest Workflows]() for more details on resolving failures.

    >{: .note}
    >
    >Ingests marked as `Failed` will be caught, recorded and terminated/retried via a google script. This script runs hourly and records the parent and child classic interface workflow links in the assigned spreadsheet. 
    >
    >Ingests marked as `Completed w/ Errors` will need to be manually recorded as needed. 

4. Follow the [Deletion and Re-Ingest Workflows]() to comfirm sucessful ingests. 
5. Update the Trello card once the batch is ingested.

### Validate Ingest
1. Compare the items on network storage to the items ingested to ensure ingest has been attempted on all.
{: .development }
The following steps are under development.
2. Validate the batch with the appropriate validator, e.g.
    ```sh
    validate_ami --directory /path/to/batch
    ```
3. Review remediation steps in [Deletion and Re-Ingest Workflows]() for any invalid ingests.
4. Once the batch is valid, delete the source files
    ```sh
    delete_source --directory /path/to/batch
    ```