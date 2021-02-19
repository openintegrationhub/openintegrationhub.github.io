---
layout: default
title: Secret Service
parent: Services
nav_order: 15
---

<!-- Description Guidelines

Please note:
Use the full links to reference other files or images! Relative links will not work under our theme settings settings.
-->

<!-- please choose the appropriate batch and delete/comment the others  -->

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Secret Service** <!-- make sure spelling is consistent with other sources and within this document -->

## Introduction

<!-- 2 sentences: what does it do and how -->

The Secret Service is used to store and access securely client/user credentials

[API Reference](http://skm.openintegrationhub.com/api-docs/){: .btn .fs-5 .mb-4 .mb-md-0 }
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/secret-service){: .btn .fs-5 .mb-4 .mb-md-0 }
[Service File](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/secret-service){: .btn .fs-5 .mb-4 .mb-md-0 }

## Technologies used

<!-- please name and elaborate on other technologies or standards the service uses -->

## How it works

<!-- describe core functionalities and underlying concepts in more detail -->

# Introduction

In an environment similar to Open Integration Hub different services communicate with each other but also with external cloud services. Such external services normally require some sort of authentication and authorization in order to identify the communicating party and the it's privileges. Typically RESTful APIs tend to secure the API with API-Keys and tokens (e.g. OAuth 2.0 tokens). The usage of certificate in combination with a private certificate authority is also possible, but this document does not focus on the concept of a Public Key Infrastructure. From a security point of view, it is reasonable to store API-Keys and tokens encrypted and have strong authorization mechanisms to access them. In a multi-tenant environment – ideally using different encryption keys for each tenant. In Open Integration Hub we need an appropriate storage for tenant or user specific key value pairs, whereupon the value can be an encrypted string, certificate, etc. All stored keys and tokens should be accessible via an API if a valid authorization exists.
ice)

# Use Cases and requirements in Open Integration Hub

When dealing with integration flows there are different scenarios when secrets are required. Here are some examples:

1. Store an API-Key and use it for a specific solution in context of a specific integration flow.
2. Store the access & refresh tokens (OAuth 2.0) acquired through user consent.
3. Allow sharing of secrets with other users/groups but limit read & write access to the keys only to authorized users.
4. A connector which runs in context of an integration flow created by the user/tenant must be able to receive the required keys/tokens.

Additionally, it is essential to have a sophisticated audit logging for all operations involving these secrets.

# Secret Service

A dedicated service should carry out the management of keys and secrets. Keys need to have at least one owner and the ownership should be validated through OIH IAM. Additionally, the underlying vault framework should be interchangeable through an abstraction layer, thus ensuring a stable public API.

The Secret-Service will provide a CRUD-API for keys, which can be accessed by other privileged services, e.g. flow operator or even connector, as well as the users. The API may look similar to current elastic.io credentials API <https://api.elastic.io/v2/docs/#credentials>

Proposal for secret structure

```javascript
{
  "_id": "59f9f2ba112f28001921f274",
  "type":"credential",
  "name": "MySecret",
  "attributes": {
    "keys":{
      "host":"sftp.company.org",
      "username":"testuser",
      "password":"testpassword"
    }
  },
  "owners": [
    {
        "entityType": "USER",
        "entityId": "59f747c33f1d3c001901a44e"
    },
    {
        "entityType": "USER",
        "entityId": "59f747c33f1d3c001901a44f"
    },
    {
        "entityType": "TENANT",
        "entityId": "59f747c33f1d3c001901a43e"
    }
  ],
  "meta":{}
}
```

# Access control

To enforce authentication and authorization, the Secret-Service service must also store the owner or owner group, e.g. secret is owned by user with id XYZ. Similar to current elastic.io model, flow and credentials can have a group (workspace in elastic.io documentation) as an owner. If each user has at least her own private group and each tenant likewise, then the credentials owner can be described as an array of groups.

![Credentials ownership](Assets/credentials-relationship.png)

Credential ownership ensures, that only users belonging to the correct group/workspace are allowed to read/modify the secrets.

Apart from users, connectors also require access to credentials, e.g. when the user creates a flow and configures her credentials to be used in a specific flow step.

There are two alternatives to solve this

1. The service responsible for connector instantiation provides the connector with all required secrets
2. Each connector receives a token with which it can fetch the secrets from Secret-Service

With the second alternative we can make sure, that each connector has access only to specific secrets by providing them with a corresponding JWT token from IAM service. This JWT token would contain as a claim the user's group-id. This in return allows the Secret-Service to verify that the connector accesses only the credentials it actually is authorizes to read. Still, this seems a bit over-engineered compared to the first one. The second alternative does however make sense if it combined with the feature of AccessToken Auto-Refreshing.

As IAM currently does not have or support the concept of groups, we will implement the access control in the first phase as a tuple of entityType and entityId, e.g.

```javascript
{
    "entityType": "USER",
    "entityId": "59f747c33f1d3c001901a44e"
},
{
    "entityType": "TENANT",
    "entityId": "59f747c33f1d3c001901a44f"
}
```

and later as only a groupId, which must also exist in IAM.

The following diagram illustrates both alternatives.

![Passing credentials to connectors](Assets/skm-2.png)

# Flows, Connectors and Credentials

In an integration flow, each connector represented as a node, could receive it's own JWT token containing the claims to access those credentials, which are required in this specific step. This could be accomplished for example through scheduler or flow manager, which ever is responsible for creation of connector instances and passing of environment variables to them. The connector then can fetch all required secrets from Secret-Service and provide the JWT token containing the claims. The Secret-Service validates the request and returns the secrets, if the connector is authorized to access them.

The JWT payload could contain as less as the secret ids required for this specific integration step. The same ids could be present in the flow data payload, which the connector receives from the queue upon initialization.

# Summary

- Service account(s) or JWT for a connector to fetch secrets
- Adapter communicates with an external application and requires user's secret
  - Adapter either fetches the secrets itself from secrets service using it's own service account and context (context may be the flow or flow step, and thus limit the access only to secrets of flow owner)
  - or receive the secret as env vars through a superordinate priveleged service
- In case of OAuth access tokens, a separate service could be used to refresh access tokens (singleton) and all services using an access token must request it from this singleton service
  - Advantage: only one token refresh, all depending connectors fetch the new access token from this service
- Secrets are stored encrypted
- Access control to secrets is based on IAM authentication and authorization mechanisms
- Secret-Service has it's own mongodb, where it stores: secret_id (secret is stored in vault, but referenced through secret_id), owner (user or tenant)
- Secret-Service can fetch and return a new access token using the refresh token it stored in vault

# Secrets management framework

Our research for a suitable and mature solution lead us to HashiCorp Vault, which we will be using for our prototypical implementation. Other solutions should be possible to integrate, as the Secret-Service acts also as an abstraction layer of the underlying vault framework. This allows to interchange the vault framework through other solutions, as long as the Secret-Service API persists.

For a list of secrets managements solutions (albeit focused more on infrastructure and possible not up-to-date) please see this comparison list:

<https://gist.github.com/maxvt/bb49a6c7243163b8120625fc8ae3f3cd>

<https://www.vaultproject.io/intro/vs/index.html>

## Requirements

- Strong Encryption of stored secrets
- HA capability
- An open source project with a large community and activity
- Maturity of the framework
- Good documentation
- Flexible storage choice
- Enterprise ready

Compared to other solutions, we find that HashiCorp Vault [fits best](https://www.vaultproject.io/intro/vs/index.html) with our requirements.

# Access Token Auto-Refreshing

In the section Access Control we mentioned an alternative where connectors fetch the secrets directly from the Secret-Service. This approach has an advantage if at some point either Secret-Service or a correlating service also manages OAuth access tokens. In practice, this means that a connector would call Secret-Service an request an access token of a specific OAuth secret. The Secret-Service can then either return the access token, if it has a valid one or fetch the access token using for example client id and client secret. If more than one connector rely on a single access token (an identical access token), then fetching and refreshing of an access token is done ideally by a singleton – in our case Secret-Service or a correlating service responsible for this types of requests.

### Interaction with other Services

Secret Service can receive events from any service, but only directly interacts with two of them:

- [Message Oriented Middleware]({% link docs/Services/MessageOrientedMiddleware.md %}): It receives all events through the Message Oriented Middleware. It publishes several events for most important actions e.g. "secrets.secret.created".

- [Identity Management](https://openintegrationhub.github.io/docs/5%20-%20Services/IdentityManagement.html): It requires a bearer token created by the Identity Management to determine current user and check required permissions.
