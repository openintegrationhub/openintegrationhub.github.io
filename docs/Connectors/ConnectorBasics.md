---
layout: default
title: Connectors
nav_order: 6
has_children: true
---

<p align="center">
  <img src="https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/large-oih-vertikal-zentriert.png" alt="Open Integration Hub" width="300"/>
</p>
<br>
<br>

# Connector Guidelines

The Open Integration Hub enables data synchronization across a variety of applications. To create a connection and enable interaction a link is needed between the software application and the Open Integration Hub - the Open Integration Hub connector.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->
<!-- /TOC -->

A connector connects a software solution to the Open Integration Hub. It consists of two distinct parts, namely adapter and transformer.  It contains different functionalities e.g. to fetch and transform data. These functionalities are further explained in the sections [adapter](#adapter) and [transformer](#transformer). In order to achieve our goal to establish a successful open source community we need to steadily increase the number of connectors. So join us and help us grow as an open source community!


The following illustration provides a holistic overview of a connector:
![Connector](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ConnectorsV3.png)

### Adapter

An adapter is a module for the syntactic connection of an external application and its data to the Open Integration Hub. This includes protocol translation, data format transformation, etc.
Furthermore it provides functionalities to perform e.g. CRUD operations within the source system.

For further information please read through the information within the [adapter folder](https://openintegrationhub.github.io//docs/Connectors/Adapter.html).


### Transformer

A transformer is responsible to semantically transform an incoming JSON object into another JSON object. Thus the mapping between two data models is done within the transformer.

For further information please read through the information within the [transformer folder](https://openintegrationhub.github.io//docs/Connectors/Transformer.html).
