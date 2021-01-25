---
layout: default
title: Working with OIH Images
nav_order: 5
parent: For Developers
---

# Working with the OIH Framework Images

## Monorepo Versions

The OIH Framework deploys images for each service to Docker Hub when each change is merged into the _master_ branch. These images are provided a version based on the services package version and the build from which it is deployed. (_ex: `0.2.3_32353`_ )

Starting with the first release of 2021, each image which is part of a release is tagged with a version matching the framework release. (ex: `2021.0.1`). It is recommended that only stable versions from releases be used for production deployments, and auto-generated images are only used for testing purposes.

## Declaring versions in deployments

When implementing deployments, it is recommended to create your own deployment files based on the example Kubernetes object files provided in the repository. Your custom deployments should then be updated to reference the desired image.

- In each /services/_SERVICE_NAME_/k8s/deployment/deployment.yaml file, the version should be explicitly provided as in `openintegrationhub/flow-repository:2021.0.1`
- It is not recommend to use the _latest_ tag when deploying images, because a newer image with unknown changes could be deployed when a Pod is restarted.

In case you need help or want to give us some feedback, head over to [GitHub](https://github.com/openintegrationhub/openintegrationhub/issues).
