---
layout: default
title: Policy Introduction
nav_order: 6
has_children: false
parent: For Developers
---

# Introduction to Policies

OIH Policies offer a way for you to control exactly which data objects are passed through a flow irrespective of connector logic. This way, no matter which data objects are passed into the flow from a source, you can still ensure that any sensitive data is blocked.

## Requirements

In order to use policies, your OIH installation must be running the Governance Service on version 1.2.1 or newer, and the connectors in the flow must be using the Ferryman version 1.6.3 or newer.

## Concepts

The general concept of Policies can be divided into three main parts: Actions, Permissions, and Duties. These can be further refined with Constraints.

### Actions

An action describes in general what a flow step does with a given data object. It is expressed as a simple, preferably human-readable, string value. It could be something like `marketing`, `archival`, or `distribution`. There are no predefined formats or keywords for these actions, you can choose them in whichever way makes it easiest for you to keep track of. The value is only relevant insofar as they match the same action noted in an object's Permissions.

### Permissions

A data object may have any number of Permissions assigned to it. A Permission consists of an Action expressed by a string value (see above), and optionally certain constraints (more on those later). In a flow configured to check policies, only objects with a Permission matching the flow's Action will be passed through, all others are blocked.

For example, say you would like to transfer data from a contact management app to an email marketing app. You might give that flow the Action `marketing`, and then only objects who have a Permission referring to the action `marketing` would be passed through the flow. Any objects lacking this Permission (e.g. because they've been marked as "no contact" in the contact management app) would be blocked and not transferred further.

### Duties

A duty is something that must be done every time a data object is transmitted through a flow. This may be something like posting a notification about the data transfer to a third party, or stripping certain sensitive information out of the object before passing it on. A data object may have any number of duties attached to it, and may only be transferred if each duty has been successfully carried out.

For example, in the scenario of a contact app to marketing app flow described above, you could define a Duty that any personal data not immediately relevant to the marketing campaign (such as shipping/billing information) must be filtered out before being passed into the marketing app.

### Constraints

Constraints allow you to further refine when exactly Permissions and Duties are supposed to apply. If a Permission has a Constraint, it is only considered valid if that Constraint is fulfilled. If a Duty has a constraint, it only needs to be executed if that Constraint is fulfilled.

Constraints are the real meat of the policy system, allowing you to fine-tune and tailor things exactly to what your use case requires. Taking the above example of a contact app to marketing app flow, you could define that a contact data object has a Permission for the Action `marketing`, but only if that object has been assigned the category "subscriber" in its source contact management app.

Constraints are generally expressed through a simple equation, with two operands and one operator. The operands may refer to any attribute of the data value, a constant value, or an attribute of the current flow or component. The operator refers to a predefined function stored in the Governance Service. A suite of generic operator functions is already included by default, and can be easily extended with additional ones depending on your use-case.

For example, a simple Constraint might look like this:
```
{
  leftOperand: "category",
  operator: "equals",
  rightOperand: "subscriber"
}
```

This constraint checks whether the data object has a `category` attribute, and whether the value of that category equals `subscriber`.

## Policy Scenarios

A policy can be used in one of two ways: Either as an individualized policy directly attached to individual data objects, or as a general one applying equally to every object passing through a given flow.

### Individual Policies

In this case each data object (optionally) comes with a unique policy applying only to itself. This is most useful if you have a source system that already supports some sort of policy/access control itself, and requires a connector that can transform this information into a OIH-policy.

### Flow-Wide Policies

Alternatively, you can design policies that apply to every single data object passing through a flow. This is most useful if you have a Duty you would like to apply to each object (e.g. anonymizing certain personal information), or if you wish to filter the objects based on a certain constraint (e.g. discard all objects that do not have a certain property).

## Policy Definition and Application

In order to automatically parse and apply policies within a flow, certain formats and settings are required.

### Policy Format

To be parsed by the OIH, the policy must be expressed in a JSON format. There is one array each for Duties and Permissions, each containing an object detailing the Duty/Permission and any Constraints. For example:

```
{
    duty: [
      {
        action: 'anonymize',
        constraint: {
          leftOperand: 'lastName',
          operator: 'applyToKey'
        }
      }
    ],
    permission: [
      {
        action: 'distribute',
        constraint: {
          leftOperand: 'category',
          operator: 'equals',
          rightOperand: 'subscriber'
        }
      }
    ]
}
```

### Passing the Policy into a Flow

There are two ways of passing a policy into a flow, depending on whether individual or flow-wide policies are used:

#### Individual Policies

To apply individual policies, place a valid policy object into a data object's `metadata` field under the key `policy`, like so:

```
{
  metadata: {
    policy: {
      permission: [
        {
          action: 'distribute'
        }
      ]
    }
  },
  data: {
    firstName: 'Jane',
    lastname: 'Doe'
  }
}
```

This can be done either as part of the source connector, or through some other component within the flow. To actually apply the policy, you need to set two settings in the flow step's `nodeSettings`:
- `applyPolicy`: Set this to `true` to enable automatic application of individual policies
- `policyAction`: Provide the Action that this flow takes (see above). Only objects with a Permission matching this Action will pass.

 For example, in a minimized example flow could look like this:

```
{
  name: 'Policy Example Flow',
  graph: {
    nodes: [
      {
        id: 'step_1',
        name: 'Source Component'
      },
      {
        id: 'step_2',
        name: 'Target Component',
        nodeSettings: {
          applyPolicy: true,
          policyAction: 'distribute',
        }
      }
    ],
    edges: [
      {
        source: 'step_1',
        target: 'step_2'
      }
    ]
  }
}
```
Note that the policy is applied immediately *before* the data is passed into the respective step's component. So in this example, the policy would be checked after `step_1` but before `step_2`, and only objects whose policy is fulfilled are passed into the Target Comonent.

#### Flow-wide Policies

In this case, a valid policy object can be passed directly into the flow configuration. You will need to set two fields in the step's `nodeSettings`:
- `flowPolicy`: A valid policy object that should be applied to all flow's data objects
- `policyAction`: Provide the Action that this flow takes (see above)

There is no setting specifically enabling the checking of a flow policy, the presence of any `flowPolicy` object implicitly enables it.

A minimized example flow:
```
{
  name: 'Policy Example Flow',
  graph: {
    nodes: [
      {
        id: 'step_1',
        name: 'Source Component'
      },
      {
        id: 'step_2',
        name: 'Target Component',
        nodeSettings: {
          policyAction: 'distribute',
          flowPolicy: {
            permission: [
              {
                action: 'distribute',
                constraint: {
                  leftOperand: 'category',
                  operator: 'equals',
                  rightOperand: 'subscriber'
                }
              }
            ]
          }
        }
      }
    ],
    edges: [
      {
        source: 'step_1',
        target: 'step_2'
      }
    ]
  }
}
```

As described above, a flow policy is evaluated before it is passed into the step's component. So in this case it would be applied after `step_1`, but before `step_2`

### Further Reading

- Governance Service technical documentation and source code: https://github.com/openintegrationhub/openintegrationhub/tree/master/services/governance-service
