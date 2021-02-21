---
layout: default
title: Data Hub
parent: Services
nav_order: 6
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Data Hub**

## Introduction

<!-- 2 sentences: what does it do and how -->

The Data Hub is responsible for storing, retrieving and updating oihDataRecords. It functions as the central data storage (synchronized data) within the Open Integration Hub.

[API Reference](http://data-hub.openintegrationhub.com/api-docs){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/data-hub){: .btn .fs-5 .mb-4 .mb-md-0 }

<!--[Service File](){: .btn .fs-5 .mb-4 .mb-md-0 }-->

## Technologies used

[MongoDB](https://www.mongodb.com/): MongoDB is used as the Data Hub's storage solution.

## How it works

### ID Linking

As described in [update propagation]({{ site.baseurl }}{% link docs/BasicConcepts/updatePropagation.md %}) the Data Hub receives events by the SDF Adapter. These events contain meta information including the `applicationUid` of the source application and the `recordUid` of the incoming payload within the source application.

The Data Hub stores information about each set of applicationUids and recordUids in an array called `refs`.
In case of a create operation it creates an oihDataRecord and adds the refs array with one object in it i.e. `applicationUid` and `recordUid` of the source system.
In case of an update operation it identifies the oihDataRecord by the incoming combination of `applicationUid` and `recordUid` and updates the content.

In both cases, after the successful creation or update, it emits an event to dispatcher service containing either only the object that was received by sdf adapter (create) or the original object and in addition all existing combinations of `applicationUid` and `recordUid` (update).

In case of a create operation the target applications adapter sends the `recordUid` of newly created data record to the SDF Adapter. The SDF Adapter then passes it to the DataHub, so that Data Hub can create a new reference in the `refs` array.

The following code represents the structure of an oihDataRecord:

```json
{
  "data": [
    {
      "domainId": "string",
      "schemaUri": "string",
      "content": {},
      "refs": [
        {
          "applicationUid": "string",
          "recordUid": "string",
          "modificationHistory": [
            {
              "user": "string",
              "operation": "string",
              "timestamp": "2019-10-09T11:43:25.270Z"
            }
          ]
        }
      ],
      "owners": [
        {
          "id": "123",
          "type": "user"
        }
      ],
      "id": "string",
      "createdAt": "2019-10-09T11:43:25.270Z",
      "updatedAt": "2019-10-09T11:43:25.270Z"
    }
  ],
  "meta": {
    "page": 0,
    "perPage": 0,
    "total": 0,
    "totalPages": 0
  }
}
```

### Interaction with other Services

- Dispatcher Service: Emits events for dispatcher service in order to enable the hub and spoke archticture (See: [update propagation]({{ site.baseurl }}{% link docs/BasicConcepts/updatePropagation.md %})).
- SDF Adapter: Receives events from SDF Adapter in order to create or update oihDataRecords (See: [update propagation]({{ site.baseurl }}{% link docs/BasicConcepts/updatePropagation.md %})).
