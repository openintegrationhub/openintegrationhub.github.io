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

The Open Integration Hub enables data synchronization across a variety of applications. To achive this goal we are using Components which are implemented individualy for each API. The Components
are set to each and every step of data syncronization flow and speak to each other using the tranformation model functions from the new Ferryman library from OIH.  

A **Component** is the result of the union of the connector along with the transformation functions that are provided through the new Ferryman. It is the initialiser and translator of the data 
that comes in and goes out and it is assigned as a whole step in a flow.

An **connector** is a module for the syntactic connection of an external application and its data to the Open Integration Hub. This includes protocol translation, data format transformation, etc.
Furthermore it provides functionalities to perform e.g. CRUD operations within the source system through the actions and the triggers that are provided to each individual connector.

A **transform function or tranformer in previous releases** is responsible to semantically transform an incoming JSON object into another JSON object. The functionality is implemented using the 
new version of ferryman and importing the necessary expressions into each connector. Therefore we save some steps from a flow while we still give the same functionality. 



The following illustration provides a holistic overview of a Component:
![Connector](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ConnectorsV3.png)

## Open Source Components

Like the Open Integration Hub services, components are also standardized that can be reused in any implementation of the framework. There are several contributors that provide a wide range of open source components already. So before you start your own, check out what's already there:

[Open Integration Hub](https://github.com/openintegrationhub){: .btn .fs-5 .mb-4 .mb-md-0 }

For a quick start in developing your own connector, we offer a pair templates to start out with. These contain the basic necessary structure and functions for functionality. While these are specific to the contact data domain, they can easily be adjusted or expanded to suit any other type of data or application.

[Contacts Component Template](https://github.com/openintegrationhub/contacts-adapter-template)

<!-- [Contacts Transformer Template](https://github.com/openintegrationhub/contacts-transformer-template) -->

<!-- If you want to build your own connector, we suggest you start with a our node.js example, to understand the structure and what you need to get going. Most components are build in node.js, although you can choose any language you want.

[node.js example](https://openintegrationhub.github.io//docs/Connectors/building-nodejs-component.html) -->

If you prefer a real world example, the Snazzy Contacts and Wice components are a good place to get inspiration.

[Snazzy Contacts Component](https://github.com/openintegrationhub/snazzycontacts-adapter)

<!-- [Snazzy Contacts Transformer](https://github.com/openintegrationhubsnazzycontacts-transformer) -->

[Wice Component](https://github.com/openintegrationhub/wicecrm-adapter)

<!-- [Wice Transformer](https://github.com/openintegrationhub/wicecrm-transformer) -->
