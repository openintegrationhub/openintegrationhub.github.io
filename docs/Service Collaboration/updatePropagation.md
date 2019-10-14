---
layout: default
title: Update Propagation
parent: Service Collaboration
nav_order: 2
---

# Introduction

If a message is sent to the Open Integration Hub via a connector flow there are several services within the Open Integration Hub that collaborate in order to propagate this message.
A connector flow always contains an adapter, transformer and Smart Data Framework (SDF) adapter.

The following graphic illustrates how update propagation works while using the hub and spoke architecture within the Open Integration Hub:

![hubAndSpoke](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/hubAndSpoke.png)

## Source Flow

In the source flow the triggering event is the reception of data by the adapter. This data is then passed to the transformer for a mapping and afterwards passed to the SDF adapter.
The SDF adapter then sends the data to the Open Integration Hub (For more information: [SDF Adapter](https://github.com/openintegrationhub/sdf-adapter)).

## Smart Data Framework

Once the message is received by the Data Hub it creates or updates an oihDataRecord. After the successful opertion it emitts an event which is then received by the Dispatcher Service.
The Dispatcher looks up configurations in which the source-flowId matches the incoming flowId. After all targets have been identified, the Dispatcher sends the new/updated dataset to the SDF Adapter . Furthermore, it sends start request(s) to the flow repository which itselfs emitts an event for component orchestrator (For more information: [Dispatcher Service](https://github.com/openintegrationhub/openintegrationhub/blob/master/services/dispatcher-service/README.md), [Flow Repository](https://openintegrationhub.github.io/docs/Services/FlowRepository.html)). The component orchestrator send starts all needed containers.

## Target Flows

Target flows always start with an instance of the SDF adapter. The SDF Adapter subscribes to a certain topic and passes the incoming message to transformer.After after a successful mapping, the Transformer passes the message to the adapter. The adapter then sends the data to the application and passes the recordUid of the entity to SDF adapter which sends it back to the Open Integration Hub for ID Linking. More on ID Linking: [ID Linking in Open Integration Hub](../../services/DataHub.md#id-linking)

## Create and Update Operations

### Create Events

The following figure shows what happens within the Smart Data Framework if  a create event is received:

![CreateEvent](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/Create-SDFCommunication.png)

### Delete or Update Events

The following figure shows what happens within the Smart Data Framework if a delete or update event is received:

![updateOrDelete](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/UpdateOrDelete-SDFCommunication.png)