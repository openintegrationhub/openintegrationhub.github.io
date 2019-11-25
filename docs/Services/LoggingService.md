---
layout: default
title: Logging Service
parent: Services
nav_order: 16
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# Logging service

## Introduction
This service is responsible for providing a convenient API to access flow execution logs.

[API Reference](http://flow-repository.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/logging-service){: .btn .fs-5 .mb-4 .mb-md-0 }
<!-- [Service File](){: .btn .fs-5 .mb-4 .mb-md-0 } -->

## Technologies used
- Node.js
- Typescript
- GCP Stack Driver

## How it works
Each of flow components running in its own Docker container and write its logs to `stdout` and `stderr`.
These log entries are being collected by GCP Stack Driver. This service allows to get the logs page by page using a convenient API.

### Interaction with other Services
- interacts with IAM to verify introspect provided IAM token
