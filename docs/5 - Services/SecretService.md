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

[API Reference](http://skm.openintegrationhub.com/api-docs/)
[Implementation](https://github.com/openintegrationhub/openintegrationhub/tree/master/services/secret-service)

## Technologies used

Node.JS
MongoDB
crypto
OAuth

## How it works

This service is used to securely store and access client secrets/credentials (e.g. Basic Auth, OAuth tokens, etc.). This service can also create OAuth flows, such as 3-legged and also automatically refresh OAuth accessTokens if a valid refreshToken exists. The primary use-case is to allow connectors to fetch data from external APIs on behalf of the user.

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

Tenant and user specific secrets are stored in this service. Infrastructure specific secrets, such as k8s Config Maps or IAM Tokens are not part of it. Secrets have at least one owner. Each secret element has an array of owners. Although, ideally no secret should have two different users as owners, in some cases this is required, which we will touch on later. 

The Secret-Service will provide a CRUD-API for keys, which can be accessed by other privileged services, e.g. flow operator or connectors, as well as the users. 

Secret document structure. An example with an OAuth 2 secret:

```javascript
{
    "_id" : ObjectId("61d8550ae4b4931adb279676"),
    "encryptedFields" : [],
    "__t" : "S_OA2_AUTHORIZATION_CODE",
    "owners" : [ 
        {
            "id" : "5bffec9902586f630df98a73",
            "type" : "USER"
        }
    ],
    "type" : "OA2_AUTHORIZATION_CODE",
    "value" : {
        "authClientId" : ObjectId("5e05f5b06202450d1fdc15a2"),
        "accessToken" : "EwBgA+l3BAAUFyCt+7N9khsYleqxRpqgu....xY0kCPl8C",
        "scope" : "https://outlook.office.com/Calendars.Read https://outlook.office.com/Contacts.Read",
        "expires" : "2022-02-04T12:06:30.170Z",
        "refreshToken" : "M.R3_BAY.-CX4aFsEiqAPkuopSmJgjjI...eeV5b!feI4nD8ASHt8X9OyIUC2udFinVr",
        "externalId" : "hello@example.com"
    },
    "name" : "hello@example.com",
    "createdAt" : ISODate("2022-01-07T14:58:18.214Z"),
    "updatedAt" : ISODate("2022-02-04T11:06:30.191Z"),
    "__v" : 0,
    "lockedAt" : null
}
```

# Auth clients

An auth client manages the communication with an external identity provider, e.g. OAuth2 tokens. It containts the clientId and clientSecret of OIH platform provider, which are required to create and update OAuth flows. After registering your application with the 3rd party (e.g. Google), create an auth client and add your clientId and clientSecret. You must also register the callback URL redirectUri of Secrets-Service with the third party. In case of OAuth/OAuth2, you should also define the auth and token in endpoints. See the OpenAPI spec for AuthClient model definition.

Example of a Microsoft Oauth2 auth client:

```
{
  "predefinedScope" : "offline_access",
  "endpoints" : {
    "auth" : "https://login.microsoftonline.com/common/oauth2/v2.0/authorize?prompt=select_account&scope={{scope}}&response_mode=query&state={{state}}&redirect_uri={{redirectUri}}&response_type=code&client_id={{clientId}}",
    "token" : "https://login.microsoftonline.com/common/oauth2/v2.0/token",
    "userinfo" : "https://login.microsoftonline.com/common/openid/userinfo"
  },
  "clientId" : "${CLIENT_ID}",
  "clientSecret" : "${CLIENT_SECRET}",
  "redirectUri" : "https://${CALLBACK_URL_BASE}/hook",
  "mappings" : {
    "externalId" : {
      "source" : "access_token",
      "key" : "unique_name"
    },
    "scope" : {
      "key" : "scope"
    }
  },
  "__t" : "A_OA2_AUTHORIZATION_CODE",
  "type" : "OA2_AUTHORIZATION_CODE",
  "name" : "Microsoft oAuth2",
  "owners" : [
  ]
}
```


# Access control

Only secret owners can access a secret. Multiple users can be owners a signle secret. This is because some OAuth providers only allow a single refresh-/access token to be issued to a user at a time. If a user were to have two secrets with the same credentials, then refreshing one secret would automatically invalidate the second secret. For any given OAuth 2 owner, when a secret is created, we know which authClient is used and which identity the secret has in the target system. The combination of authClient and secret value determine if an existing secret will receive an new owner or a new secret is created.

Once a secret is created, sensitive fields (e.g. password, accessToken, refreshToken) aren't returned via the REST-API when requesting a secret. Sensitive data in a secret (password, accessToken, refreshToken) are masked with stars *** and aren't displayed plain in the response. To see the raw data, the requester must have the `secrets.secret.readRaw` permission (see IAM).

Apart from users, connectors also require access to credentials, e.g. when the user creates a flow and configures her credentials to be used in a specific flow step. The connectors also require the sensitive fields in order to be authorized to perform a request with the given 3rd party.
Currently, this handled by the orchestrator. For each connector, the orchestrator create a temporary IAM token for the connector and appends the `secrets.secret.readRaw` permission to this IAM token. This allows the connectors to use the accessToken for requests, but the risk of exposing these secrets to users is reduced.


# CRYPTO (encryption) for secrets

All sensitive fields (listed in src/constant) of every secret will be encrypted before they get stored into database. By default aes-256-cbc is used to provide fast and secure encryption. Therefore, you need to specify a key adapter to supply users with the keys and setup encryption. Users receive decrypted secrets only if a valid key is provided.

All secrets are encrypted by default. You can disable this by setting the CRYPTO_DISABLED="true" env var. (!) If you wish to use secrets encryption, make sure you provide the CRYPTO_KEY env var with your encryption key. Otherwise a random encryption key will be generated each time you start this service.

**Default Settings**
- CRYPTO_DISABLED: false - Turns on encryption.
- CRYPTO_ALG_HASH: sha256 - Hashing of externalId to obfuscate private data.
- CRYPTO_ALG_ENCRYPTION: aes-256-cbc - Default algorithm used for encryption.
- CRYPTO_OUTPUT_ENCODING: latin1 - Charset of encryption output.


# Access Token Auto-Refreshing

Whenever an accessToken is fetched, Secret service checks if the secret has an `expires` property (`value.expires`). If a predefined threshold (for example <5 min) is reached, the secret will be automatically refreshed before returning to the requester. All OAuth secrets are refreshed on-demand and never periodically. Should the refreshToken expire due to inactivity, the document will be flaged with an error. Whenever the OAuth provider returns a new refreshToken, it is automatically refreshed on the secret document as well.

Secret service is a HA capable service and can run in a replica set. A secret can be requested by multiple connectors simultaniously. If an accessToken needs to be refreshed, we have a race condition. Whenever a secrets is refreshed, the document is locked via the `lockedAt` property. A subsequent secret service process/node will recongnize that the document is locked, backoff and retry shortly afterwards. This ensures, that the secret is updated only once and all requesters will potentially wait a few milliseconds longer for the request to be processed.


### Interaction with other Services

Secret Service can receive events from any service, but only directly interacts with two of them:

- [Message Oriented Middleware](https://openintegrationhub.github.io/docs/5%20-%20Services/MessageOrientedMiddleware.html): It receives all events through the Message Oriented Middleware. It publishes several events for most important actions e.g. "secrets.secret.created".

- [Identity Management](https://openintegrationhub.github.io/docs/5%20-%20Services/IdentityManagement.html): It requires a bearer token created by the Identity Management to determine current user and check required permissions.
