---
layout: default
title: Meta Data Repository
parent: Services
nav_order: 13
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Meta Data Repository** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->

The Meta Data Repository is responsible for storing domains and their master data models. The models stored within this service are consulted for different tasks such as data validation. The meta models are also used by the transformer to map the incoming data onto the Open Integration Hub standard.

[API Reference](http://metadata.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/meta-data-repository){: .btn .fs-5 .mb-4 .mb-md-0 }
[Event Controller](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/meta-data-repository#event-controller){: .btn .fs-5 .mb-4 .mb-md-0 }

<!--[Service File](){: .btn .fs-5 .mb-4 .mb-md-0 }-->

## Technologies used

<!-- please name and elaborate on other technologies or standards the service uses -->

- [Node.js](https://nodejs.org)
- [MongoDB](https://www.mongodb.com)
- [JSON Schema](https://json-schema.org/)

## Purpose of the Meta Data Repository

If we talk about metadata in this context, we mean the description of the domains and their corresponding Master Data Models. An Open Integration Hub Master Data Model (OMDM) describes the data of a certain domain in a depth which is sufficient enough to map and synchronize the specific data of multiple applications in that domain. The meta data delivers all the information a user or customer needs to work with data within a specific domain.

The domain models are specified by special workgroups. Please see the specific domain model repository for further informations on a domain and its master data model.

## Basic Version

### Model Structure

The service is mainly responsible for storing meta models and domains. In order to unify the meta data to describe domains and models we need a model structure for both objects.

#### Domain Object

The domain object is responsible describing the domain itself. Thus, the following object structure is proposed:

```json
{
  "$schema": "http://json-schema.org/draft-06/schema#",
  "$id": "http://json-schema.org/draft-06/schema#",
  "title": "Domain object description",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier of the domain",
      "examples": [1]
    },
    "name": {
      "type": "string",
      "description": "Name of the domain",
      "examples": ["products"]
    },
    "description": {
      "type": "string",
      "description": "Short description of the domain",
      "examples": [
        "This domain includes all product related models"
      ]
    },
    "owners": {
      "type": "array",
      "description": "List of owners, who have access to this domain",
      "items": {
        "properties": {
          "id": {
            "type": "string"
          },
          "type": {
            "type": "string"
          }
        }
      },
      "examples": [
        {
          "id": "5bffec99a43c7f3ca95b09e6",
          "type": "tenant"
        }
      ]
    }
  }
}
```

#### Model Object

The model object is responsible for describing the meta model and should have a reference to the superordinated domain.
Therefore, the following structure is supposed:

```json
{
  "$schema": "http://json-schema.org/draft-06/schema#",
  "$id": "http://json-schema.org/draft-06/schema#",
  "title": "Domain object description",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier of the model",
      "examples": ["13"]
    },
    "domaindId": {
      "type": "string",
      "description": "Unique identifier of the domain the model belongs to",
      "examples": ["1"]
    },
    "description": {
      "type": "string",
      "description": "Short description of the model",
      "examples": [
        "Master Data Model Products v1"
      ]
    },
    "owners": {
      "type": "array",
      "description": "List of owners, who have access to this domain",
      "items": {
        "properties": {
          "id": {
            "type": "string"
          },
          "type": {
            "type": "string"
          }
        }
      },
      "examples": [
        {
          "id": "5bffec99a43c7f3ca95b09e6",
          "type": "tenant"
        }
      ]
    },
    "model": {
      "type": "object",
      "description": "A JSON schema of the actual model",
      "examples": [
        {
          "$schema": "http://json-schema.org/schema#",
          "$id": "https://github.com/openintegrationhub/Data-and-Domain-Models/blob/master/src/main/schema/addresses/personV2.json",
          "title": "Person",
          "description": "Describes a natural person",
          "type": "object",
          "allOf": [
            {
              "$ref": "../oih-data-record.json"
            }
          ],
          "properties": {
            "title": {
              "type": "string",
              "description": "Title of the person",
              "examples": ["Dr."]
            },
            "salutation": {
              "type": "string",
              "description": "Salutation of the person",
              "examples": ["Mr."]
            },
            "firstName": {
              "type": "string",
              "description": "Given name of the person",
              "examples": ["Max"]
            }
          }
        }
      ]
    }
  }
}
```

### Interaction with other Services

Meta Data Repository can receive events from any service, but only directly interacts with two of them:

- [Message Oriented Middleware]({% link docs/Services/MessageOrientedMiddleware.md %}): It receives all events through the Message Oriented Middleware. It publishes several events for most important actions e.g. "metadata.domain.deleted".

- [Identity Management]({% link docs/Services/IdentityManagement.md %}): It requires a bearer token created by the Identity Management to determine current user and check required permissions.
