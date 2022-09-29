---
layout: default
title: Monorepo services and external services
parent: System Architecture
nav_order: 1
---

# Monorepo services and external services

OIH Services are divided in core services and external services.

Core services are in the [openintegrationhub mono repository]/https://github.com/openintegrationhub/openintegrationhub) in the services folder.

All external services are in their separate repository. For example the [template repository](https://github.com/openintegrationhub/template-repository).

The Open Integration Hub is structured to be modular in nature. Only a few services are strictly necessary for basic functionalities.

For more information about when and how to use the different services, see also: [Deployment Scenarios](https://openintegrationhub.github.io/docs/3%20-%20SGettingStarted/DeploymentScenarios.html)


### Monorepo services "core services"

**Identity and Access Management (IAM)**: Create and modify users, tenants, roles, and permissions. Required to perform most API interactions on other services.

**Component Orchestrator**: Organizes the routing of the data transfer between the flow steps and starts required local components.

**Scheduler**: Polls connectors on a regular schedule to feed the flow. (Required for polling flows)

**Webhooks Service**: Executes flows based on incoming webhook calls. (Required to trigger flows based on a webhook)

**Flow Repository**: Create, modify, and start/stop integration flows.

**Component Repository**: Store and modify connector components.

**Data Hub**: Long-term storage for flow content and snapshot data. Required for ID-Linking

**Secret Service**: Securely store authentication data for other applications, and execute OAuth authentication flows.

**Snap Shot Service**: Saves the state of the last execution of a flow. Used to prevent duplicate delivery of entries in polling flows.

**Web UI**: A basic browser-based UI to control

**Audit Log**: View event logs spawned by the other services.

**Reporting and Analytics**: Gathers and presents data about the overall activity within your OIH installation

### External Services

**Metadata Repository**: Create and modify master data models used by your connectors.

**App-Directory**: Define apps connected by your flows and associate them with specified connectors and schemas used by them.

**Integration Layer Service**: Perform data operations such as merging or splitting objects.

**Attachment Storage Service**: Temporarily store larger files for easier handling in flows.

**Governance Service**: The Governance Service serves as the central repository for all data governance-related data and functions inside the OIH. It offers both a database for long-term storage of relevant data, and an API for data retrieval and validation. Besides that, it provides functionalities for displaying said data and for validating policies against provided data.

**Dispatcher Service**: The Dispatcher Service is responsible for routing messages between individual connector flows. This allows for the exchange of data in a centralized fashion via the Hub & Spoke model.

**Raw Data Storage Service**: RDS (raw data storage) is responsible for keeping data untransformed in its original form. During a flow runtime, data can be transmitted via the Ferryman before it is used within the transformer.

**SmartAssistant Service**:

**Template Repository**: This repository is designed to store templates for creating integration flows.

**Workflow Repository**:
