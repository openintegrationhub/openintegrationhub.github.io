---
layout: default
title: Raw Data Storage
parent: Services
nav_order: 19
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->

![dev](https://img.shields.io/badge/Status-Development-red.svg)

# **Raw Data Storage**

 <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

As the name implies, Raw Data Storage (RDS), can be used to store all raw data received by connectors.
It receives messages containing raw, untransformed, and unformatted data to store their payload inside a database

This service, an additional service and therefore in its own repository.

<!-- [API Reference](){: .btn .fs-5 .mb-4 .mb-md-0 }-->
[API Reference](https://rds.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/raw-data-storage-service){: .btn .fs-5 .mb-4 .mb-md-0 }

## How it works

<!-- describe core functionalities and underlying concepts in more detail -->

This service is optional and may be used to extract tenant specific raw data at later time, i.e. for later data transformations if OIH Data Hub data models should change.
To enable this feature, set the following flag for any connector in the corresponding `nodeSettings`
```
cfg.nodeSettings.storeRawRecord = true
```
The Ferryman will recognize the flag and automatically emit the raw data to RDS where it will be stored with an uuid reference. The transformed data stored in the DataHub will also store this uuid as a backreference to the raw data record.
