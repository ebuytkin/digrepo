---
layout: page
title: Digitized AMI
nav_order: 3
parent: Legacy Infrastructure
---

Digitized audio and moving image packages were initially ingested to set of systems related to, but often a parallel to, the image repository.
These systems may be referred to as Media Ingest, although that term is also used for a specific set of ingest processes built for AMI and may not include databases and storage systems that supported it.

Media Ingest ran from 2016 until 2022.

## Media Ingest Requirements

Media Ingest package requirements were more strict than the specifications for AMI digitization.
For example, because the capture data model used by MMS can only model one-to-one relationships between file derivatives, packages such as DVDs or complex audio reels could not be processed.

Media Ingest was capable of handling video Excel bags and most categories of JSON bags.

## Architecture Components to Migrate

The components of the legacy digitized AMI ingest structure that have been replaced by Preservica are the FileWatcher, JSONBagParser, FilestorePusher, S3Uploader, Elastic Transcoder, Ingest Reporter, and Rabbit MQ.
AMI Filestore will not be retired until the files it tracks have been migrated to Preservica.

![alt text](https://nypl.github.io/repo-docs/img/media-ingest.png "Diagram of the media ingest infrastructure from repo-docs repository, https://nypl.github.io/repo-docs/media-ingest.html")

## Files and Metadata to Migrate

### Reversing Media Ingest

Because Media Ingest consumed a subset of the standard AMI packages, the simplest strategy to migrate these files to Preservica is to reform the original packages and run them through standard Preservica ingest.

### Service Files

The Media Ingest process transcoded and placed service files in an AWS bucket for streaming.
These files are not tracked by AMI Filestore, but they can be associated to their source using IDs in AMI Filestore.

Some of the Media Ingest service files have been double-transcoded and have visual quality losses. They should not be migrated.

The bucket also contains service files previously generated on Brightcove and then migrated to S3.
There have been documented transcoding errors with Brightcove files and they should not be migrated.

It may be simpler and more consistent to regenerate service files from preservation files rather than migrating the files in the bucket.

### Ignoring Access-only Ingests

Because Media Ingest was the only method to create the MMS records necessary for publication on Digital Collections, it was occasionally used to run non-preservation workloads to support access initiatives.
These files should not be migrated.
