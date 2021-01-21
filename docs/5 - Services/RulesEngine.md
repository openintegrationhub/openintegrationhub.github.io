---
layout: default
title: Rules Engine
parent: Services
nav_order: 19
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)


# **Rules Engine / Logic Gateway** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->
Although initially planed to be a dedicated service, Rules engine (aka Logic Gateway) will be implemented as a special type of privileged component, which can be used in a flow.   

[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/logic-gateway){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used
<!-- please name and elaborate on other technologies or standards the service uses -->

Node.Js

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->
A Logic Gateway is very similar to a BPMN gateway. It can have multiple inbound/outbound edges and usually placed within a flow. These components control the subsequent flow execution and can have sophisticated expression to evaluate the results of all previous flow steps.

Initially, flows in the Flow Repository were designed to be a directed graph without loops. This works well for exclusively machine-to-machine interaction, but lacks functionality for asynchronous actions within a flow or when input specific conditions should execute a specific sub-graph. In combination with the Workflow Repository, OIH introduced Rules Engine to create BPMN like processes with flows, targeting the needs of SME.

## Flow control

Currently, whenever a flow step is finished a message is sent back to the Orchestrator. The Orchestrator has a copy of all currently running flows and determines which next nodes should be executed. A Logic Gateway can instruct the Orchestrator to execute another step, independent of the next step in the flow graph. 

The orchestrator shall verify that the current step is actually a logic gateay component (which could only be defined by a user) and only accept such instructions only from a logic gateway component. 

If a workflow consists of multiple subsequent flows, the last component of the predecessor flow shall be a logic gateway containing the instruction to execute the next flow. The orchestrator needs to make sure, that both flows are part of a common workflow (thus the same owner) to prevent any unexpected or malicious executions. 

## Gateway types

Following gateway types were identified and will be implemented. Further gateway may fellow when required.

* Split: Execute multiple graphs/flows in parallel
* Switch: Execute a subset of parallel flows depending on the condition defined in the gateway
* Join: Wait for the incoming parallel flows/graphs to finish. Can be conditional, e.g. repeat a graph until condition met, wait for the first incoming flow, wait for all to finish with a specific result
* Filter: Allow only for specific results to pass through.

![Workflow](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/Workflow-Repository-3.png)


## Gateway logic

As described in the Workflow Repository, all data associated with a workflow shall be passed via Snapshot Repository to every Ferryman based node in a flow. Logic Gateway components can utilize this approach as well and thus access any data previously emitted by any node.

Gateways will also expose methods to control the flow execution. These are:
* `executeStep(stepId)`
* `rescheduleStep(stepId, delayInSeconds)`

The following code contains an example of how the actual rule could look like.
```js

const processRule = async ({ flows, snapshot }) => {
    if (flows[0].steps[2].completed && flows[0].steps[3].completed) {
        if (snapshot.flowData[0].tasks[0].dueDate < new ISODate('2021-01-012T00:00:00.007Z')) {
            cb(null, {
                nextStepId: 4,
            });    
        } else {
            cb(null, {
                nextStepId: 5,
            })
        }       
    }
}
```

The payload sent to the Orchestrator:

```json
{
    "command": "executeStep",
    "jwtToken": "TOKEN",
    "flowId": "59feb5ba112f28001921f233",
    "stepId": 3
}
```

