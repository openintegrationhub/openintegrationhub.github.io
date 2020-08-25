---
layout: default
title: Development Example for Node.js
nav_order: 1
parent: Connectors
---

# Deprecated :warning:

This petstore example is only compatible with the legacy release of the Open Integration Hub. An updated example will be released in the future. Please see one of the existing connectors for an updated example.

[Snazzy Contacts Adapter](https://github.com/openintegrationhub/snazzycontacts-adapter)

[Snazzy Contacts Transformer](https://github.com/openintegrationhubsnazzycontacts-transformer)


# Node.js Development Example

Open Integration Hub supports `Node.js` programming language for building integration components such as Adapters and Transformers.

To help you create an Adapter in `Node.js` we have created a simple [Petstore component in Node.js](https://github.com/openintegrationhub/petstore-component-nodejs) which connects to the [Petstore API](https://petstore.elastic.io/docs/). This component is specified for the elastic.io iPaaS platform, which uses many Open Integration Hub services. Using elastic.io for learning the concepts makes it easier for you, as you can interact with the components via user interface - which is just more comfortable than using the framework alone. But don't worry, the technological basics are the same.

For now lets start with understanding the different parts a typical adapter:

## Petstore Component

Let us have a look at the structure of the Petstore Component in Node.js:

```
petstore-component-nodejs

├── component.json                                          (1)
├── lib
│   ├── actions                                             (2)
│   │   ├── createPetWithGenerators.js
│   │   └── createPetWithPromise.js
│   ├── schemas                                             (3)
│   │   ├── createPet.in.json
│   │   ├── createPet.out.json
│   │   └── getPetsByStatus.out.json
│   └── triggers                                            (4)
│       ├── getPetsByStatusWithDynamicSelectModel.js
│       ├── getPetsByStatusWithGenerators.js
│       └── getPetsByStatusWithPromises.js
├── logo.png                                                (5)
├── package.json                                            (6)
└── verifyCredentials.js                                    (7)
```

The Node.js components get built by NPM `run-script` which checks first the `package.json` (6) configuration file and starts initialising the `node` and `npm` versions and builds them. Next, the dependencies get downloaded and build. All node.js components must use the following dependencies:

```javascript
"dependencies": {
  "elasticio-sailor-nodejs": "^2.2.0",
  "elasticio-node": "^0.0.8"
}
```

Sailor is the Node.js SDK. It makes your component a citizen of the platform by providing a simple programming model for components and ensuring a smooth communication with the platform.

The `component.json` file (1) is the component descriptor interpreted by the platform to gather all the required information for presenting to the user in the platform user interface (UI). For example, you can define the component’s title in the component descriptor but also the component’s authentication mechanism. The descriptor is the only place to list the functionality provided by the component, the so called `triggers` and `actions`.

The directory `lib` together with its sub-directories **actions** (2), **schemas** (3) and **triggers** (4) get defined in the `component.json` file. The Node.js sources are in the sub-directories `lib/actions` (2) and `lib/triggers` (4). The JSON schemas defining the component’s metadata  are in the sub-directory `lib/schemas` (3). We discuss them in more details [later in this article](#component-descriptor).

If you have a logo for the component, you can place the file called `logo.png` (5) in the root directory of the component. Typically the logo of the API vendor gets used as component logo. If you did not provide any logo, the component will show a generic logo for your component.

Last but not least, the `verifyCredentails.js` (7) file should contain the main verification mechanism for all the Node.js components built for the {{site.data.tenant.name}} platform. We cover this topic [later in the article](#verify-credentials).


## Component descriptor

As mentioned above, the `component.json` file is the component descriptor interpreted by the platform to gather all the required information about the component. Let’s explore the descriptor of the Petstore component:

```json
{
  "title": "Petstore API (Node.js)",                                       (1)
  "description": "Component for the Petstore API",                         (2)
  "credentials": {                                                         (3)
    "fields": {
      "apiKey": {
        "label": "API key",
        "required": true,
        "viewClass": "TextFieldWithNoteView",
        "note": "Please use <b>secret</b> as API key."
      }
    }
  },
  "triggers": {                                                            (4)
    ...
    },
  "actions": {                                                             (5)
    ...
    }
  }
}
```
The component descriptor above defines the component title (1) and description (2). It also defines the fields used to ask the user to provide input for authentication (3). In this case a single field is define in which the user will input the `API key` for the Petstore API so that the component can communicate with the API on user’s behalf. The verification process is in the `verifyCredentails.js` file as mentioned above.

The triggers (4) and actions (5) properties define the component’s triggers and actions.

### Triggers

Now let’s have a closer look on how to define the `triggers`. The example below demonstrates the triggers section from the `component.json` component descriptor file:

```json
"triggers": {
    "getPetsByStatusWithGenerators": {                                  (1)
      "main": "./lib/triggers/getPetsByStatusWithGenerators.js",        (2)
      "type": "polling",                                                (3)
      "title": "Get Pets By Status With Generators",
      "fields": {                                                       (4)
        "status": {
          "label": "Pet Status",
          "required": true,
          "viewClass": "SelectView",
          "model": {
            "available": "Available",
            "pending": "Pending",
            "sold": "Sold"
          },
          "prompt": "Select Pet Status"
        }
      },
      "metadata": {
        "out": "./lib/schemas/getPetsByStatus.out.json"                 (5)
      }
    }
  }
```

The example above demonstrates that the trigger with id `getPetsByStatusWithGenerators` (1) gets implemented by the function described in the  `getPetsByStatusWithGenerators.js` file (2). The trigger is of `polling` type (3) meaning it will wake up periodically to poll for changes in the Petstore API. The triggers configuration is in the `fields` (4) and the out-metadata is in the file `getPetsByStatus.out.json` (5).

### Actions

Let us check also the `actions`. The example below demonstrates the actions section from the `component.json` component descriptor file:

```json
"actions": {
    "createPetWithPromise": {                                           (1)
      "main": "./lib/actions/createPetWithPromise.js",                  (2)
      "title": "Create a Pet With Promise",
      "metadata": {
        "in": "./lib/schemas/createPet.in.json",                        (3)
        "out": "./lib/schemas/createPet.out.json"                       (4)
      }
    }
  }
```

In the example above the action with id `createPetWithPromise` (1) gets implemented by the function described in the `createPetWithPromise.js` file (2). It is possible to give actions in the `fields` configurations like in the triggers part, however, this component has none of it. Compared to the triggers, the action has 2 metadata files. One is for in-metadata `createPet.in.json` (3) and the other is for out-metadata `createPet.out.json` (4).

## Verify Credentials

When creating an {{site.data.tenant.name}} component you may allow users to check entered credentials for validity, during the integration flow creation. This feature is useful when users need to type-in passwords, host-names, IP addresses, etc. Credentials verification is an optional step but it makes the user flow much more reliable and usable.

For node.js component you can use any libraries and/or functionality at your disposal to authorise the user credentials. But for the platform to find and call your verification code you have to make sure that:

*   Your component has a `verifyCredentails.js` file in the **root of the folder structure**
*   Your `verifyCredentials.js` file is a [common.js module](http://wiki.commonjs.org/wiki/Modules/1.1)
*   It returns one function that accepts **two parameters**: `credentials` and `cb` (callback).
*   All the credentials for verification gets passed through **credentials** parameter which is an object. This object can contain the properties that match or correspond to the account definition parameters from the `component.json`.

Here is the skeleton structure for the verify Credentials for a reference. You are welcome to use it as a starting point to write your own `verifyCredentails.js`:

```javascript
// here you can place your variables

// This function gets called by the platform to verify credentials
module.exports = function verifyCredentials(credentials, cb) {
    // In credentials you will find what users entered in account form
    console.log('Credentials passed for verification %j', credentials)
    if (true) {
        // Verified
        return cb(null, {verified: true});
    } else {
       // Verification failed
       return cb(null , {verified: false});
    }
}
```

The use of any specific verification method depends on the project and the third party API that gets communicated the credentials with. We will not dive into details of every possible solution for your chosen third party API here.

## Start your own
Now that you have the basics down, we suggest to start with determining what functionality your connector should include. Essential for that is the API you are connecting with.
[Actions and Triggers](https://openintegrationhub.github.io//docs/Connectors/ActionsAndTriggers.html){: .btn .fs-5 .mb-4 .mb-md-0 }

For a quick start, you can check out to our [contacts adapter template](https://github.com/openintegrationhub/contacts-adapter-template). It contains a baseline for all the functions you'll need, which you can then adapt and expand for your own requirements.

In the future, the sailor utility described earlier will be replaced by a new OIH-specific version called the Ferryman. The actual connector architecture and functionality will not be impacted by this. For more information about the Ferryman, check the [Ferryman documentation](https://github.com/openintegrationhub/openintegrationhub/tree/master/lib/ferryman)

More advanced and optional additions to any connector include [Conflict Management](https://openintegrationhub.github.io//docs/Services/ConflictManagement.html).
