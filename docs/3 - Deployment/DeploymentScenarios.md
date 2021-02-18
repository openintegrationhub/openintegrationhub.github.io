---
layout: default
title: Deployment Scenarios
parent: Deployment
nav_order: 3
---

# Deployment Scenarios

The OIH is structured to be modular in nature. Only a few services are strictly necessary for basic functionalities, while others offer optional services to fulfill certain use cases. In this document, we will show you a few common deployment scenarios, detailing which services are necessary for each of them.

## Overview of services used in different deployments

**Identity and Access Management (IAM)**. Create and modify users, tenants, roles, and permissions.

**Component Orchestrator**. Organizes the routing of the data transfer between the flow steps and starts required local components.

**Scheduler**. Polls connectors on a regular schedule to feed the flow.

**Secret Service**. Securely store authentication data for other applications.

**Flow Repository**. Create, modify, and start/stop integration flows.

**Component Repository**. Store and modify connector components.

*Optional:*

**Data Hub**. Long-term storage for flow content and snapshot data. Required for ID-Linking

**Snap Shot Servie**. Saves the state of the last execution of a flow. Can prevent duplicate delivery of entries.

**Webhooks Service**. Starts flows based on incoming calls.

**Metadata Repository**. Create and modify master data models used by your connectors.

*Integration Layer Service*. Perform data operations such as merging or splitting objects.

*Web UI*. A basic browser-based UI to control

*Audit Log*. View event logs spawned by the other services.

*Attachment Storage Service*. Temporarily store larger files for easier handling in flows.

## Basic flow based deployment

For executing a minimal flow at least the following OIH services have to be running:

- Identity and Access Management (IAM)

- Component Orchestrator

- Scheduler

- Flow Repository

- Component Repository

- Snap Shot Service (Optional but highly recommend)

If the flow is using global connectors, then they need to be started before. Non global connectors are started automatically by the Orchestrator

## Flow based deployment with ID-linking

With the ID-Linking feature of the OIH external ids of data sets coming from a connectors are linked to one entry with a OIH-Id. This prevents duplicate entries in the connected systems and enables to synchronize data easier.
  
For using the ID-Linking functionalities all the aforementioned services are required and additionally the following:

- Data hub

## Hub & Spoke

The Hub & Spoke approach may be useful when you have scenarios in which you want to synchronize a shared data set among a large number of applications at once. Rather than requiring you to manually set up and run flows between every single involved application, it offers you an automated functionality to define a network of connected applications and have the necessary flows created and started automatically. As a rule of thumb, the Hub & Spoke approach can be useful if you regularly want to synchronise a given data set between more than three applications. For synchronizations between fewer applications, traditional single directional flows generally offer you the same functionality at lesser overhead.

Required Services:

- As Basic Flow Deployment
- Data Hub
- Dispatcher Service
