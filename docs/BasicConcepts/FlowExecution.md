---
layout: default
title: Flow Execution
parent: Basic Concepts
nav_order: 3
has_children: false
---

# Flow Execution

This document is designed to describes the basics of how flows are being executed. It acts as a starting point to get an understanding of the Open Integration Hub.
Most of the examples are triggered by user interactions (e.g. starting a flow) and only the "happy path" i.e. success scenario is described.

Each example is described through a graphical overview, a textual description, and pre-conditions.
For further information for a specific version please have a look at the [services]({% link docs/Services/Services.md %}).

- [Starting a flow](#starting-a-flow)
  - [Flow repository](#flow-repository)
  - [Webhooks](#webhooks)
  - [Scheduler](#scheduler)
- [Execute Polling Flow](#execute-polling-flow)
- [Execute Webhook Flow](#execute-webhook-flow)
  - [POST Request](#post-request)
  - [GET Request](#get-request)
- [Request Resources](#request-resources)
- [Creating audit log records](#creating-audit-log-records)

# Starting a flow

**Pre-Conditions:** None.

This example describes the scenario of starting a flow. Once the user starts a flow the following steps are processed:

1. The user starts a flow using the flow repository's `REST API`.
2. Flow Repository sets the flow's status from `inactive` to `starting` and raises the event `flow.starting`.
3. There are three services listening to the event `flow.starting`: Webhooks, Scheduler, and Component Orchestrator. Each of them examines the event's payload and decide if they need to react appropriately. We will discuss the exact reaction of `Webhooks` and `Scheduler` later in this document.
4. Upon receiving `flow.starting` event the `Component Orchestrator` starts deploying local compontents. Once all local components were deployed, `Component Orchestrator` raises the `flow.started` event regardless of whether used global components are running – **Keep in mind that global components need to be started manually. otherwise, warnings are thrown**
5. Flow Repository receives the `flow.started` event and switches flow's status property from `starting` to `started`.
6. Webhooks receives the `flow.started` event and starts receiving incoming HTTP calls for the given flow.
7. Scheduler receives the `flow.started` event and starts scheduling the flow, according to its cron property.
8. When a client stops a running flow using the flow repository's REST API, the event `flow.stopping` is raised which is causing an inverse reaction chain of events.

Now let's discuss the individual services in detail:

## Flow repository

[Service Documentation]({% link docs/Services/FlowRepository.md %})

- `POST /flows/{id}/start`: Used to start a flow
- `POST /flows/{id}/stop`: Used to stop a flow

Upon receiving the HTTP call for starting a flow, Flow Repository sets the flows `status` to `starting`. Upon receiving a stopping request it sets the status to `stopping`. If the flow has been started and the flow repository receives `flow.started` event, it sets the status to `active` in contrast, it sets it to `inactive` upon receiving `flow.stopped`. The schema of the event payload is shown below.

```yaml
Event:
  type: object
  required:
    - headers
    - payload
  properties:
    headers:
      type: object
       required:
         - name
         - createdAt
         - serviceName
      properties:
        name:
          type: string
        createdAt:
          type: string
          format: date-time
        serviceName:
          type: string
    payload:
      type: object
```

The `payload` property is an arbitrary object to be sent with the event. Flow repository will send the entire flow as `payload`.

## Webhooks

[Service Documentation]({% link docs/Services/Webhooks.md %})

Please note that Webhooks ignores a flow if the following condition is met:

- `cron` property is set

Upon receiving `flow.starting` event the service checks if the `cron` property is **not** set. If so, the service persists a data record in his local DB but **doesn't start receiving HTTP requests** for the given flow yet.
After receiving the `flow.started` event, the service starts accepting incoming messages from the flow's webhook URL and instructs `Component Orchestrator` by publishing the `flow.executed` event as soon as a valid webhook call is carried out.

Upon receiving the `flow.stopping` event, the service deletes the record for the given flow and stops accepting requests.

## Scheduler

[Service Documentation]({% link docs/Services/Scheduler.md %})

Please note that Scheduler ignores a flow if the following condition is met:

- `cron` property is missing

Upon receiving `flow.starting` event the service checks if the `cron` property is set. If so, the service persist a data record in his local DB, but **doesn't start scheduling** the given flow yet. The following table demonstrates an example of such records.

| flowId                   |      cron       |      dueExecution       |
| ------------------------ | :-------------: | :---------------------: |
| 58b41f5da9ee9d0018194bf3 | _/3 _ \* \* \*  | 2019-01-25T13:39:28.172 |
| 5b62c91afd98ea00112d5404 | 15 14 \* \* 1-5 | 2019-01-27T14:15:00.00  |

Upon receiving the `flow.started` event the service starts scheduling the flow executions by retrieving the flow data from its local DB and instructing `Component Orchestrator` by publishing the `flow.executed` event.

Upon receiving the `flow.stopping` event, the service deletes the record for the given flow and stops scheduling flow executions.

# Execute Polling Flow

**Pre-Conditions:**

- Starting a flow.
- `cron` property is set

As described in [scheduler section](#scheduler) when a flow is started the service starts scheduling the flow executions. Once a flows `cron` condition is met it publishes `flow.executed`.

![webhookPost](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ExecutePollingFlow.png)

Figure: _executePollingFlow_

# Execute Webhook Flow

**Pre-Conditions:**

- Starting a flow.
- `cron` property must **NOT** exist

Once Webhooks receives a valid request it publishes `flow.executed`.
Either `POST` or `GET` requests are accepted.

## POST Request

Requests of this kind should have their payload within the `body`.

![webhookPost](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ExecuteWebhookFlowPost.png)

Figure: _executeWebhookFlowPost_

## GET Request

Requests of this kind have to be structured differently. Webhooks takes the url parameters and request headers and puts them into the message. This means in particular the headers go to `headers` while query string parameters go to `body`.

An examplary webhook GET request could look like the following: `GET /hook/<flow-id>?param1=value&param2=value`

![webhookPost](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ExecuteWebhookFlowGET.png)

Figure: _executeWebhookFlowGet_

# Request Resources

The following example shows every step necessary to allow a user to request a flow resource. The graphic below shows how this example would look like.

1. User logs in into IAM. (e.g. Basic Auth)
2. IAM responds with an access token token.
3. User uses this access token to request a cetrain resource (e.g. a specific flow by id).
4. Flow repository introspects the token at specific IAM endpoint (services accounts receive a permanent token when they first register) using IAM utils (middleware).
5. IAM responds with user information such as username, tenant, tenant specific role and user permissions related to this token.
6. Flow Repsitory checks if the user has the permission to request the resource.
7. Flow repository responds with the requested information.

Illustration of this process: (Figur _requestResourceSuccess_).

![requestResourceSuccess](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/requestResourceSuccess.png)

Figure: _requestResourceSuccess_

**1**: Access token<br>
**2**: Service makes request with service account token<br>
**3**: User information e.g.: username, tenant, tenant specific role, permissions

# Creating audit log records

To create a record that should be stored in the audit log a service simply has to put a message onto the queue with a predefined topic. Each service decides on its own, which events should be stored in the audit log service.
Audit log listens to all events having `audit.*` as topic.

![creatingAuditLogEvents](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/CreatingAuditLog.png)
