---
layout: default
title: Component Orchestrator
parent: Services
nav_order: 3
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)


# **Component Orchestrator** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction
<!-- 2 sentences: what does it do and how -->

The Component Orchestrator is responsible for a fair resource distribution.

<!--[API Reference](){: .btn .fs-5 .mb-4 .mb-md-0 }-->
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/component-orchestrator){: .btn .fs-5 .mb-4 .mb-md-0 }
[Service File](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/component-orchestrator){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used
<!-- please name and elaborate on other technologies or standards the service uses -->

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->

In a multi-tenant environment it must be guaranteed that a user or tenant
(intentionally or unintentionally) may not get an unfair usage of shared
resources such as CPU, Memory, Network, etc. It must be guaranteed that
every integration flow gets a chance to be executed, close to the intervals
defined by its Cron expression.

The Component Orchestrator is a micro-service defining the fairness policy
and controlling that each user/flow/tenant is complying with that policy.
The Component Orchestrator is responsible for:

* Protection from over-scheduling: if an execution of a flow takes longer than its scheduling interval, the following executions must be skipped.
* Making sure that a flow or its steps are not deployed multiple times
* If scaling is configured for a flow, the specified number of instances must be deployed
* Detection of policy violations and punishment of "bad citizens"
