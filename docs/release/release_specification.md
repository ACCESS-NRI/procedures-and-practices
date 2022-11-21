---
title: Release Specification - Draft
---

# Introduction

Release specification describes what constitutes a release. There may be a range of release types. In the first instance this document describes a "Flagship Release" but can be updated to include other release types.

# Flagship Release

A Flagship Release is a release package for a major model, e.g. ACCESS-CM. The release package will consist of:

-   A model configuration

-   Experiments

See below for details on the different elements of a release package

## 

## Model Configuration

Each model configuration contains

-   A number of model components and coupling components
-   Technical documentation describing how the model components and coupling components are combined into a model

Each component contains

-   Source code
-   Technical documentation of the model
-   Input (ancillary) files required to run the model, e.g. topography, vegetation types, initial conditions

All source code and documentation will be publicly available and released as open source where licensing permits.

Attributes of a model:

-   Version controlled software
-   Reproducible build with full build provenance
## 

All input files will:

-   Meet FAIR standards
-   Where applicable code to recreate the data will be well documented, open source and publicly available

## Experiment

An experiment is a specific realisation of a model configuration supported by the NRI. It has the following attributes:

-   Available to the community, publicly available and open source where licensing allows
-   Versioned
-   Reproducible
-   Supported on NCI hardware
-   Clearly articulated support timeline
-   Restart and output data products available to the community
-   Data products meet FAIR standards
-   Includes documentation:
    -   characterising model (as described in previous section)
    -   instructions on how to run the model
    -   description of how the model was run *for this experiment*
    -   instructions for accessing data products and example analyses

The NRI will commit to a support timeline for each experiment. The details of that support timeline will vary depending on community needs.

## What does support entail?

Support timelines are tied to the supported data products. Data products have a long research life and impact. Data products are not expected to change unless bugs in the model code or errors in the configuration are discovered. Configuration changes that lead to significant changes in model output will trigger a new run of the model and an updated version of the data product.

Support falls into four categories: active support, maintenance support, unsupported and archived.

Support windows are time based on the release date, and the period of time that each support level applies.

Support windows are important. They clearly communicate to the community what the NRI will do to support each release. It may be possible to revise support windows after release on a release-by-release basis, if the community thinks they are of sufficient importance and the NRI has the resources to do so.

**Support level summary: Flagship Release**

| **Level**   | **Window**         | **Description** |
|-------------|--------------------|-----------------|
| Active      | 0-2 years          | Experiments actively supported |
|             |                    | Bugs in model components fixed |
|             |                    | Errors or omissions in experiment configuration fixed |
|             |                    | New versions of data products generated as required |
|             |                    | Active data curation: errors remedied, indexes updated as required |
|             |                    | Documentation updated and errors fixed |
|             |                    | Requests for extending documentation will be assessed and done if resources are available |
| Maintenance | 2-4 years          | Data no longer updated |
|             |                    | Bugs in model components will be fixed if relevant to current NRI development activities |
|             |                    | Errors in experiment configuration, or bugs in model components will be characterised and assessed, and meta-data for affected data products updated. In extreme cases data products may be withdrawn. |
|             |                    | User documentation no longer updated. Community can continue to generate their own documentation. Some documentation products could transition to community support. |
| Unsupported | 4-6 years          | Data and documentation no longer updated by ACCESS-NRI |
|             |                    | Data still published and available on disc at NCI |
| Archived    | 6+ years           | Data available on tape archive |
|             |                    | Retrieval is possible, but will require a request which will be assessed based on the size of the data and complexity |
| Deleted     | TBD (min 7+ years) | Archival is not indefinite. Once archive window exceeded data **may** be deleted, but more likely just not transferred when archive hardware renewed |


For a specified period after release the model and associated experiments are actively supported. During this period bugs in the model component code will be investigated and fixed. Errors or omissions in experiment configurations will be investigated and remedied if required. Data products from affected experiments will be updated as described above. Documentation will be updated as required.

Errors in user documentation and analysis will be investigated and remedied. Requests for improving or extended user documentation will be entertained during active support.

Once the active support window elapses the maintenance support window commences for a specified time. During maintenance support the data will no longer be updated. Errors in model component code may be investigated and remedied if the component is still being used in NRI models. If errors are found in the model configuration these will be characterised, and their effect documented in the meta-data of the data products. If the errors are significant enough the data product may be withdrawn.

There will be no updates to user documentation by the NRI during the maintenance support period. However the community is encouraged to create their own additional documentation at any time.

Once maintenance support lapses the experiment and its documentation and data products will be unsupported. When an experiment is unsupported there will be no updates at all. The data products will continue to be available to the community for a mandated period of time, but once this time has lapsed the data may be archived.

Once archived data products will not be readily accessible. If access is required a request will have to be made to retrieve the data from tape archive. The merits of the request will be assessed, along with the size and complexity of the retrieval process, and a decision made if the request will be granted. Archival support is not indefinite. There is a limit to archive support after which time the data *may* be deleted, or more likely not migrated when archive hardware is update.

Bugs or errors can be reported via an issue in the appropriate repository in the [ACCESS-NRI GitHub organisation].

## Indicative support timelines

The length of support timelines for flagship releases is yet to be determined. In the summary above the main support categories are 2 years each.

The data will be retained for at least 7 years after initial publication. Retained in this context means **not** deleted. This covers any legal requirement for data access linked to publications and grants.

[ACCESS-NRI GitHub organisation]: https://github.com/ACCESS-NRI
