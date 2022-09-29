---
layout: default
title: Governance-Service
parent: Services
nav_order: 14
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->

![alpha](https://img.shields.io/badge/Status-Alpha-yellow.svg)

# **Governance Service** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->

The Governance Service serves as the central repository for all data governance-related data and functions inside the OIH. It offers both a database for long-term storage of relevant data, and an API for data retrieval and validation.

Besides that, it provides functionalities for displaying said data and for validating policies against provided data.

This service, an external service and therefore in its own repository.

[API Reference](http://governance-service.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/governance-service){: .btn .fs-5 .mb-4 .mb-md-0 }

<!-- [Service File](){: .btn .fs-5 .mb-4 .mb-md-0 } -->

## Technologies used

<!-- please name and elaborate on other technologies or standards the service uses -->

[MongoDB](https://www.mongodb.com/): MongoDB is used as the Governance Service storage solution.

## How it works

<!-- describe core functionalities and underlying concepts in more detail -->

### Policies

The "Policy" functions of the Governance Service are intended to offer a way to control exactly which and how data objects are passed through a flow, irrespective of connector logic.

Therefore, the Governance Service provides an endpoint to validate any policies on provided data.

You can find more details about policies here:

- [OIH Policies](https://openintegrationhub.github.io//docs/4%20-%20ForDevelopers/PolicyIntro.html):
OIH Policies offer a way for you to control exactly which and how data objects are passed through a flow, irrespective of connector logic.

Additionally, the Governance Service manages default and user created functions, that can be used in policies.

### Data Provenance

The "Data Provenance" function of the Governance Service is intended to allow users to reconstruct their data's path through the OIH from the very first time it was synchronized up until the current moment.

This way, the data owner will be able to track all origins and destinations of their data, and whether it has been modified inside the OIH. This way, the data owner will be made more capable of complying with data governance policies and laws, such as the GDPR.

To this end, the Governance Service is capable of receiving metadata about certain events, such as a data object being transmitted from one application to another, and stores it as a detailed data provenance event. These events can then be retrieved, filtered, and searched using the Service's API.

#### Provenance data model

The used data model is based on [PROV-DM](https://www.w3.org/TR/prov-dm/). This allows for easy mapping and export of provenance data to other systems. The model describes tuples of entities, agents, and activities, in addition to optional situational fields, such as describing one agent acting on behalf of another.

Example provenance object:

```json
{
  "entity": {
    "id": "aoveu03dv921dvo",
    "entityType": "oihUid"
  },
  "activity": {
    "activityType": "ObjectReceived",
    "used": "getPersons",
    "startedAtTime": "2020-10-19T09:47:11+00:00",
    "endedAtTime": "2020-10-19T09:47:15+00:00"
  },
  "agent": {
    "id": "w4298jb9q74z4dmjuo",
    "agentType": "Component",
    "name": "Google Connector"
  },
  "actedOnBehalfOf": [
    {
      "first": true,
      "id": "w4298jb9q74z4dmjuo",
      "agentType": "Component",
      "actedOnBehalfOf": "j460ge49qh3rusfuoh"
    },
    {
      "id": "j460ge49qh3rusfuoh",
      "agentType": "User",
      "actedOnBehalfOf": "t454rt565zz57"
    },
    {
      "id": "t454rt565zz57",
      "agentType": "Tenant"
    }
  ]
}
```

### Data visualization

The Governance Service also provides several endpoints for showing the data distribution and any flow warnings related to the governance functionality.

Besides that it can provide an interactive HTML view of the data distribution visualized in form of an interactive graph which can be embedded.

### Using the Governance Service

Besides a running instance of the Governance Service, it is required to set the flag _governance: true_ in the _nodeSettings_ of each flow step.

Furthermore, the ID-Linking functionality of the [Ferryman](https://openintegrationhub.github.io//docs/5%20-%20Services/IdentityManagement.html) should be activated by setting the _nodeSettings_ flag _idLinking:true_. This will also require that the
[Data Hub](https://openintegrationhub.github.io//docs/5%20-%20Services/DataHub.html) Service is running. Otherwise, it is not guaranteed that every provenance event has the required oihId.

### Governance Service API

The Governance Service offers a REST API through which the stored provenance data can be retrieved. To interact with this API, the user must supply a valid bearer token generated by the [Identity Management](https://openintegrationhub.github.io//docs/5%20-%20Services/IdentityManagement.html).

#### List of supported Methods and Routes

---

| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /event   |  GET   | Searches stored provenance events. | Based on the supplied filter criteria as detailed below |

The following query parameters can be appended to the URL to further refine the result list:

- page[size]
- page[number]
- from
- until
- filter[agent.id]
- filter[agent.agentType]
- filter[actedOnBehalfOf]
- filter[activityId]
- filter[activityType]

More details about the endpoint can be found in the swagger documentation of the service.


##### Policy related endpoints

You can find more details about policies here:

- [OIH Policies](https://openintegrationhub.github.io//docs/4%20-%20ForDevelopers/PolicyIntro.html):
  OIH Policies offer a way for you to control exactly which and how data objects are passed through a flow, irrespective of connector logic.


| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /applyPolicy   |  POST   | Applies a policy and returns the modified data and a passed flag | Based on the supplied policy and the userId|

Expects the following parameters:

Body:
- data (the data object)
- metdata (with policy)

Query:
- action

Each policy can make use of the provided default functions and the created stored functions

Default functions:

equals
notEquals

exists
notExists

Numeric:

smallerThan
biggerThan

smallerOrEqual
biggerOrEqual


Strings:

contains
notContains
hasLength

Object
keyLength
Number of keys in object

isType
Checks if value is type

anonymize
Anonymize field specified by key with 'XXXXXXXXXX'

| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /storedFunction   |  POST   | Adds a new stored function | Based on the userId |
| /storedFunction   |  DELETE   | Deletes a stored function | Based on functionId and the userId |
| /storedFunction   |  GET   | Gets a list of all stored functions | Based on the supplied filter criteria as detailed below and the userId|

The following query parameters can be appended to the URL to further refine the result list:

- page[size]
- page[number]
- from
- until
- filter[name]
- filter[id]

- names ('function1,function2, ... function-n')

sort
- createdAt
- -createdAt
- updatedAt
- -updatedAt


##### Dashboard related endpoints

| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /distribution   |  GET   | Returns overview of data distribution. | Based on the supplied user credentials |


Returns an object with the total amounts of:
- retrieved
- updated
- created
- deleted

for each connector


| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /distribution/graph   |  GET   | Returns data distribution in form of a graph | Based on the supplied user credentials |

Format:

```
{
	nodes: [
		{
		    id: serviceName,
		    created: 0,
		    updated: 0,
		    retrieved: 0,
		    deleted: 0,
		  },
		},
	],
	edges: [
		{
		  data: {
		    id: flowId,
		    created: 0,
		    updated: 0,
		    retrieved: 0,
		    deleted: 0,
		    source: false,
		    target: false,
		  },
		},
	]
}
```



| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /distribution/graph/html   |  GET   | Delivers a html page which is rendering the graph of the data distribution | Also dynamically showing additional data. Based on the supplied user credentials |
| /objectStatus/:id   |  GET   | Returns oihUid and all recordUids of a given object | Based on the supplied user credentials |

The following parameter is required in the URL:

oihUid or recordUid



| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /objectStatus/:id   |  GET   | Returns a list of current warnings and advisories regarding the flow configuration | Based on the supplied user credentials |

Currently warns about not existent nodeSettings and missing governance flags in flows.

| endpoint | method | description                        | comments                                                |
| -------- | :----: | ---------------------------------- | ------------------------------------------------------- |
| /objectStatus/:id   |  GET   | Returns a combined list of object distribution overview and flow warnings | Based on the supplied user credentials |


## REST-API documentation

Visit http://governance-service.openintegrationhub.com/api-docs/ to view the Swagger API-Documentation

### Interaction with other Services

<!-- list and link the services this one interacts with and describe each interaction briefly (1-2 sentences) -->

- [Ferryman](https://openintegrationhub.github.io//docs/5%20-%20Services/Ferryman.html):
  The Governance Service receives provenance events emitted by the ferryman module running on top of each Connector.
  The Ferryman also calls the governance service to check any provided policies

- [Data Hub](https://openintegrationhub.github.io//docs/5%20-%20Services/DataHub.html): Optionally the ferryman sends the recordId the connector provides for an entry to the Data Hub for ID-linking to one OihId

- [Identity Management](https://openintegrationhub.github.io//docs/5%20-%20Services/IdentityManagement.html): The Governance Service API endpoints relies on a bearer token supplied by the Identity Management to determine which integration flows the current user may see, and which actions they may take.
