---
layout: default
title: Local Installation
parent: Getting Started
nav_order: 1
---

# Local Installation Guide

![linux](https://img.shields.io/badge/Linux-red.svg) ![Windows](https://img.shields.io/badge/Windows-blue.svg) ![Mac](https://img.shields.io/badge/Mac-green.svg)

In addition to setting up the Open Integration Hub on a cloud infrastructure such as GCP it is also possible to setup a local version of the framework. Make sure to perform the following to set up a local version of the OIH within your own minikube:

- [Requirements](#requirements)
- [Installation](#installation)
  - [Install Minikube](#install-minikube)
  - [Basic Open Integration Hub Infrastructure Setup](#basic-open-integration-hub-infrastructure-setup)
  - [Host Rules Setup](#host-rules-setup)
  - [Identity and Access Management Deployment](#identity-and-access-management-deployment)
  - [Service Account Creation](#service-account-creation)
    - [Login as Admin](#login-as-admin)
    - [Create a Service Account](#create-a-service-account)
    - [Create persistent Service Token](#create-persistent-service-token)
  - [Shared Secret Application](#shared-secret-application)
  - [Service Deployment](#service-deployment)
  - [Service Availability](#service-availability)
- [Usage](#usage)
  - [Creating Components](#creating-components)
  - [Creating Flows](#creating-flows)
  - [Starting Flows](#starting-flows)
  - [Lessons Learned](#lessons-learned)

# Development Environments

Besides the basic installation guide found here, the OIH monorepo provides two possible setups for a development environment. They are found in the `dev-tools/` folder in the monorepo.

- The [minikube instructions](https://openintegrationhub.github.io/docs/4%20-%20ForDevelopers/MinikubeInstallation.html) are based off of the basic local minikube installation found here. It adds the ability to optionally launch each service using local source code served over NFS instead of the public Docker image.
- The [docker-compose installation](https://openintegrationhub.github.io/docs/4%20-%20ForDevelopers/DockerInstallation.html) contains configuration files and helper scripts to run local services directly from Docker. It still relies on minikube to execute flows.

# Requirements

**Please make sure to clone the [monorepo](https://github.com/openintegrationhub/openintegrationhub) before you start.**

Make sure that minikube is endowed with sufficient resources. We suggest at least:

- _8GB of memory_
- _4 CPUs_

<div style="
    margin: 10px 0px;
    background: #f8f8f8;
    padding: 10px;
    border-radius: 3px;
    font-size: 1em;
    border: 1px solid #9c9c9c;">
    <div style="float: left; margin-right: 10px;">
<img src="https://img.shields.io/badge/Windows-blue.svg" height="30">
</div>
<hr style="margin-bottom:1rem;"/>
If you're using Windows we suggest to use virtual box. In order to use it, Hyper-V must be disabled <a href="https://docs.microsoft.com/de-de/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v">Enable/Disable Hyper-V on Windows 10.</a> You may also have to enable virtualisation features in your BIOS.
</div>

# Installation

## Install Minikube

Make sure minikube is installed, configured, and started. The command for allocating sufficient resources is

    minikube start --memory 8192 --cpus 4

<div style="
    margin: 10px 0px;
    background: #f8f8f8;
    padding: 10px;
    border-radius: 3px;
    font-size: 1em;
    border: 1px solid #9c9c9c;">

<div style="float: left; margin-right: 10px;">
    <img src="https://img.shields.io/badge/Windows-blue.svg" height="30">
    <img src="https://img.shields.io/badge/Mac-green.svg" height="30">
</div>
<hr style="margin-bottom:1rem;"/>
The OIH Framework requires the <i>ingress</i> addon for kubernetes. This is not supported via Docker Bridge for Mac and Windows. Therefore, on these Operating Systems, minikube must be started with the flag `--vm=true`. More information can be found on the <a href="https://github.com/kubernetes/minikube/issues/7332">minikube Github page</a>.
</div>

If you already have an installed minikube instance that is using the virtualbox driver you can do

    minikube stop

and then

    VBoxManage modifyvm "minikube" --memory 8192 --cpus 4

to adjust the resource limits before starting again.

In particular, ensure that its ingress module is enabled (`minikube addons enable ingress`). Also make sure that `kubectl` is configured to use minikube. To see if its correctly configured use

    `kubectl config current-context
    or
    cluster info`

For further information about how to set up minikube, see here:

- [Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [Installing Kubernetes with Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/)

<div style="
    margin: 10px 0px;
    background: #f8f8f8;
    padding: 10px;
    border-radius: 3px;
    font-size: 1em;
    border: 1px solid #9c9c9c;">
    <div style="float: left; margin-right: 10px;">
<img src="https://img.shields.io/badge/Windows-blue.svg" height="30">
<img src="https://img.shields.io/badge/Mac-green.svg" height="30">
</div>
<hr style="margin-bottom:1rem;"/>
If you're using Docker for Desktop it overwrites the acutal kubectl version. This version is generally not compatible with minikube. There are two options to correct this:
<ul>
<li> Download the `kubectl.exe` from <a href="https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows">Install kubectl on Windows</a>. Navigate to the docker directory (e.g. Program Files\Docker\Docker\resources\bin) andreplace the kubectl.exe in this folder with the one you just downloaded.</li>
<li> Use the "Edge" version of Docker Desktop. This can be done by installing the edge version of the application from the <a href="https://docs.docker.com/desktop/">Docker Desktop site</a>. If you already have Docker Desktop installed, you can switch to the Edge version from the Docker menu. Select <b>Preferences > Command Line</b> and then activate the <b>Enable experimental features</b> toggle. After selecting <b>Apply & Restart</b>, Docker will update versions. More information can be found <a href="https://docs.docker.com/docker-for-mac/install/#switch-between-stable-and-edge-versions">here</a>.</li>
</ul>
</div>

## Basic Open Integration Hub Infrastructure Setup

**Please make sure to clone the [monorepo](https://github.com/openintegrationhub/openintegrationhub) before you start. You will need the files in the minikube folder.**

Set up the basic Open Integration Hub infrastructure. To do this, simply execute

`kubectl apply -f ./1-Platform`

This may take a while to finish. You can use `minikube dashboard` to check the status of the various deployments. Once they are all ready, you can proceed.

## Host Rules Setup

To actually reach the services, you need to add an entry in your hosts file for each service. You can retrieve the IP with `minikube ip` and need to create an entry for each host listed in the `ingress.yaml` file (e.g. `iam.localoih.com`).
If you are using...

a **Linux** distribution, you can automate this by using this terminal command:

```console
echo "$(minikube ip) app-directory.localoih.com iam.localoih.com skm.localoih.com flow-repository.localoih.com auditlog.localoih.com metadata.localoih.com component-repository.localoih.com dispatcher-service.localoih.com webhooks.localoih.com attachment-storage-service.localoih.com data-hub.localoih.com ils.localoih.com web-ui.localoih.com" | sudo tee -a /etc/hosts
```

a **Windows** distribution, you can find the host files under:

```console
c:\windows\system32\etc\hosts
or
c:\windows\system32\drivers\etc\hosts

then add

your_minikube_ip app-directory.localoih.com
your_minikube_ip iam.localoih.com
your_minikube_ip skm.localoih.com
your_minikube_ip flow-repository.localoih.com
your_minikube_ip auditlog.localoih.com
your_minikube_ip dispatcher-service.localoih.com
your_minikube_ip metadata.localoih.com
your_minikube_ip component-repository.localoih.com
your_minikube_ip webhooks.localoih.com
your_minikube_ip attachment-storage-service.localoih.com
your_minikube_ip data-hub.localoih.com
your_minikube_ip ils.localoih.com
your_minikube_ip web-ui.localoih.com
```

## Identity and Access Management Deployment

Deploy the OIH Identity and Access Management. To do so, simply execute `kubectl apply -f ./2-IAM`. Again, wait until the service is fully deployed and ready.

## Service Account Creation

Create a service account and token for the other services in the IAM. Using Postman (or another similar tool of choice), send these POST requests to the IAM.

**Base URL:** `iam.localoih.com`

**Header:** `Content-Type: application/json`:

### Login as Admin

_Path_:

`/login`

_Request Body:_

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

_Path:_

`/api/v1/users`

_Request Body:_

```json
{
  "username": "test@test.de",
  "firstname": "a",
  "lastname": "b",
  "role": "SERVICE_ACCOUNT",
  "status": "ACTIVE",
  "password": "asd",
  "permissions": ["all"]
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

Use the returned `id` in the following request to create the token.

### Create persistent Service Token

_Path:_

`/api/v1/tokens`

_Request Body:_

```json
{
  "accountId": "PASTE SERVICE ACCOUNT ID HERE",
  "expiresIn": -1,
  "initiator": "PASTE SERVICE ACCOUNT ID HERE",
  "inquirer": "PASTE SERVICE ACCOUNT ID HERE"
}
```

The returned token is the service token that will be used by the other services to authenticate themselves to the IAM. Copy the value, encode it in _base64_ (for encoding you can use online tools such as: <https://www.base64encode.org/>), and then past it into the file found at `./3-Secret/SharedSecret.yaml` at the indicated position (`REPLACE ME`).

## Shared Secret Application

Apply the shared secret via

    kubectl apply -f ./3-Secret.

Ordinarily, each service would have its own secret for security reasons, but this is simplified for ease of use in a local context

## Service Deployment

Deploy the remaining services via the following command. This may take a while.

    kubectl apply -Rf ./4-Services

## Service Availability

The Open Integration Hub is now running and ought to function just as it would in an online context. You can reach the various services via the following URLS:

- **Identity and Access Management**. Create and modify users, tenants, roles, and permissions.
  - `iam.localoih.com`
- **Secret Service**. Securely store authentication data for other applications.
  - `skm.localoih.com`
- **Flow Repository**. Create, modify, and start/stop integration flows.
  - `flow-repository.localoih.com`
- **Audit Log**. View event logs spawned by the other services.
  - `auditlog.localoih.com`
- **Metadata Repository**. Create and modify master data models used by your connectors.
  - `metadata.localoih.com`
- **Component Repository**. Store and modify connector components.
  - `component-repository.localoih.com`
- **Attachment Storage Service**. Temporarily store larger files for easier handling in flows.
  - `attachment-storage-service.localoih.com`
- **Data Hub**. Long-term storage for flow content.
  - `data-hub.localoih.com`
- **Integration Layer Service**. Perform data operations such as merging or splitting objects.
  - `ils.localoih.com`
- **Web UI**. A basic browser-based UI to control certain other services.
  - `web-ui.localoih.com`

Most of these services have an OpenAPI documentation of their API available through the path `/api-docs`. You can also check the [API Reference Documentation](https://openintegrationhub.github.io/docs/6%20-%20ReferenceDocumentation/APIReferenceOverview.html). If you want to learn more about the services, check the [Service Documentation](https://openintegrationhub.github.io/docs/5%20-%20Services/Services.html) or their readmes in the `services` folder of the GitHub Repository: [Open Integration Hub Services](https://github.com/openintegrationhub/openintegrationhub/tree/master/services)
