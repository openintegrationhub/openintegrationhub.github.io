---
layout: default
title: Integration Flows
parent: Basic Concepts
nav_order: 2
has_children: false
---

# Flow Basics

Flows are one of the most fundamental functions of the Open Integration Hub. They allow you to automatically pass data through a series of user-defined components, allowing you to transfer and modify it according to your requirements.

## Flow Structure

A Flow can best be described as a simple directed graph, connecting components to one another. Each component can receive data from the one that comes before it, and pass it on to next one in line. Flows can be as simple or as advanced as required, ranging from simple end-to-end data transfer to complex branching flows with various user-defined interactions.

The simplest, and probably most common example of a Flow is called an Integration Flow. It is used to transfer data from one application to another. Say you would like to transfer your contacts from your Google Contacts account to your Microsoft Outlook account. All you need for that are two components (called Connectors, as they connect with an external application): One to fetch your data from Google, the other to send it to Outlook.

<p align="left">
  <img src="https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images//FlowExample1.png" alt="Simple Flow Graph" width="250"/>
</p>

If you decide you'd like to additonally send your contacts to a third destination such as Salesforce, it's as simple as adding a third component to your flow. The data received from Google would then automatically be delivered both destinations at once.

<p align="left">
  <img src="https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images//FlowExample2.png" alt="Expanded Flow Graph" width="250"/>
</p>

## Flow Definition

Within the framework of the OIH, a Flow is defined through a JSON definition, allowing a user to read and adjust Flows without requiring a dedicated interface. For example, a simple Flow could be described as such:

```json
{
  "name": "Google to Outlook",
  "description": "This flow transfers contacts from Google Contacts to Microsoft Outlook.",
  "graph": {
    "nodes": [
      {
        "id": "step_1",
        "componentId": "60005f333ef679001b0949f5",
        "name": "Google Component",
        "function": "getPersons",
        "credentials_id": "5cb87489df763a001a54c7de"
      },
      {
        "id": "step_2",
        "componentId": "5f64afabdf73fe001b4e3303",
        "name": "Outlook Component",
        "function": "upsertPerson",
        "credentials_id": "5f7ef9da20f27900113b13c9"
      }
    ],
    "edges": [
      {
        "source": "step_1",
        "target": "step_2"
      }
    ]
  },
  "cron": "* * * * *"
}
```

The fields shown here have the following meanings:

- name: A name to recognize your Flow by
- description: A more detailed user-defined description explaining the Flow's function
- graph: The heart of the Flow definition, determining which components take what actions in what order.
- cron: By default, Flows are executed periodically as defined by a [cron](https://en.wikipedia.org/wiki/Cron) expression.

The graph in particular requires more explanation. Like any graph, it consists of a number of nodes, connected to one another through a number of edges. Each node contains one component used by the Flow, while the edges determine in what direction data is routed from one component to another.

Each node contains these fields:

- id: A unique identifier for this node within the graph
- componentId: The id under which the component is saved within the OIH. These can be found in the Component Repository
- name: A descriptive name for the node, to better explain what it does
- function: What the component is supposed to do. Each component can offer several functions, this field determines which one will be executed
- credentials_id: Communication with external applications generally requires a set of credentials. These can be securely stored in the OIH Secret Service and referenced by their ID here

The edges are more self-explanatory, each containing a source and a target, which refers to an id field of a node.
