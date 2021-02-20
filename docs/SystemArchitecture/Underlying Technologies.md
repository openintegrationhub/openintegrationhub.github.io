---
layout: default
title: Underlying Technologies
parent: System Architecture
nav_order: 1
---

# The Open Integration Hub TechStack

The Framework is meant to be as flexible as possible and therefore takes a generalized approach. This will mean that your individual setup may require a different set of tools or services. Either because you already use them or because you have requirements that cannot be met by the default. We made some technological decision for deployment as well as certain functionality. Generally speaking, we tried not to develop everything from scratch, if we already found a fitting solution. We always opted for technologies that are open source, lightweight to ensure easy entry and fairly accepted by the developer community.

As you know, technology changes quickly. You may find better tools out there. Or you may need more for your individual setup. Please tweak the framework to your liking and make suggestions to improve the framework for everyone.

The more general choice of using [node.js](https://nodejs.org/en/) for the core development, standard formats like [JSON](https://www.json.org/json-en.html) or [OpenAPI](http://spec.openapis.org/oas/v3.0.3) are of course more or less set.
Some important technologies you may want to change are:

### RabbitMQ

Queues are at the core of any interaction within the system. They are used to push objects from one integration component to another, but also for the services to exchange events. [RabbitMQ](https://www.rabbitmq.com/) is therefore quite broadly used, for instance in our so called [Message Oriented Middleware]({{ site.baseurl }}{% link  docs/Services/MessageOrientedMiddleware.md %}). Check out the Basic Concepts to better understand what role it plays in the current architecutre.

### MongoDB

Even though the framework is about transmitting data, many services also need to store data. This can either be related to the objects you are piping through (i.e. for ID Linking) or to store descriptions of your integrations flow or components. [MongoDB](https://www.mongodb.com/de) is the default data base for all services.

### Docker Hub

All services and components contain dockerfiles and their images are provided via Docker Hub. Having an publicly available Docker Registry makes it easier to share images with the community. Services like the Component Repository also hold references to these images. Open Integration Hub is also an official Docker Partner.

### CircleCI

[CircleCI](https://circleci.com/) is used for the development process for the open source framework. It is mainly to ensure quality of the committed code and services for the community. For your own deployment you are of course free to use whatever you prefer. In case you want to contribute to the open source code base, be aware of the automated tests we perform via CircleCI.

<!-- ### Minikube -->

<!-- ### DockerCompose -->

### Google Cloud Platform

There are many ways and platforms to run your kubernetes cluster. [Google Cloud Platform](https://cloud.google.com/) has the most usage in the Open Integration Hub community, which is why we run our test cluster on this platform and also provide a guide on how to set up your own instance on GCP. You may find some GCP specific configurations or decisions. We do try to keep the dependency as low as possible though, so you can deploy the framework on any other platform as well. We are open for contributions of best practises and guides for other vendors -> [GitHub](https://github.com/openintegrationhub/openintegrationhub.github.io).

### A word on IAM

When we started developing the framework, we did not find an Identity and Access Management solution really fitting our needs. We therefore decided to build our own. There are of course plenty of other options out there and often times, you already use one, if you have an exitsing application. It also heavily depends on the platform you like to use to run your system. It is not manditory to use the IAM provided by us. Especially in conjunction with the so called Secret Service, it is however central for the framework and therefore will take some effort to exchange for another solution.
