---
layout: default
title: Webhooks
parent: Services
nav_order: 17
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Webhooks** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->
Receives http calls and passes messages to execution.

<!-- [API Reference](){: .btn .fs-5 .mb-4 .mb-md-0 }-->
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/webhooks){: .btn .fs-5 .mb-4 .mb-md-0 }
[Service File](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/webhooks){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used
<!-- please name and elaborate on other technologies or standards the service uses -->

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->

It listens for incoming HTTP connections, serializes incoming data and puts it to the queue of the first node of the flow. The message is being consumed and processed by the component.

### Available endpoints

- `HEAD /hook/{flowId}` - returns `200` if a flow is found and ready for receiving messages, otherwise `404`. This endpoint doesn't process any incoming data.
- `GET /hook/{flowId}` - this endpoint processes incoming data. It's possible to pass message arguments as query params and headers.
- `POST /hook/{flowId}` - this endpoint processes incoming data. It allows to pass data in request body, headers or query params.

## Prerequisites

- RabbitMQ
- MongoDB

## How to build

```docker
docker build -t openintegrationhub/communication-router:latest -f Dockerfile ../../
```

or

```npm
VERSION=latest npm run build:docker
```

## How to deploy

Kubernetes descriptors for Communication Router along with the other core platform services can be found in the [k8s](./k8s) directory.

## Environment variables

### General

| Name | Description |
| --- | --- |
| LISTEN_PORT | Port for HTTP interface. |
| LOG_LEVEL | Log level for logger. |
| MONGODB_URI | MongoDB connection string. |
| PAYLOAD_SIZE_LIMIT | Maximum request's payload size that could be handled. |
| RABBITMQ_URI | RabbitMQ connection URI for the Resource Coordinator application. |
