---
layout: default
title: Master Data Models
nav_order: 7
has_children: true
---

<p align="center">
  <img src="https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/large-oih-vertikal-zentriert.png" alt="Open Integration Hub" width="400"/>
</p>

# Master Data Models

## Why Master Data Models?

A fundamental challenge of data integration is mapping data, because applications usually use different properties and structures to represent their data.
Traditionally the mapping is manually, where the data is copied between the objects. This is not very efficient, time consuming and does not scale.

The Open Integration Hub's goal is to make integration easier, by providing reusable standard components. In this spirit, also mapping of data should be easier and scalable.

In order to map data only once per application and not for every single integration flow, we make use of standardized `Transformers`. Those contain mappings of an applications data schema to a Master Data Model.

Using the (optional) _Data Hub_ enables _Master Data Management_ for the respective domain by centrally storing the data of the integrated applications.


## General Approach

There is not _the one_ canonical data model for Open Integration Hub. Instead a model for each context (domain) is defined:

- A Master Data Model describes the data of a certain domain in a depth which is sufficient enough to map and synchronize the specific data of multiple applications in that domain.
- The model is generic and it can be extended on the application level.

## Structure

Data models consists of numerous entities. Hence, it is necessary to split a data model into smaller parts to enable the transfer of optimized amounts of data:

- __Every Master Data Model consists of one or multiple _loosely coupled_ sub-models.__

Although it is necessary to split (especially big) data models into smaller sub-models, it often does not make sense to send each entity on its own, when there is a _high cohesion_ between certain entities. Hence,

- __each sub-model can consist of one _or more_ entities.__


- __In case a sub-model consists of multiple entities, it *must* be modeled as an _aggregate_:__

> __Aggregate__ is a pattern in Domain-Driven Design. A DDD aggregate is a cluster of domain objects that can be treated as a single unit. An example may be an order and its line-items, these will be separate objects, but it's useful to treat the order (together with its line items) as a single aggregate.
>
> An aggregate will have one of its component objects be the aggregate root. Any references from outside the aggregate should only go to the aggregate root. The root can thus ensure the integrity of the aggregate as a whole.
>
> [s. [Martin Fowler: DDD_Aggregate](https://martinfowler.com/bliki/DDD_Aggregate.html)]
### JSON-Schema

All Master Data Models in Open Integration Hub use [JSON Schema](http://json-schema.org) as the format that data is processed with.

Open Integration Hub is able to validate data at runtime. As Master Data Models are split into sub-models, those sub-models must be processable independently.

- __for every sub-model there must be a _seperate_ JSON schema describing the entity or aggregate.__

As there are situations where entities are reused in (i.e. are part of) two or more aggregates, it is of course adequate to encapsulate those entities in an additional schema file and reference them from the several sub-models to avoid redundancy.

<!--
## Usage within Open Integration Hub
### Metadata Repository
### Data Hub
### ID Linking
### Integration Layer Service
### Conflict Management
-->
