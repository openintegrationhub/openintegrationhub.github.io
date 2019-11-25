---
layout: default
title: Attachment Storage
parent: Services
nav_order: 1
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# Attachment Storage Service

## Introduction
Stores files involved in an integration. 

[API Reference](http://attachment-storage-service.openintegrationhub.com/api-docs){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/attachment-storage-service){: .btn .fs-5 .mb-4 .mb-md-0 }
[Service File](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/attachment-storage-service){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used
- Node.js
- Typescript
- Redis

## How it works
Provides the REST API for storing and getting files. As a backend uses Redis.

### Supported content types
- application/octet-stream
- application/json
- application/xml
- text/xml
- text/plain
- text/csv
- text/tsv

### Interaction with other Services
- Interacts with IAM to introspect provided IAM token.
- Flow components are able to store and get their attachments into the Attachment Storage Service via the REST API.
