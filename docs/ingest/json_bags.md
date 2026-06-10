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
