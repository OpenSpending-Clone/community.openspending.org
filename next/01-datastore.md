---
layout: docs
section: next
type: spec
title: Datastore
---

## Overview

OpenSpending Next has a flat file datastore at its core. The flat file store contains the "master copy" of all data, plus any other flat file representations. Other services may pull data out of this datastore in order to create other representations of the data, for example in a relational database, or in a search index.

All data added by users to OpenSpending is stored in Data Package format - more specifically, [OpenSpending Data Package](http://labs.openspending.org/osep/osep-04.html). Data Package is a format for the delivery, installation and management of datasets. In OpenSpending Data Package, data resources are stored as CSV files, with additional metadata provided via the `datapackage.json` descriptor.

## Implementation

S3 bucket, publicly readable, with data packages scoped by username.
