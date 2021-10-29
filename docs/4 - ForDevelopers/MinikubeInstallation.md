---
layout: default
title: Minikube Development
nav_order: 3
parent: For Developers
---

# Minikube Development Guide

The minikube-based development setup enables users to bring up a local implementation of the OIH Framework based on the current service images found on Docker Hub, while individually selecting which services they would like to deploy from their own local source folders.
![linux](https://img.shields.io/badge/Linux-red.svg) ![Windows](https://img.shields.io/badge/Windows-blue.svg) ![Mac](https://img.shields.io/badge/Mac-green.svg)

- [Requirements](#requirements)
- [Configuration](#configuration)
- [Usage](#usage)

# Requirements

**Please make sure to clone the [monorepo](https://github.com/openintegrationhub/openintegrationhub) before you start.**

The development environment requires:

- minikube >= 1.23.0
- kubectl
- python3
- curl
- base64

Make sure that minikube is endowed with sufficient resources. We suggest at least:

- _8GB of memory_
- _4 CPUs_

The setup script will attempt to provision the minikube instance with these values by default on execution. This can be changed by altering the variables _MK_MEMORY_ and _MK_CPUS_.

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

If you're using Windows we suggest to use virtual box. In order to use it, Hyper-V must be disabled <a href="https://docs.microsoft.com/de-de/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v">Enable/Disable Hyper-V on Windows 10.</a>.You may also have to enable virtualisation features in you BIOS.

</div>
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

If you're using Docker for Desktop it overwrites the acutal kubectl version. This version is generally not compatible with minikube. There are two options to correct this:

<ul>
<li> Download the `kubectl.exe` from <a href="https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows">Install kubectl on Windows</a>. Navigate to the docker directory (e.g. Program Files\Docker\Docker\resources\bin) andreplace the kubectl.exe in this folder with the one you just downloaded.</li>
<li> Use the "Edge" version of Docker Desktop. This can be done by installing the edge version of the application from the <a href="https://docs.docker.com/desktop/">Docker Desktop site</a>. If you already have Docker Desktop installed, you can switch to the Edge version from the Docker menu. Select <b>Preferences > Command Line</b> and then activate the <b>Enable experimental features</b> toggle. After selecting <b>Apply & Restart</b>, Docker will update versions. More information can be found <a href="https://docs.docker.com/docker-for-mac/install/#switch-between-stable-and-edge-versions">here</a>.</li>
</ul>
</div>
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

The OIH Framework requires the <i>ingress</i> addon for kubernetes. This is not supported via Docker Bridge for Mac and Windows on older versions of minikube. Therefore, minikube must be at a minimum version of 1.23.0. For older versions, you can refer to previous releases of the OIH framework(< 21.2.0) for the appropriate setup script. More information can be found on the <a href="https://github.com/kubernetes/minikube/issues/7332">minikube Github page</a>.

</div>

# Configuration

Before running the setup script, the location of your NFS server must be updated in ./1-Platform/2.1-sourceCodeVolume.yaml. The server host is the IP address which minikube uses to access the host. In order to verify your server address, you will need to start minikube and then execute the following command: `minikube ssh grep host.minikube.internal /etc/hosts | cut -f1`

The path is the path to the root of your cloned repository / base path of the framework

```
nfs:
    server: 192.168.64.1
    path: '/Users/user/projects/openintegrationhub'
```

# Usage

From the $OIH_ROOT/dev-tools/minikube folder, call the setup script

```
bash ./setup.sh
```

The script will check for prerequisites, and attempt to build all services. The script automates the steps found in the minikube Local Installation Guide.

Additionally, the following options may be sent in the arguments to `setup.sh`.

- `-c`: Clear Minikube. The minikube cluster will be deleted and rebuilt from scratch.

- `-s`: Skip Deployment. Any service names which are provided as arguments to this flag will not be included in the running cluster. They will be temporarily deployed to ensure all dependencies are met, then deleted at the end of startup.

  > Example: `bash ./setup.sh -s meta-data-repository, snapshots-service`

- `-d`: Development Mode. Any service names which are provided as arguments to this flag will be deployed from the local source files. Instead of being deployed from the Docker Hub containers, a generic node container will be deployed, and the source directory will be mounted through NFS. Changes to the source files will be watched and reflected in the running containers.

  > Example: `bash ./setup.sh -d iam, component-orchestrator`

- `-i`: Use Custom Component Image. This setup automatically deploys a development connector and includes it in sample flows. To change the container which is used for this connector, include an image name here. (Default image: `openintegrationhub/dev-connector:latest`)

- `-p`: Start Proxy. If this flag is set, then `kubectl` will set up proxy connections to Mongo, RabbitMQ, and Redis for use in debugging backend and messaging systems.
