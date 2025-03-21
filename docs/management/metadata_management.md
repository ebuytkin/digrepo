---
layout: page
title: Metadata Management
parent: Management
nav_order: 0
---

The digital repository program uses Preservica metadata fields to store identifiers that connect Preservica objects to records in other systems.
These fields are managed by XML documents created by digital repository staff, including:

- barcodes
- division codes
- catalog IDs
- finding aid IDs

Staff have grouped these fields into semantic portions called metadata fragments.

The program maintains copies of the XML schemas in a private GitHub repository,
[NYPL/prsv-schemas](https://github.com/NYPL/prsv-schemas).

## Preservica Metadata Documents

### XML Schemas

XSD is used to define the fields, names, and hierarchy of metadata used.
Each metadata fragment has a corresponding XSD.

### XML Transforms

XSLT is used to control the presentation of metadata fields in the Preservica interface.
Each metadata fragment has 2 XSLT documents, one for the viewer mode and editor mode each.

### XML Documents

XML documents are used to define additional UI customizations within Preservica.
Template XML Documents control how additional metadata fragments are added to an entity.
Indexer documents create custom search indexers for metadata fields.

## Schemas management instructions

1. At the beginning of every fiscal quarter, run [get_schemas.py](https://github.com/NYPL/prsv-tools/blob/main/src/prsv_tools/manage/get_schemas.py) to download all three types of metadata schema documents from Preservica.
2. Add and commit the new files to the private GitHub repository.
