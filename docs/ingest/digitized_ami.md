---
layout: page
title: Digitized AMI
has_children: True
parent: Ingest
nav_order: 4
---

Digitized Audio and Moving Image packages are any packages created through the digitization of source audio and moving image media.
They are generally referred to as AMI.
Their content is notable compared to other packages for:

* the size of each package, and
* the potential complexity of files derived from a single object.

## Package Requirements

There are two general types of packages:

* JSON Bags - the current specification where each package contains the files from a single object.
Metadata is stored in JSON files.
* Excel Bags - the former specification where each package contained the files from all objects in a digitization project.
Metadata is stored in Excel files.

The two types are largely similar in structure, file naming conventions, and other aspects.
However, the Excel bags are generally much larger in size and the metadata is more complex to access programmatically.
As part of ingest, all Excel bags will be converted to the JSON bag style.

There are additional variations and one-offs in packaging that must also be converted.

### JSON Bag Package Requirements

1. The top-level folder must be named according to the AMI ID assigned to the physical object, `123456`
2. The bag payload must contain folders for `PreservationMasters` and `ServiceCopies`. Additional folders for `EditMasters`, `Mezzanines`, and `Images` are not required.
3. Every media file must have a JSON sidecar.
4. Additional digitization logs and metadata may be stored in a `tags` folder at the root of the bag.

## Process

Digitized AMI packages are added to the ingest queue once approved by the AMI Preservation team.

Remediation work is performed by the Digital Preservation team in collaboration with the AMI Preservation team.
