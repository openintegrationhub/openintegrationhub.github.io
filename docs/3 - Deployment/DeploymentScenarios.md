---
layout: default
title: Deployment Scenarios
parent: Deployment
nav_order: 3
---

# Deployment Scenarios

The OIH is structured to be modular in nature. Only a few services are strictly necessary for basic functionalities, while others offer optional services to fulfill certain use cases. In this document, we will show you a few common deployment scenarios, detailing which services are necessary for each of them.

## Overview of services used in different deployments

*Necessary:*

**Identity and Access Management (IAM)**: Create and modify users, tenants, roles, and permissions. Required to perform most API interactions on other services.

**Component Orchestrator**: Organizes the routing of the data transfer between the flow steps and starts required local components.

**Scheduler**: Polls connectors on a regular schedule to feed the flow. (Required for polling flows)

**Webhooks Service**: Executes flows based on incoming webhook calls. (Required to trigger flows based on a webhook)

**Flow Repository**: Create, modify, and start/stop integration flows.

**Component Repository**: Store and modify connector components.

*Optional:*

**Data Hub**: Long-term storage for flow content and snapshot data. Required for ID-Linking

**Secret Service**: Securely store authentication data for other applications, and execute Oauth authentication flows.

**Snap Shot Service**: Saves the state of the last execution of a flow. Used to prevent duplicate delivery of entries in polling flows.

**Metadata Repository**: Create and modify master data models used by your connectors.

**App-Directory**: Define apps connected by your flows and associate them with specified connectors and schemas used by them.

**Integration Layer Service**: Perform data operations such as merging or splitting objects.

**Web UI**: A basic browser-based UI to control

**Audit Log**: View event logs spawned by the other services.

**Attachment Storage Service**: Temporarily store larger files for easier handling in flows.

**Reporting and Analytics**: Gathers and presents data about the overall activity within your OIH installation

## Basic Flow Deployment

For executing a minimal flow at least the following OIH services have to be running:

See also: [Flow execution](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/FlowExecution.html)

- Identity and Access Management (IAM)

- Component Orchestrator

- Scheduler and/or Webhook Service (depending weather polling flows or webhooks are used)

- Flow Repository

- Component Repository

- Snap Shot Service (Optional but highly recommend for polling flows)

If the flow is using global connectors, then those need to be manually started through the component repository. Non global connectors are started automatically by the Orchestrator

## Flow Deployment with ID-linking

With the ID-Linking feature of the OIH, external ids of data sets coming from a connectors are grouped and linked to one entry with a OIH-Id. This prevents duplicate entries in the connected systems and enables dynamic updating of existing entries in connected systems.

Required Services:

- Basic Flow Deployment
- Data hub

## User-facing Flow Deployment

If you wish to offer flow functionalities to users other than yourself, additional functionalities may be useful. The Secret Service will allow these users to save authentication data without the risk of that date being exposed to anybody else. Resources and Analytics as well as the Audit Log can aid you in tracking user activity. Finally, the UI offers a starting point for users to familiarize themselves with the OIH's capabilities.

Required Services:

- Basic Flow Deployment
- Secret Service
- Web-UI
- Audit Log

## Hub & Spoke

The Hub & Spoke approach may be useful when you have scenarios in which you want to synchronize a shared data set among a large number of applications at once. Rather than requiring you to manually set up and run flows between every single involved application, it offers you an automated functionality to define a network of connected applications and have the necessary flows created and started automatically. 

As a rule of thumb, the Hub & Spoke approach can be useful if you regularly want to synchronize a given data set between more than three applications. For synchronizations between fewer applications, traditional single directional flows generally offer you the same functionality at lesser overhead.

Required Services:

- Basic Flow Deployment
- Data Hub
- Dispatcher Service
- App-Directory
