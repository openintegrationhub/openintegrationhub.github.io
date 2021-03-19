---
layout: default
title: Message Oriented Middleware
parent: Services
nav_order: 12
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Message Oriented Middleware** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->

The Message Oriented Middleware stores and routes messages while transferring them from senders to receivers.

## Technologies used

<!-- please name and elaborate on other technologies or standards the service uses -->

- RabbitMQ

## How it works

<!-- describe core functionalities and underlying concepts in more detail -->

An integration flow is represented by a directed acyclic graph in which
nodes are represented by integration components communicating with a
particular API or executing some custom logic. The edges of the
integration graph define which of two components are connected.

An integration flow is executed by number of [Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/),
each representing a flow's node, also called a `flow step`. The steps
communicate which each other through a messaging queue, such as [RabbitMQ](https://www.rabbitmq.com/).

<!--
The following diagram displays an example of an integration flow using
a message broker.

![Message Oriented Middleware](Assets/MessageOrientedMiddleware.png)

In the diagram above `Step 1` is a trigger component producing data by
polling an API periodically. The produced messages are sent to a queue
connecting `Step 1` and `Step 2`. Because the component in `Step 2` is
very slow, its consumption rate is lower than the publish rate. The
result is that the queue is growing. That's why 2 instances of `Step 2`
are started, each consuming messages from the same queue. The message
broker makes sure that the messages are sent to a single consumer only.

### Interaction with other Services
list and link the services this one interacts with and describe each interaction briefly (1-2 sentences) -->
