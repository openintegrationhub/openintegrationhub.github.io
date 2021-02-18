# Service communication

The sum of all conceivable possibilities for automated integration processes seems enormous. From the standardization and central administration of various contact details to the coordination and monitoring of manufacturing processes. Everything that is possible is increasingly being integrated in different ways. However, the heterogeneity of possible use cases leads to different requirements for a framework.

The OIH already offers a large number of appropriated services and libraries that can be useful in very different scenarios. **[Reference to examples]**

Here we will assume a setup consisting of the following services needed to run OIH.

- IAM (Identity and Access Management)
- Component Orchestrator
- Scheduler or Webhooks
- Component Repository
- Flow Repository
- Snapshots Service

A communication of between all services internally and externally takes place depending on the situation either by **authenticated REST** calls ​​of the respective API or by means of a **message queue** (MQ).

## 1.1 Examples

In order to illustrate this, we would like to list a few examples here in a schematically reduced way:

a) A user creates a flow

`` User --REST--> Flow Repository ``

b) A user creates a service account

`` User --REST--> IAM ``

c) A user starts a simple flow

`` User --REST--> Flow Repository --MQ--> Component Orchestrator --REST--> IAM
                                 |--MQ--> Scheduler
                                 or--MQ--> Webhooks
``

d) A flow (2 nodes) is executed

``
                                                                |-REST--> Snapshots Service                        |-REST--> Snapshots Service          
Webhooks or Scheduler --MQ--> Component Orchestrator --MQ--> Flow Node 1 --MQ--> Component Orchestrator --MQ--> Flow Node 2 --MQ--> Component Orchestrator
``

## 1.2 Forms of communication

### 1.2.1 REST-API Call

Most services can be addressed via a REST-API call. Here, data can either be transferred or requested (see a), or instructions can be carried out by the user (see c)

### 1.2.2 Message queue
There are two different forms of MQ communication. 

**Services-Service communication** (see c) is managed and controlled via an event bus (link). 

**Component-Service communication** (see d) takes place either via a queue created by the component orchestrator or the event bus. There is no provision for direct communication between components. 