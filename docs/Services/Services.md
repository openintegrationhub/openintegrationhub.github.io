---
layout: default
title: Services
nav_order: 2
has_children: true
---

# Overview

The Open Integration Hub framework consists of a variety of service. This will give you a good overview:

## Auditlog
The Auditlog serves to receive, store, and return logging information about important user actions and system events. Other services generate audit messages and pass them on to the Audit Log via the message and event bus or a simple HTTP POST request.

Stored Logs can be retrieved by authorized users through a simple REST API. [Auditlog](https://openintegrationhub.github.io/docs/Services/AuditLog.html)

## Component Orchestrator

The Component Orchestrator orchestrates flow lifecycle. It creates queues in RabbitMQ and manages Docker containers (deploy/stop/remove) for each flow node whenever a flow is created, stopped or removed. For further information see:[Component Orchestrator](https://openintegrationhub.github.io/docs/Services/ComponentOrchestrator.html).

## Component Repository

A component is a piece of software that connects another application to Open Integration Hub. This includes Adapters and Transformer. Components are lightweight and stand-alone Docker images stored in a
component repository: [Component Repository](https://openintegrationhub.github.io/docs/Services/ComponentRepository.html).

## Flow Repository

All connected solutions to the Open Integration Hub and the work they are doing there are represented by an "integration flow". The Flow Repository is responsible for storing, retrieving, updating and deleting integration flows: [Flow Repository](https://openintegrationhub.github.io/docs/Services/FlowRepository.html)

## Identity Access Management

Identity Access Management (IAM) provides secure authentication and authorization of users/clients. The IAM Service can be configured to use JWT tokens with HMAC or RSA. It has also support for OpenId Connect: [IAM](https://openintegrationhub.github.io/docs/Services/IdentityManagement.html)

## Integration Layer Services

The basic purpose of the Integration Layer Service is to receive data objects from one or several incoming flows, apply some business logic on them (such as a merge or split), validate them against a supplied schema, and then provide the resulting valid objects as input to other flows. For this purpose, the ILS is capable of temporarily storing these objects in a database until they are no longer required. [Integration Layer Service](https://openintegrationhub.github.io/docs/Services/IntegrationLayerService.html)

## Message Oriented Middleware

All the communication between steps of an integration flow in runtime is happening through a message broker. Two steps
of an integration flow are separate Docker container isolated from each other. They are connected through a messaging
queue than provides them a reliable and asynchronous communication protocol. See more details on  message oriented middleware: [MessageOrientedMiddleware](https://openintegrationhub.github.io/docs/Services/MessageOrientedMiddleware.html).

## Meta Data Repository

The Meta Data Repository is responsible for storing domains and their master data models. The models stored within this service are consulted for different tasks such as data validation. The meta models are also used by the transformer to map the incoming data onto the Open Integration Hub standard. For further information see:[Meta Data Repository](https://openintegrationhub.github.io/docs/Services/MetaDataRepository.html)

## Scheduler

There are two ways to trigger a flow execution: to poll changes periodically or to receive notifications through webhooks.
The majority of integration flows poll perform polling as the most of APIs out there don't support webhooks. The polling
is performed periodically with the help of the Scheduler: [Scheduler](https://openintegrationhub.github.io/docs/Services/Scheduler.html)

## Secret Service

The Secret Service is used to  securely store and access client/user credentials. For further information see: [Secret Service](https://openintegrationhub.github.io/docs/Services/SecretService.html).

## Webhooks

The webhooks service is used to expose externally-reachable URLs for integration
flows that can be used to implement webhook-triggered flows. The webhooks service receives http calls and passes messages to execution. For further information: [Webhooks](https://openintegrationhub.github.io/docs/Services/Webhooks.html).
