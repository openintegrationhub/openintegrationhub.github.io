---
layout: default
title: Scheduler
parent: Services
nav_order: 13
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Scheduler** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->
The service scheduler is responsible for a periodical execution of integration flows.

<!-- [API Reference](){: .btn .fs-5 .mb-4 .mb-md-0 } -->
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/scheduler){: .btn .fs-5 .mb-4 .mb-md-0 }
[Service File](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/scheduler){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used

<!-- please name and elaborate on other technologies or standards the service uses -->
- Cron-Deamon

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->

In the Open Integration Hub there is a great number of active integration flows
to be executed periodically. Each integration can be configured with a
[Cron](https://en.wikipedia.org/wiki/Cron) expression defining flow's execution
interval. Let's consider the following Cron expression:

````sh
*/3 * * * *
````

The cron expression above executes an integration flow at every 3rd
minute starting from the flow's start time.

With the following Cron expression a flow is executed every Sunday at 6:00 am:

````sh
0 6 * * 7
````

The Scheduler iterates over all active integration flows and evaluates
their Cron expressions. Is a flow due to be executed, Scheduler tells the
Resource Coordinator to deploy the integration flow for execution.

### Interaction with other Services
<!-- list and link the services this one interacts with and describe each interaction briefly (1-2 sentences) -->
