---
layout: default
title: Integration Components
parent: Basic Concepts
nav_order: 1
---

# Components Basics

## Overview

The Open Integration Hub enables data synchronization across a variety of applications. To achieve this goal we are using modular Components which are implemented individually for each API or desired function. These components are capable of passing data from one to another, allowing you to organize them in user-defined flows to pass data between systems with ease.

A **Component** is a lightweight assortment of functions packaged as a [Docker image](https://docs.docker.com/). The OIH runs these images as containerized applications, pass data into them, and execute the component's provided functions. While each component requires certain dependencies and format to be compatible with the OIH, they can contain and execute essentially arbitrary code, allowing you to cover a wide array of use-cases.

A **Connector** is our term for a particular kind of component, and generally the most common one. It's a component specifically created for communicating with the API of an external system. They are most useful in the case of data transmission or synchronization. You can use a connector built for one system to fetch data from it, and then use a connector for another system to push that data into that system.

A **Transform Function** is responsible to semantically transform an incoming JSON object into another JSON object. In order for Connectors to effectively communicate with one another, they need to transform that data into a shared master data format before passing it on. This can be done as part of the Component's innate functions, or by using provided OIH helper functions to transform it on the fly using [JSONata](https://jsonata.org/)

The following illustration provides a holistic overview of a Component:
![Connector](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ConnectorsV3.png)

## Deployment and Communication

Whenever an OIH component is started, the OIH will fetch the most recent version of its image from its designated docker repository. It is then deployed as a [kubernetes deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) in the same cluster as the OIH itself. A single such deployment of a Component can handle data from several sources at once, allowing for a number of simultaneous users while maintaining a low resource footprint.

Communication between Components is handled through the OIH infrastructure. The OIH offers a standardized communication interface to Components, allowing any one Component to be able to natively communicate with any other Component. The direction and content of this communication is controlled by Flows, which are described further in [their own section](https://openintegrationhub.github.io/docs/1%20-%20BasicConcepts/FlowBasics.html).

## Open Source Components

Like the Open Integration Hub services, components are also standardized that can be reused in any implementation of the framework. There are several contributors that provide a wide range of open source components already. So before you start your own, check out what's already there:

[Open Integration Hub](https://github.com/openintegrationhub){: .btn .fs-5 .mb-4 .mb-md-0 }

For a quick start in developing your own connector, we offer a pair templates to start out with. These contain the basic necessary structure and functions for functionality. While these are specific to the contact data domain, they can easily be adjusted or expanded to suit any other type of data or application.

[Contacts Component Template](https://github.com/openintegrationhub/contacts-adapter-template)

<!-- [Contacts Transformer Template](https://github.com/openintegrationhub/contacts-transformer-template) -->

<!-- If you want to build your own connector, we suggest you start with a our node.js example, to understand the structure and what you need to get going. Most components are build in node.js, although you can choose any language you want.

[node.js example](https://openintegrationhub.github.io//docs/Connectors/building-nodejs-component.html) -->

If you prefer a real world example, the Snazzy Contacts and Wice connectors are a good place to get inspiration.

[Snazzy Contacts Connector](https://github.com/openintegrationhub/snazzycontacts-adapter)

<!-- [Snazzy Contacts Transformer](https://github.com/openintegrationhubsnazzycontacts-transformer) -->

[Wice Connector](https://github.com/openintegrationhub/wicecrm-adapter)

<!-- [Wice Transformer](https://github.com/openintegrationhub/wicecrm-transformer) -->
