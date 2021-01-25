<!-- ---
layout: default
title: Transformer
nav_order: 3
parent: Connectors
--- -->

# !!! Outdated: has to be adjusted to transformation via ferryman!!!

# Transformation Functions

As already mentioned the transformation functions transforms one JSON object into another. Prior to this transformation a semantic mapping has to take place where the entities of the source model are mapped against the entities of the Open Integration Hub master data model.

## Table of Contents

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Table of Contents](#table-of-contents)
- [Transformer Concept](#transformer-concept)
- [Transformation Language](#transformation-language)
- [Transformer Evolution](#transformer-evolution)

<!-- /TOC -->

## Transformer Concept

As can be seen in the [connector overview](https://github.com/openintegrationhub/Connectors/blob/master/Assets/ConnectorsV2.svg) a transformer should transform data in both directions:

1. From _source model_ to _Open Integration Hub master data model_
2. From _Open Integration Hub master data model_ to _source model_

A transformer expects a JSON object as an input. Depending on the direction of the transformation the input either represents the structure of the proprietary data model (_flow direction 1_) or the structure of the Open Integration Hub master data model (_flow direction 2_).

Afterwards it transforms the incoming JSON object into another JSON object. This can also be done via a transformation language like _JSONata_.

Depending on the transformation direction, the transformer's output is then send either to the Open Integration Hub and is validated against a deposited JSON schema or to the corresponding connector.

\_Note: As the Open Integration Hub is feasible of storing different data models, it is also possible that the mapping of the source model is done against another model than the Open Integration Hub master data model.

<!--An example of how an implementation with individually uploaded data models could look like is presented under the section [possible implementations](#possible-implementations)._-->

## Transformation Language

One way of transforming the JSON objects is the usage of a transformation language. As already mentioned one possible transformation language is JSONata as it is especially built to transform one JSON object into another.

For detailed information on transformation language and JSONata as well as a general example please have a look at [TransformationLanguage](https://github.com/openintegrationhub/Connectors/blob/master/Transformer/TransformationLanguage.md)

## Transformer Evolution

Sometimes there is a need to change the existing data model to adjust to different requirements. In this case, a transformer needs to be adjusted/updated in order to be compatible with the newest version of the data model.

Some changes do not affect existing mappings/transformations and transformers can still be used (although they are not referring to the newest model version). Thus, backward compatibility is automatically given.

According to the [OData specification](http://docs.oasis-open.org/odata/odata/v4.0/errata03/os/complete/part1-protocol/odata-v4.0-errata03-os-part1-protocol-complete.html#_Toc453752210) (shortened/modified list) the following changes do not require a change within a transformer:

- Adding an attribute that is nullable
- Adding an object to the model
- Adding a new complex type to the model
- Adding an option to an enumeration

Apart from this various changes require transformer adjustments as they break existing mappings/transformations. Thus, backward compatibility is not automatically given (in case the Open Integration Hub operator does not run both model versions in parallel).

The following list provides an overview of changes that require a transformer adjustment:

- Renaming an existing attribute
- Changing the type of an existing attribute
- Changing the properties of an existing attribute from _nullable_ to _not nullable_
- Deleting an existing attribute
- Renaming an existing object
- Deleting an existing object
- Adding an attribute that is not nullable

If a new version of a model is created, two options exist to create a transformer that is compatible with the newest model version:

1. The existing transformer is adjusted/updated according to the structure of the new data model.
2. A completely new transformer is built.
