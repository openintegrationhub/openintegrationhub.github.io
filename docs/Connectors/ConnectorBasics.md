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

The Open Integration Hub enables data synchronization across a variety of applications. To enable interaction with any software an technical component is needed to the Open Integration Hub - the connector. It consists of two distinct parts: the adapter and the transformer.

An **adapter** is a module for the syntactic connection of an external application and its data to the Open Integration Hub. This includes protocol translation, data format transformation, etc.
Furthermore it provides functionalities to perform e.g. CRUD operations within the source system.

A **transformer** is responsible to semantically transform an incoming JSON object into another JSON object. Thus the mapping between two data models is done within the transformer.

The following illustration provides a holistic overview of a connector:
![Connector](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ConnectorsV3.png)

## Open Source Connectors

Like the Open Integration Hub services, connectors are also standardized components that can be reused in any implementation of the framework. There are several contributors that provide a wide range of open source connectors already. So before you start your own, check out what's already there:

[Open Integration Hub](https://github.com/openintegrationhub){: .btn .fs-5 .mb-4 .mb-md-0 }

For a quick start in developing your own connector, we offer a pair templates to start out with. These contain the basic necessary structure and functions for functionality. While these are specific to the contact data domain, they can easily be adjusted or expanded to suit any other type of data or application.

[Contacts Adapter Template](https://github.com/openintegrationhub/contacts-adapter-template)

[Contacts Transformer Template](https://github.com/openintegrationhub/contacts-transformer-template)

<!-- If you want to build your own connector, we suggest you start with a our node.js example, to understand the structure and what you need to get going. Most components are build in node.js, although you can choose any language you want.

[node.js example](https://openintegrationhub.github.io//docs/Connectors/building-nodejs-component.html) -->

If you prefer a real world example, the Snazzy Contacts or Wice components are good place to get inspiration.

[Snazzy Contacts Adapter](https://github.com/openintegrationhub/snazzycontacts-adapter)

[Snazzy Contacts Transformer](https://github.com/openintegrationhubsnazzycontacts-transformer)

[Wice Adapter](https://github.com/openintegrationhub/wicecrm-adapter)

[Wice Transformer](https://github.com/openintegrationhub/wicecrm-transformer)
