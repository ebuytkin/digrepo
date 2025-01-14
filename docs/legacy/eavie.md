---
layout: page
title: Early Access Viewer
nav_order: 4
parent: Legacy Infrastructure
---

The Early Access Viewer (EAVie) is a product built to provide access to digitized AMI that had not been run through Media Ingest.
It uses separate infrastructure and processes from the image repository.

## EAVie Requirements

Ingest to EAVie requires:

1. a SPEC inventory record
2. a video service file or audio edit file
3. a JSON sidecar for the media file

## Architecture Components to Migrate

There are no direct relationships between EAVie and Preservica processes at this time.

## Files and Metadata to Migrate

All files within EAVie are derivatives of existing AMI packages.
There is no need to migrate data from EAVie to Preservica.
