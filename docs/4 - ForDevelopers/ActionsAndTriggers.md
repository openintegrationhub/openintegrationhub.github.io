---
layout: default
title: Actions and Triggers
nav_order: 1
parent: Development Example for Node.js
grand_parent: For Developers
---

# Actions and Triggers

Actions and Triggers are the two primary types of functions offered by components. As a general guideline, Triggers are responsible for fetching and receiving data from external sources, whereas Actions receive data from other components and then act upon that data in some fashion, such as by modifying it or transferring it to an external destination. Most simple integrations have a component executing a trigger at the beginning, fetching data from one place, and a component executing an action at the end, posting that data elsewhere.

Note that this distinction is only a convention to more easily understand a component's capabilities. On the software level, both actions and triggers are treated the same.

## Triggers

As mentioned above, Triggers are generally responsible for fetching and/or receiving data from external sources and introducing it into an integration flow. There are two general types of triggers, distinguished by when and how they are triggered:

### Polling Triggers

Polling triggers are automatically executed in set intervals by the OIH framework. The frequency of this execution is defined on a flow-by-flow basis, expressed through a CRON-expression. Whenever a polling trigger is executed, it independently fetches data from a particular source. Most commonly this source will be a REST-API, but it is also possible to fetch data from different sources through other protocols, such as directly querying a database.

Unless a particular use case requires otherwise, it is generally recommended that a given trigger only fetch one type of resource. For example, if you're developing a connector for a store API that can return data objects describing customers, orders, and items, it would be recommoned to create one unique trigger for each of these object types. 

#### Snapshots

Generally, when using polling triggers you do not wish to fetch the same identical data set every time on each execution. More commonly, you'll want to fetch only whatever data has changed since the last execution of the trigger. For this purpose, the OIH offers the snapshot functionality. Snapshots allow a trigger to store a state between each of its executions. For example, you could use snapshots to store a timestamp of the most recent change in the data set you fetched, and in future executions only fetch objects that have been changed since that timestamp.

### Webhook Triggers

As the name suggests, webhook triggers are triggered through external incoming webhooks. External systems can post data to an URL exposed by the OIH's [Webhook Service](https://openintegrationhub.github.io/docs/5%20-%20Services/Webhooks.html), and this data is then passed directly to a webhook trigger. As such, a webhook trigger does not need to independently fetch data, only receive it. In this case, its role consists primarily of verifying the integrity of the received data, and optionally to transform it.
