---
layout: default
title: Ferryman
parent: Services
nav_order: 14
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Ferryman** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->

The Ferryman is handling the execution of the connectors in every flow. It receives messages from the [Component Orchestrator](https://openintegrationhub.github.io//docs/Services/ComponentOrchestrator.html) and is executing the connectors triggers and actions with the supplied data.

<!-- [API Reference](){: .btn .fs-5 .mb-4 .mb-md-0 } -->
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/ferryman){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used

<!-- please name and elaborate on other technologies or standards the service uses -->
- RabbitMQ

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->

Each connector to the Open Integration Hub can contain several actions and triggers to receive and send data.

Strictly speaking not a service, the Ferryman is a library that abstracts the communication of the connectors with the OIH and handles the execution of the connectors in a flow.

Each connector loads its own instance of the ferryman. The Ferryman library then fulfills the service of receiving messages the [Component Orchestrator](https://openintegrationhub.github.io//docs/Services/ComponentOrchestrator.html) sends via the Queue and executes the corresponding action or trigger in the connector.

After execution the results of the operation are also passed to the ferryman by the connector.

Based on this the ferryman also can optionally send the oihId and the recordId of the data (The id of the data set in the connected system) to the [Data Hub](https://openintegrationhub.github.io//docs/Services/DataHub.html). for ID-Linking functionality.

Besides that if requested it will create data provenance events for the [Governance Service](https://openintegrationhub.github.io//docs/Services/GovernanceService.html).

Further functionality like sending to raw data storage is in development.


### Interaction with other Services
<!-- list and link the services this one interacts with and describe each interaction briefly (1-2 sentences) -->


- [Component Orchestrator](https://openintegrationhub.github.io//docs/Services/ComponentOrchestrator.html): Whenever an integration flow is running and an action is required from a connector, the Component Orchestrator notifies the ferryman of the said connector, which then takes the necessary actions.

- [Data Hub](https://openintegrationhub.github.io//docs/Services/DataHub.html): Optionally the ferryman sends the recordId the connector provides for an entry to the Data Hub for ID-linking to one OihId

- [Governance Service](https://openintegrationhub.github.io//docs/Services/GovernanceService.html): If requested in the flow step the ferryman generates data provenance events for each executed action or trigger and notifies the governance service via the queue.
