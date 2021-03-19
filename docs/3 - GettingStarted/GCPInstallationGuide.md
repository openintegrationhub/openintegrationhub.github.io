---
layout: default
title: GCP Installation
parent: Getting Started
nav_order: 2
---

# Google Cloud Platform Deployment Guide

The following guide helps to deploy the Open Integration Hub on the Google Cloud Platform.

- [Installation](#installation)
  - [Install Kubectl](#install-kubectl)
  - [Access to GCP K8s](#access-to-gcp-k8s)
  - [Basic Open Integration Hub Infrastructure Setup](#basic-open-integration-hub-infrastructure-setup)
  - [Setup Storage](#setup-storage)
  - [Identity and Access Management Deployment](#identity-and-access-management-deployment)
  - [Service Account Creation](#service-account-creation)
    - [Receive User Token](#receive-user-token)
    - [Login as Admin](#login-as-admin)
    - [Create a Service Account](#create-a-service-account)
    - [Create persistent Service Token](#create-persistent-service-token)
    - [Secret Creation](#secret-creation)
  - [Service Availability](#service-availability)
- [User Tutorial](#user-tutorial)
  - [Creating Components](#creating-components)
  - [Creating Flows](#creating-flows)
  - [Starting Flows](#starting-flows)
  - [Lessons Learned](#lessons-learned)
- [Notes](#notes)
  - [GCP Ingress Behavior](#gcp-ingress-behavior)

# Installation

## Install Kubectl

Make sure you have kubectl installed locally. Official guide: [Install Kubectl](https://kubernetes.io/de/docs/tasks/tools/install-kubectl/)

## Access to GCP K8s

Make sure you have created a project on gcp and receive the authentication information for your cluster:

1. Create project
2. In case no cluster exists: Create one
3. Get authentication information for the cluster

For detailed information see: [Kubernetes Engine - Quickstart](https://cloud.google.com/kubernetes-engine/docs/quickstart)

## Basic Open Integration Hub Infrastructure Setup

**Please make sure to clone the [monorepo](https://github.com/openintegrationhub/openintegrationhub) before you start. You will need the files in the minikube folder.**

Set up the basic Open Integration Hub infrastructure.
If you want to change you namespace in the `namespace.yaml`, you also need to adjust the namespace in each service.yaml and deployment.yaml.

Execute the following commands:

1. `kubectl apply -f platform/namespace.yaml`
2. `kubectl apply -f platform/rabbitmq.yaml`
3. `kubectl apply -f platform/redis.yaml`

## Setup Storage

Next, you need to make sure to setup / connect to a storage solution such as [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
Optionally, the mongodb.yaml provided under `platform/mongodb.yaml` could be used. To do so:

- apply minikube/1-Platform/volume.yaml
- apply minikube/1-Platform/volumeClaim.yaml
- apply minikube/1-Platform/platform/mongodb.yaml

Note: _We suggest to use the first variant i.e. an external storage solution_

Irrespective of the solution you choose, ensure that each service uses its own database. This is necessary to ensure proper encapsulation and to avoid accidental data pollution. For more information about MongoDB databases, please refer to the official [MongoDB Documentation](https://docs.mongodb.com/manual/core/databases-and-collections/).

## Identity and Access Management Deployment

Deploy the Open Integration Hub Identity and Access Management (IAM). To do so, simply create a secret for IAM:

- Create a temporary file (**find an example secret below**)
  - Set `mongourl` to the connection string (base64 encoded) of your storage solution
  - Optionally set the other values of the other keys. Standard value: 'somestring'
- Execute `kubectl apply -f YOUR_TEMPORARY_FILE`
- Execute `kubectl apply -f services/iam/k8s`

Before you apply ingress for IAM also make sure to replace host entries:

E.g for [iam](https://github.com/openintegrationhub/openintegrationhub/blob/master/platform/ingress.yaml#L11):

```yaml
host: iam.openintegrationhub.com
```

Next, comment out all services except for IAM in the `platform/ingress.yaml` file, and apply:

`kubectl apply -f platform/ingress.yaml`

(for more information on staggered ingress deployment see the note on [GCP Ingress Behavior](#gcp-ingress-behavior))

Wait until the service is fully deployed and ready. Afterwards, check if iam is existent on your cluster.

Example Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: iam
  namespace: oih-dev-ns
type: Opaque
data:
  mongourl: 'bW9uZ29kYjovL21vbmdvZGItc2VydmljZS5vaWgtZGV2LW5zLnN2Yy5jbHVzdGVyLmxvY2FsL2lhbQ=='
  jwtsecret: 'c29tZXN0cmluZw=='
  cookiesecret: 'c29tZXN0cmluZw=='
  admin_password: 'c29tZXN0cmluZw=='
  serviceacc_password: 'c29tZXN0cmluZw=='
  oidc_client_secret: 'c29tZXN0cmluZw=='
```

## Service Account Creation

Create a service account and token for all of the services listed below.

- Attachment-Storage-Service
- Audit-Log
- Component-Orchestrator
- Component-Repository
- DataHub
- Dispatcher
- Flow-Repository
- MetaData-Repository
- Secret-Service

Use Postman (or another similar tool of your choice), to send these POST requests the IAM.

### Receive User Token

First, you have to request a user token (for admin account). This step only need to be perfomed once.

**Base URL:** `IAM_BASE_URL` (received from ingress)

**Header:** `Content-Type: application/json`:

### Login as Admin

_Path_:

`/login`

_Request Body:_

If you haven't changed the `admin_password` in the secret you created a few steps ago, you can use the following json. Otherwise replace the password with the new value.

```json
{
  "username": "admin@openintegrationhub.com",
  "password": "somestring"
}
```

_Response Body Structure:_

```json
{
  "token": "string"
}
```

Use the returned `token` as a Bearer token for the remaining requests.

### Create a Service Account

After you received the user token, a service account must be created for each service listed at the [beginning](#service-account-creation) of this chapter.

Expect for `component orchestrator`, each service needs at least the following permission:

- `iam.token.introspect`

Component Orchestrator needs the following permissions:

- Component Orchestrator: `components.any.read`, `iam.token.create`, `iam.token.delete`

Replace the following values with a value of your choice:

- `firstname`
- `lastname`
- `password`

_Path:_

`/api/v1/users`

_Request Body:_

```json
{
  "username": "servicename@serviceaccount.de",
  "firstname": "a",
  "lastname": "b",
  "role": "SERVICE_ACCOUNT",
  "status": "ACTIVE",
  "password": "asd",
  "permissions": [
    "iam.token.introspect",
    "components.get"
  ]
}
```

_Response Body Structure:_

```json
{
  "id": "string",
  "username": "string",
  "firstname": "string",
  "lastname": "string",
  "status": "active",
  "tenant": "string",
  "roles": [
    {
      "name": "string",
      "permissions": ["all"],
      "scope": "string"
    }
  ],
  "permissions": ["all"],
  "confirmed": true,
  "img": "string"
}
```

The returned `id` is later needed to create a service token.

### Create persistent Service Token

_Path:_

`/api/v1/tokens`

_Request Body:_

```json
{
  "accountId": "PASTE SERVICE ACCOUNT ID HERE",
  "expiresIn": -1
}
```

The returned token is the service token that will be used by the other services to authenticate themselves to the IAM. Copy the value, encode it in _base64_ (for encoding you can use online tools such as: <https://www.base64encode.org/>), and then paste it into the secret files for each of the services listed at the [beginning](#service-account-creation) of this chapter.

### Secret Creation

For each services listed in `./services` a secret file is needed. Thus, the following steps need to be performed for every service:

- Replace `metadata.name` with the current service name
- `Data` must include all `secretKeyRef`s from the `./k8s/deployment.yaml` of each service. E.g. flow repository: [flow-repository deployment.yaml](https://github.com/openintegrationhub/openintegrationhub/blob/master/services/flow-repository/k8s/deployment.yaml#L20-L29)
- For each service that was listed at the [beginning](#service-account-creation) of this chapter make sure to add the persistent token as the value for the **iamtoken**.

Example secret file for **flow-repository**:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: flow-repository
  namespace: oih-dev-ns
type: Opaque
data:
  mongourl: CONNECTION_URL
  iamtoken: IAM TOKEN
```

## Deploying Services

Once you created the secret files, execute the following commands for each service:

1. Execute `kubectl apply -f YOUR_TEMPORARY_FILE`
2. Execute `kubectl apply -f services/CURRENT_SERVICE/k8s/service.yaml`
3. Execute `kubectl apply -f services/CURRENT_SERVICE/k8s/deployment.yaml`

After deploying your services, update your GCP Ingress to include the new services. Before you apply ingress, make sure to replace host entries to match your external domain:

E.g for [iam](https://github.com/openintegrationhub/openintegrationhub/blob/master/platform/ingress.yaml#L11):

```yaml
host: iam.openintegrationhub.com
```

Comment out all non-deployed services in the `platform/ingress.yaml` file, and apply:

`kubectl apply -f platform/ingress.yaml`

(for more information on staggered ingress deployment see the note on [GCP Ingress Behavior](#gcp-ingress-behavior))

## Service Availability

The Open Integration Hub is now running and ought to function just as it would in an online context. You can reach the various services via the following URLS:

- **Identity and Access Management**. Create and modify users, tenants, roles, and permissions.
  - `YOUR IAM URL`
- **Secret Service**. Securely store authentication data for other applications.
  - `YOUR Secret-Service URL`
- **Flow Repository**. Create, modify, and start/stop integration flows.
  - `YOUR Flow-Repository URL`
- **Audit Log**. View event logs spawned by the other services.
  - `YOUR Audit-Log URL`
- **Metadata Repository**. Create and modify master data models used by your connectors.
  - `YOUR Meta-Data-Repository URL`
- **Component Repository**. Store and modify connector components.
  - `YOUR Component-Repository URL`
- **Attachment Storage Service**. Temporarily store larger files for easier handling in flows.
  - `YOUR Attachment-Storage-Service URL`
- **Data Hub**. Long-term storage for flow content.
  - `YOUR Data-Hub URL`
- **Integration Layer Service**. Perform data operations such as merging or splitting objects.
  - `YOUR Integration-Layer-Service URL`
- **Web UI**. A basic browser-based UI to control certain other services.
  - `YOUR Web-UI URL`

Most of these services have an OpenAPI documentation of their API available through the path `/api-docs`. You can also check the [API Reference Documentation](https://openintegrationhub.github.io/docs/6%20-%20ReferenceDocumentation/APIReferenceOverview.html). If you want to learn more about the services, check the [Service Documentation](https://openintegrationhub.github.io/docs/5%20-%20Services/Services.html) or their readmes in the `services` folder of the GitHub Repository: [Open Integration Hub Services](https://github.com/openintegrationhub/openintegrationhub/tree/master/services)

# Notes

## GCP Ingress Behavior

Using Google Kubernetes Engine creating an Ingress additionally creates a Load Balancer and its related Backend Services. Each Backend Service also has a Healthcheck created and assigned, which pings the root endpoint / of Pods to see if they are alive. This Healthcheck is used to determine if a pod is in a Healthy state and can have traffic sent to it.

When a Healthcheck is created, it will use the Readiness and Liveness probes from applicable Deployments, but only if they have already been applied to the Cluster. After creation, the Healthcheck does not track any changes to Deployments, so will not pick up the probe values later.

# User Guide

After you deployed your own Open Integration Hub instance, check out the User Guide. You will learn how to setup your frist example flow using generic integration components.

<div class="oih-docs-learn-overview-container">
<div class="container-further">
    <a class="item" href="https://openintegrationhub.github.io/docs/3%20-%20GettingStarted/UserGuide.html">User Guide</a>
</div>
</div>
