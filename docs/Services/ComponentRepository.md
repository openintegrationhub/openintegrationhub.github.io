---
layout: default
title: Component Repository
parent: Services
nav_order: 4
---
<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->
![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# Component Repository

## Introduction
The Component Repository stores information about integration components such as adapters & transformer.

[API Reference](http://component-repository.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/component-repository){: .btn .fs-5 .mb-4 .mb-md-0 }
[Service File](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/component-repository){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used
- Node.js
- MongoDB

## How it works
<!-- describe core functionalities and underlying concepts in more detail -->

Integration components are lightweight and stand-alone Docker images that include everything needed to run the
component, including the component's code, a runtime, libraries and dependencies. Each component is based on an Open Integration Hub
[parent image](https://docs.docker.com/engine/userguide/eng-image/baseimages/) which provides the component runtime.
For example, for Java component the parent images provides the JDK and for Node.js component the parent image provides
[NPM](https://www.npmjs.com/) and [Node.js](https://nodejs.org).

The component images are stored in a [Docker Registry](https://docs.docker.com/registry/). A Docker Registry is a
stateless, highly scalable storage for Docker images. Any open source integration component can be store and
distributed in/from [Docker Cloud](https://cloud.docker.com) so that they would be available to any OIH installations
(cloud or on-prem) out of the box. For `private` components private Docker Registry can be maintained locally so that
no components are exposed to the cloud. Each on prem installation could decide whether to use private repos on Docker
Cloud or installing a private Docker Registry on prem for their private components.

### Interaction with other Services
- Interacts with IAM to introspect provided IAM token.
- Component Orchestrator get the information about components from this service.
