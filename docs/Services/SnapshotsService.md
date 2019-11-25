---
layout: default
title: Snapshots Service
parent: Services
nav_order: 17
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# Snapshots service

## Introduction
This service is responsible for storing flow snapshots.

### What is a snapshot?
Snapshot is a feature of the OIH platform, which provides flows components a possibility to store some `state` between executions.
A typical use case is to store some `timestamp` and use it to query records from a certain system, created after this `timestamp`,
so that you would not need to query full list of records each time. 

[API Reference](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/snapshots-service){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/snapshots-service){: .btn .fs-5 .mb-4 .mb-md-0 }
<!-- [Service File](){: .btn .fs-5 .mb-4 .mb-md-0 } -->

## Technologies used
- Node.js
- Typescript
- MongoDB
- RabbitMQ

## How it works
It connects to the RabbitMQ queue, which is in turn subscribed for the `snapshot` events, fired by flow components.
The service consume the messages from the queue and writes it to the DB. The snapshots are then accessible via REST API.

### Interaction with other Services
- Interacts with IAM to introspect provided IAM token.
- Consumes `snapshot` events from flow components via RabbitMQ.
- Component Orchestrator queries the service and provides snapshots to each flow component. 
