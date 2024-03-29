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

The Data Hub is responsible for storing, retrieving and updating data – primarily metadata – but can also store and process other data in a customizable content field. It functions as the central data storage within the Open Integration Hub.

The stored data therefore includes the OIHDataRecord and the ApplicationDataRecord.

The OIHDataRecord contains:

- the dataset identifier within the OIH (oihUid),
- its creation and last modification dates within the OIH,
- an array of modification objects to track the modification history.

The ApplicationDataRecord can contain:

- the OIH's identifier for the application (applicationUid),
- the record's ID within the application (recordUid),
- an array of modification objects to track the modification history.

Data from different sources can be synchronized via the Data Hub and combined under a single unique identifier (oihUid) to keep track of the current state of the data and enable synchronization between multiple sources.

Additionally, the entire data of an entry can be stored in the content field of the Data Hub record. This is especially useful if the OIH is configured to operate in a _Hub & Spoke_ manner.

## Data import
Existing data can be imported into the Data Hub via the Import tool. This is especially useful when using the Hub and Spoke feature of data hub to import existing data from multiple sources into the hub and potentially interlink the data.
Please note that the data stored in Data Hub might be transformed into an existing data model (e.g. person and organization).
API spec for data import:

POST {datahub-api-endpoint}/data/import

## Data Export
Data Hub provides a REST endpoint to export existing data. Please note that the Data Hub exports only the internally stored and previously transformed data. To export the initially received raw data, please see the Raw Data Storage (RDS) specification. Also, RDS is an optional feature and only stores raw data if your connectors were previously configured accordingly.
API spec for data export:

GET {datahub-api-endpoint}/data

## Hub & Spoke

In a _Hub & Spoke_ configuration, the Data Hub can serve as data source and reduce the amount of necessary operations and data transfers needed to synchronize all connected systems, while avoiding unwanted duplicates.

For an overview of how updates can be handled via this approach see: [Update Propagation](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/updatePropagation.html).

The storage and retrieval of data is then automatically organized with the help of the [Dispatcher Service](https://openintegrationhub.github.io/docs/5%20-%20Services/DispatcherService.html).

# API and Implementation

[API Reference](https://data-hub.openintegrationhub.com/api-docs){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/data-hub){: .btn .fs-5 .mb-4 .mb-md-0 }

<!--[Service File](){: .btn .fs-5 .mb-4 .mb-md-0 }-->

## Technologies used

[MongoDB](https://www.mongodb.com/): MongoDB is used as the Data Hub's storage solution.

## How it works

Data can be added to the Data Hub and retrieved via a REST API. It is also possible to add data to existing entries.

Furthermore, it is possible to keep track of the modification history of an entry.

Additionally, the Data Hub can keep track of the unique ID an entry has in different connected systems i.e. ID Linking.

### ID Linking

#### SDF

As described in [Update Propagation](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/updatePropagation.html), the Data Hub receives events from the SDF Adapter. These events contain meta information including the `applicationUid` of the source application and the `recordUid` of the incoming payload within the source application.

The Data Hub stores information about each set of applicationUids and recordUids in an array called `refs`.
In case of a create operation it creates an oihDataRecord and adds the refs array with one object in it i.e. `applicationUid` and `recordUid` of the source system.
In case of an update operation it identifies the oihDataRecord by the incoming combination of `applicationUid` and `recordUid` and updates the content.

In both cases, after the successful creation or update, it emits an event to dispatcher service containing either only the object that was received by SDF Adapter (create) or the original object and in addition all existing combinations of `applicationUid` and `recordUid` (update).

In case of a create operation the target applications adapter sends the `recordUid` of newly created data record to the SDF Adapter. The SDF Adapter then passes it to the Data Hub, so that Data Hub can create a new reference in the `refs` array.

#### Ferryman

In use cases not implementing that Smart Data Framework, ID Linking can be implemented via the Ferryman library. By activating ID Linking on a Flow node, the ferryman will query the Data Hub for any records, and update records following the action step.

----

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

- Dispatcher Service: Emits events for dispatcher service in order to enable the _Hub & Spoke_ architecture (See: [update propagation](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/updatePropagation.html)).

- SDF Adapter: Receives events from SDF Adapter in order to create or update oihDataRecords (See: [update propagation](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/updatePropagation.html)).
