---
layout: docs
section: next
type: spec
title: Data migration
---

## Overview

A lot of users have already imported and modelled budget datasets in the current version of OpenSpending.org. These users rely on this data being available and of course the datasets cannot be removed from the system as OpenSpending swaps out the datastore (built on flat files). Hence the data needs to be migrated from the current version of OpenSpending to the [new Datastore](http://community.openspending.org/next/01-datastore/).

In the current version of OpenSpending.org, budget datasets are stored in what's called an [OLAP cube](https://en.wikipedia.org/wiki/OLAP_cube), which is really great for processing the data and understanding it. However the specific structure requires transformation or remodelling of the budget data which can lose information/data of the source files or make it harder to process the data in the OLAP cube in other ways.

The new datastore (the flat file based one) will use [OpenSpending Data Package](http://labs.openspending.org/osep/osep-04.html). The collected data will not, so the data will have to be converted/transformed to the new format. The OpenSpending Data Package does not modify the data itself, it builds upon the budget resource and just adds metadata in a separate file that points to the resource.

For that reason we will try to grab the original source data first and use the existing model to convert as much of it as we can, and if any data remains, try to deduce how to use it. When the original source data is no longer accessible we will create the budget resource file based on the data in the OLAP cube.

## Rough workflow

1. Prepare the **OpenSpending current codebase** for export
    * Allow download of all datasets
        * Decide if we should include private datasets as well
        * This can use the public API
        * This could also be run on the production machine like [SpenDB's export script](https://github.com/pudo/spendb/blob/master/contrib/os_export/export.py)
    * Expose all sources for the dataset
        * Decide whether to include only sources with successful runs
    * Expose all team members for the dataset
        * The new datastore will be built around usernames
        * Decide what to do regarding multiple team members
    * Expose OpenSpending models for datasets
        * Decide what to do with datasets without models (incomplete)
2. Create a **script** to download all datasets, source files and models
    * Either fetching via API or run on the production server
        * Re-usable by others if we run via API
        * Running on production server requires access
    * Store locally to allow iterations of next steps
        * Decide if this dump should be exposed to others to allow for replication
3. Create a **script** to convert to OSDP
    * Take the OpenSpending export data dump and convert to OSDP
        * Will definitely need some magic around the mapping of dimensions
4. Create a **command line** application to upload OSDPs
    * Store OSDPs in S3
        * Prefix for dataset files /username/dataset_id/
    * Write a master registry file
        * This could be a DataPackage itself in a master location on S3?
    * This commandline should also run a validation of the OSDP
        * Just in case something breaks
        * Also make this script reusable for a future microservice to build upon

## Implementation

Work has just started and no implementation currently exists. You still have a chance to participate and help out!