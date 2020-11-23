---
layout: default
title: Guide to Update Old Connectors
nav_order: 3
parent: Connectors
---
Open Integration Hub is an open and community driven project. Customization and individual implementation come with it's nature of being a generalized framework. This also results in integration components, which are open sourced by their creators, but sometimes adjusted to fit their individual needs.

In order to make modification of such components to fit the current framework version easier, this document will list some common changes you may see.

# Elastic.io Components
As one of the founding partners of the framework and contributor of several core services, elastic.io also provides many helpful [integration components](https://github.com/elasticio). Those are adjusted for the elastic.io platform. Check and adjust the following, when running it under "vanilla Open Integration Hub":

## Must Have

### Ferryman Dependency
Ferryman is the Node.js SDK of the Open Integration Hub. The main role is to make the component part of Open Integration Hub platform and it ensures a smooth communication with the platform. You need to add ferryman the package.json file:

```json
"dependencies": {
  "@openintegrationhub/ferryman": "^1.1.5"
}
```
Elastic.io's equivalent to the ferryman is the so called sailor. You may remove it's dependcies in case you just want to use the component in an Open Intergation Hub implementation.

[Example](https://github.com/elasticio/sugarcrm-component/blob/69ea950d9ba57500d23b60dfc4d67a7c5eebdea8/package.json#L31-L33):

```json
    "elasticio-node": "0.0.9",
    "elasticio-rest-node": "1.2.6",
    "elasticio-sailor-nodejs": "2.6.18",
}
```

### Message Format
The messaging format has been adjusted over time, most recently with [issue#1078](https://github.com/openintegrationhub/openintegrationhub/issues/1078).

In practice, the changes boil down to:
Removed ```id``` key
Removed ```headers``` key
Removed ```body``` key, moved its contents to base level
Content that was formerly in ```body.data``` is now in just ```data```
Content that was formerly in ```body.meta``` is now in ```metadata```

Most connectors used to rely on a function called ```messageWithBody()``` or ```newMessage()``` to format things for the old format, which will no longer be necessary.

You may also find [this PR](https://github.com/openintegrationhub/snazzycontacts-adapter/commit/49aa1336f6c29d98c0bf86cac924e9c2da07adbf) helpful.

### Dockerfile
Elastic.io uses a different mechanism to build images and deploy components for their platform. You need to add a dockerfile.

You can just copy and adjust a Dockerfile and existing component under https://github.com/openintegrationhub

If the entrypoint is set to package.json, pelase make sure to add the necessary scripts there, as you can see in the general template:

https://github.com/openintegrationhub/contacts-adapter-template/blob/master/Dockerfile
https://github.com/openintegrationhub/contacts-adapter-template/blob/09965e800eb7dd934a73b218cffcd428ff8aca8e/package.json#L19

### IDs

In order to identify data records across all the framework you will have to implement additional UIds:

```
/*
      This metadata is used to identify the current object within the OIH and your application
      "oihUid" uniquely identifies it within the OIH itself
      "recordUid" is used to identify it within your application
    */
    const { oihUid, recordUid } = msg.metadata;
```
    

Please make sure that upsertFunctions point towards metadata.recordUid. For your reference please also check the general template: https://github.com/openintegrationhub/contacts-adapter-template/blob/master/lib/actions/upsertObject.js#L47

For ID-linking purposes you may also want to insert an applicationUid, as seen for example here:
https://github.com/openintegrationhub/snazzycontacts-adapter/blob/45943075edb5bccf84b0cd51ea732fb30cc5add1/lib/actions/upsertOrganization.js#L36

## Optional

### Transform Functions
In order to transform data from one schema to another you can either build seperate Tranformer Components or use the [transformFunctions within the ferryman](https://github.com/openintegrationhub/openintegrationhub/blob/master/lib/ferryman/lib/transformer.js).

You will have to implement these functions for each actions and trigger like shown in the general template:https://github.com/openintegrationhub/contacts-adapter-template/blob/09965e800eb7dd934a73b218cffcd428ff8aca8e/lib/triggers/getObjects.js#L19

### Stuff you can delete
- the ```schema folder``` is not needed
- ```verifyCredentials.js``` is only needed for the elastic.io and not for the Open Integration Hub.
- ```credentials``` within the component.json is only needed for elastic.io UI. Depedning on your implementation you may want to keep it. With regards to Open Integration Hub, it is not needed.
