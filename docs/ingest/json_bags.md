---
layout: page
title: JSON Bags
nav_order: 1
parent: Digitized AMI
grand_parent: Ingest
---

## Data Model Description

Each package is ingested to a single SO that represents the digitized media.
The SO contains two child SO, one for metadata derived during digitization and the other for contents recorded from the object.

All media files derived from the object are represented in an IO named `_media`. Potential additional content IOs include `_captions` and `_cue`, depending on the source object.

The metadata IOs include

* json files describing the characteristics of each file from digitization
* qctools files describing the signal characterisitcs
* framemd5 files storing frame-level checksums

## Notes on Ingest

### Missing Service Files

AMI packages may not have service files, particularly audio packages and early video packages.

If a package does not have service files:

1. Consult the current recommendations for service files from AMIP.
2. If service files were generated for another product and are of expected quality, copy those files into the bag.
3. Generate service files from the edit or preservation file as appropriate and store them in a folder titled ServiceCopies within the bag payload.

It is not necessary to update the bag manifest in either case.

### Uncompressed Files

NYPL's policy is to encode all preservation and edit files files with lossless compression whenever possible.

If a package contains uncompressed files:

1. Consult the current lossless compression specifications from AMIP.
2. Validate the bag checksums.
3. Generate lossless compressed versions and framemd5 files from the original files and store them within the bag.
4. Ensure the lossless version is truly lossless by comparing them with the framemd5 data.
5. Delete the uncompressed files.

It is not necessary to update the bag manifest in either case.

### Unclear Regions/Streams/Parts

Occasionally, the sub-object vocabulary of regions, streams, and parts were misapplied.
This is most often the case when an audio package has faces with more than 2 regions.

If a package is found with more than 2 regions:

1. Manually inspect the filenames to get a sense of the plausibility of the specific arrangement.
2. If the sub-objects should be reassigned:
   1. Validate the bag checksums
   2. Update the filenames

### Packages with streams

{: .development }
The following ingest process is under development.

Multi-stream audio objects do not have a clear access format.
Their streams should be hear simultaneously to make sense to a listener, but they also have to be mixed in order to resemble an intended sound.
For now, these are being withheld from ingest until a service file strategy is agreed on.

If a package is found with streams:

1. Move to the folder with other similar packages.

### Packages with parts

{: .development }
The following ingest process is under development.

Prior practice followed a convention of breaking audio playback into segments when the resulting data overran the 2 or 4 GB file size limit of WAVE.
These parts typically also included an overlap, and given the nature of PCM sample, it is possible to recombine the audio stream into a single file.

If a package is found with parts:

1. Create framemd5's of each part.
2. Find the overlap point of the wave files and recombine them as an RF64 Wave.
3. Compare the RF64 to the framemd5's
4. Move the file to the uncompressed file fix

