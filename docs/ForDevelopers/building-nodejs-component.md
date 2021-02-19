---
layout: default
title: Development Example for Node.js
nav_order: 1
parent: For Developers
has_children: true
---

# Node.js Development Example

Open Integration Hub supports `Node.js` programming language for building integration components such as Components.

To help you create a Connector in `Node.js` we have created a simple template component to start out with: [Contacts Component Template](https://github.com/openintegrationhub/contacts-adapter-template). It is a well-documented minimal component intended to fetch and push data of the Contacts domain from and to the SnazzyContacts API. It is built with modularity in mind, so that for your own case you should be able to re-use most of the code, needing only to switch out the parts specific to the API you would like to target.

For now lets start with understanding the different parts a typical component:

## Contacts Component

Let us have a look at the structure of the Contacts Component in Node.js:

```
contacts-component-template

├── component.json                                          (1)
├── lib
│   ├── actions                                             (2)
│   │   ├── deletePerson.js
│   │   ├── deleteOrganization.js
│   │   ├── upsertOrganization.js
│   │   └── upsertPerson.js
│   └── triggers                                            (3)
│       ├── getOrganizationsPolling.js
│       └── getPersonsPolling.js
│   └── utils                                               (4)
│       ├── authentication.js
│       ├── helpers.js
│       └── resolver.js
│   └── tranformations                                     (5)
│       ├── organizationFromOih.js
│       ├── organizationToOih.js
│       ├── personFromOih.js
│       └── personToOih.js

├── package.json                                            (6)
```

The Node.js components get built by NPM `run-script` which checks first the `package.json` (6) configuration file and starts initialising the `node` and `npm` versions and builds them. Next, the dependencies get downloaded and build. All node.js components must use the following dependency:

```javascript
"dependencies": {
  "@openintegrationhub/ferryman": "^1.1.3",
}
```

Ferryman is the Node.js SDK. It makes your component a citizen of the platform by providing a simple programming model for components and ensuring a smooth communication with the platform.

The `component.json` file (1) is the component descriptor interpreted by the platform to gather all the required information for presenting to the user in the platform user interface (UI). For example, you can define the component’s title in the component descriptor but also the component’s authentication mechanism. The descriptor is the only place to list the functionality provided by the component, the so called `triggers` and `actions`.

The directory `lib` together with its sub-directories **actions** (2) and **triggers** (3) get defined in the `component.json` file. The Node.js sources are in the sub-directories `lib/actions` (2) and `lib/triggers` (3).

## Component descriptor

As mentioned above, the `component.json` file is the component descriptor interpreted by the platform to gather all the required information about the component. Let’s explore the descriptor of the Petstore component:

```json
{
  "title": "Contacts Component Template",                                       (1)
  "description": "OIH contacts component example",                              (2)
  "triggers": {                                                                 (3)
    ...
    },
  "actions": {                                                                  (4)
    ...
    }
  }
}
```

The component descriptor above defines the component title (1) and description (2). The triggers (3) and actions (4) properties define the component’s triggers and actions.

### Triggers

Now let’s have a closer look on how to define the `triggers`. A trigger is any function that fetches data from the outside and then passes it forward, such as fetching a list of contacts from an API. Generally, triggers are the first step in any flow. The example below demonstrates the triggers section from the `component.json` component descriptor file:

```json
"triggers": {
    "getPersonsPolling": {                                                                        (1)
      "title": "Fetch new and updated persons(getPersons - Polling)",
      "description": "Get Snazzy Contacts persons which have recently been modified or created",
      "main": "./lib/triggers/getPersonsPolling.js",                                              (2)
      "metadata": {
        "out": "./lib/schemas/getPersons.out.json"                                                (3)
      }
    }
  }
```

The example above demonstrates that the trigger with id `getPersonsPolling` (1) gets implemented by the function described in the `getPersonsPolling.js` file (2). The metadata field `out` can optionally refer to an example of the kind of data this trigger generates.

### Actions

Let us check also the `actions`. An action is any function that receives data from an earlier step in the flow (either an action or trigger itself) and then acts with and/or on that data in some fashion. The example below demonstrates the actions section from the `component.json` component descriptor file:

```json
"actions": {
    "upsertPerson": {                                      (1)
      "title": "Upsert a person in Snazzy Contacts",
      "main": "./lib/actions/upsertPerson.js",             (2)
      "metadata": {
        "in": "./lib/schemas/upsertPerson.in.json",
        "out": "./lib/schemas/upsertPerson.out.json"
      }
    }
  }
```

In the example above the action with id `upsertPerson` (1) gets implemented by the function described in the `upsertPerson.js` file (2). Compared to the triggers, the action has 2 metadata files. One is for in-metadata `upsertPerson.in.json` (3) and the other is for out-metadata `upsertPerson.out.json` (4).

### Utils

This folder contains various functions necessary for the actual functionality of the connector. Of course, it's entirely possible to have all the functionality contained entirely within the code of the respective trigger or action. But in practice we have found it's preferable to have a split like this, in order to aid maintainability and reduce redundancy.

Here's a short overview over each file and its intended purpose:

- `authentication.js`: Handles the authentication necessary for taking privileged actions on the API, such as posting login data to the application and receiving an access token. This may not be necessary if your API allows for long-lived API keys or bearer tokens that can be passed on to the connector directly.

- `helpers.js`: Contains all the functions necessary for actually communicating with the target API. It sends the necessary requests using a HTTP client and formats the responses so that they can be used by the rest of the flow. Generally, this boils down to returning the body of the request response, and converting it to JSON if necessary.

- `resolver.js`: Contains an example implementation of the OIH Conflict Management module. This is somewhat more advanced and beyond the scope of this guide. If you would like to know more, check the documentation of the [Conflict Management]({% link docs/Services/ConflictManagement.md %})

<!-- ### Transformers

This folder contains various tranformation functions from the components API to the OIH master model and vice versa.

[Transformation Functions]({% link docs/BasicConcepts/TransformFunction.md %}){: .btn .fs-5 .mb-4 .mb-md-0 } -->

## Start your own

Now that you have the basics down, we suggest to start with determining what functionality your connector should include. Essential for that is the API you are connecting with.
[Actions and Triggers]({% link docs/ForDevelopers/ActionsAndTriggers.md %}){: .btn .fs-5 .mb-4 .mb-md-0 }

All communication between your connector and the OIH platform is handled by the Ferryman module. For more information about the Ferryman and the functionalities it offers, check the [Ferryman documentation]({% link docs/Services/Ferryman.md %})

## Connector Testing

Once you have begun building a new connector, the next step is testing it in concert with other connectors. Of course, one way of doing that is by simply connecting it with another existing connector in a flow similar to your intended use. But as an alternative, we also offer a minimal development connector with extensive logging of everything it sends and receives. Simply connect it with your own connector in a simple two-step flow and check the logs of the k8s deployment to see whether everything works as it should:

[Development Connector](https://github.com/openintegrationhub/development-connector)
