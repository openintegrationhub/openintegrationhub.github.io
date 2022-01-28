---
layout: default
title: Component Best Practices
nav_order: 7
parent: For Developers
has_children: false
---

# Best Practices for Component Development

In principle, an OIH component can include any arbitrary code and execute nearly any function you might need it to. Individual components can be customized to work together directly depending on your use-cases, but we have also developed a number of best practices to allow for easy compatibility of already existing components.

## Message Format

In order to more easily pass data between components, we have settled on this JSON message format:

```json
"metadata": {
    "recordUid": "String",
    "oihUid": "String"
  },
"data": {}
```

The `metadata` property is used to contain information about a data object, such as aliases it is known under within the OIH (`oihUid`) and other systems (`recordUid`). The `data` property is used to contain the actual content of the message, and can be an arbitrary object. These two properties will be present at the base level of the first argument received by a component's processMessage function. When emitting a message, simply pass an object with these properties to the emit function.

This format is expected by the Component Orchestrator. If you wish to use a different message format within your OIH deployment, you will need to use a modified version of the Component Orchestrator to support it.

### Delete Requests and Responses

As deletions are of a particularly sensitive nature, we've developed a more detailed message format intended to prevent accidental deletions and provide a more detailed record of what has happened. 

A request to delete an object would look like this:

```json
"metadata": {
    "recordUid": "String",
  },
"data": {
    "deleteRequested": "1642683883"
  }
```

As before, `recordUid` refers to the alias ID of the data object to be deleted. If the OIH [ID-Linking](https://openintegrationhub.github.io/docs/5%20-%20Services/DataHub.html#id-linking) is used, this can be automatically fetched and filled in by the OIH. 

The `deleteRequested` property can contain either a timestamp describing when the deletion was requested, or simply a boolean `true`. The important part is that it resolves to a truthy value, which serves to unambigiously confirm that a deletion is requested. Conversely, a component should never delete when this property is not present and truthy.

If a delete request of this format is received and processed, a component should emit a confirmation like this:

```json
"metadata": {
    "recordUid": "String",
  },
"data": {
    "delete": enum("pending", "confirmed", "denied", "failed"),
    "signature": "optional",
    "timestamp": "123456789"
  }
```

The `delete` key describes the result of the attempted deletion, and `timestamp` when it took place. Optionally, a component-specific `signature` may be added as an extra security to uniquely identify the component who attempted the deletion.

If the OIH [Data Provenance](https://openintegrationhub.github.io/docs/5%20-%20Services/GovernanceService.html#data-provenance) functionality is used, it will use this data to document the deletion process.
