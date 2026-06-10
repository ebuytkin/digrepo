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
* If further help is needed, contact Digital Preservation staff.

## Missing Service Files

Packages may occasionally be missing service files or the entire service copy directory.

If a package is missing service files:
1. Run `download_sc_mp4` against the target directories. This tool checks the remote repository for existing service files and downloads them into the package.
2. If no remote files are found, the tool will attempt to automatically transcode new service files from the existing preservation media.

    {: .note } Automated transcoding may be restricted for certain preservation formats (such as disk images), though remote downloads remain active.

## Hidden Files, Empty Folders, and 0-byte Files

To prevent ingest of unnecessary files generated during processing, clean up the package.

1. Run the appropriate removal tools on the target directories:
    - `delete_hidden_files.py`
    - `delete_empty_folders.py`
    - `delete_0byte.py`

2. Review the flagged files and folders prior to deletion to ensure no critical data is lost.

    {: .note } Essential package structure directories are protected and will not be removed (e.g., EditMasters, PreservationMasters, ServiceCopies).

## Uncompressed Media Files
NYPL's policy is to encode all preservation and edit files files with lossless compression whenever possible.

If a package contains uncompressed files:

1. Validate the bag checksums.
2. Run the `transcode_media` on the target directories. This tool will locate uncompressed files and generate their lossless compressed counterparts.
3. Ensure the lossless version is truly lossless by comparing them with the framemd5 data.


## Streams and Regions in Media Files
Occasionally, the sub-object vocabulary of regions and streams were misapplied.

This is most often the case when an audio package has faces with more than 2 regions.

Packages with multi-stream audio or files marked as regions require contextual evaluation.

1. Process these packages manually.
2. Review the package to determine if the streams and regions are appropriate for ingest, or if further work is needed.

# Post-Repair Process

Proceed with standard post-repair ingest procedures once all necessary structural and file-level fixes have been made.

---

{: .development }
The following repair processes are under development.

## Parts in Media Files
Prior practice followed a convention of breaking audio playback into segments when the resulting data overran the 2 or 4 GB file size limit of WAVE.

These parts typically also included an overlap, and given the nature of PCM sample, it is possible to recombine the audio stream into a single file.

If a package is found with parts:

1. Create framemd5's of each part.
2. Find the overlap point of the wave files and recombine them as an RF64 Wave.
3. Compare the RF64 to the framemd5's
4. Move the file to the uncompressed file fix

Packages containing media files split into parts should currently be sorted out for future inspection and repair workflows.

## Invalid Bag Structure
There may be cases where a package's structure is incomplete or otherwise invalid. These will need to be manually reviewed and corrected. The following tools may be helpful in repairing structurally invalid bags:
* `create_bag_structure`: May be helpful in rebuilding the bag structure, but will not automatically move files into the correct directories or create the necessary metadata/manfiest files. 
* `create_manifest`: Will create a new manifest file with md5 checksums if the `--md5` flag is used.
* `correct_double_bag`: Will repair bags with extra levels of bag nesting (e.g., `/data/data/ServiceCopies`) 

## Excel Bags
Older excel bags need to be converted into JSON bags prior to ingest. This process is currently under review. 