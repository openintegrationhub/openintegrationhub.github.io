---
layout: default
title: Audit Log
parent: Services
nav_order: 2
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)


# **Audit Log** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction
<!-- 2 sentences: what does it do and how -->

The Audit Log receives, stores, and returns logging information about important user actions and system events. Other services can generate audit messages and pass them on to the Audit Log via the message and event bus or a simple HTTP POST request.

Stored Logs can be retrieved by authorized users through a REST API. The Audit Log offers several functions to filter and search through the accumulated logs.

[API Reference](http://auditlog.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/audit-log){: .btn .fs-5 .mb-4 .mb-md-0 }
<!--[Service File](){: .btn .fs-5 .mb-4 .mb-md-0 }-->

## Technologies used
<!-- please name and elaborate on other technologies or standards the service uses -->

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->
