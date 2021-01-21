---
layout: default
title: Addresses
parent: Master Data Models
nav_order: 1
grand_parent: Reference Documentation
---

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Address Model**

## Resources

<!-- 2 sentences: what does it do and how -->

#### UML Diagram

[UML Diagram](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Addresses/MasterDataModelAddress.svg){: .btn .fs-5 .mb-4 .mb-md-0 }

#### JSON Schema

[Person](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Addresses/personV2.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Organization](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Addresses/organizationV2.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Relation](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Addresses/relationsV2.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Shared Definitions](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Addresses/sharedDefinitionsV2.json){: .btn .fs-5 .mb-4 .mb-md-0 }

#### Description Table

[Description Table](https://openintegrationhub.github.io//docs/Data%20Models/Addresses/AddressDescriptionTable.html){: .btn .fs-5 .mb-4 .mb-md-0 }

## General Structure

The addresses data model consists of different types of objects: `organizations` and `persons`. A `relations` object is used to describes the connections between other objects.

![](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/DataModels/Addresses/MasterDataModelAddress.png)

## Relations

Generally there are three types of relations:

- Persons to organizations
- Persons to persons
- Organizations to organizations

Creating a specific `relation` object allows for a more individualized expression of connection between entities than linking them directly. You can name a `relation` and thus categorize or tag relations. This way you could use the relation to describe a persons function within a company, a companies relationship with other companies (like subsidiaries or suppliers) as well as connection between real people.

You can solve for example the following user stories:

| User Stories                                                                                                          |
| :-------------------------------------------------------------------------------------------------------------------- |
| As a user I want to see the structure of a group of companies, to get a better overview of my business dealings.      |
| As a user I want to assign one or more persons to an organization to communicate with all contacts of an organization |
| As a user I want to assign relations between contact persons of my customers to get an overview of their hierarchy.   |
| As a user I want to see, if a specific person in an organization is the same person as in another organization        |

## Duplicates

In order to reflect that some people in the real world have multiple roles in one or many different organizations, or that one organization might have several locations, we allow duplicates and link them by modeling the relation.

> Suppose you have a person who is the CEO of a company and he is also board member of an association.
> If you want to mail him in his role as a CEO you want to use the company mail address.
> If you want to mail him as a board member, you want to use the mail address from the association.
> For the example above, we will store the person with his contact details for his CEO role in the company and store another entity of this person for his role as a board member in the association. Then we link them via a `relation` and describe that relation as **same person-relation**.

Some tools label the different kinds of mail accounts, but this does not scale.

Here are some other example use cases you can solve the same way:

| User Stories                                                                                                                                           |
| :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| As a user I want to see the function of a person in his organization.                                                                                  |
| As a user I want to assign a person to different organizations with different contact data to see different roles of the same person.                  |
| As a user I want to store same persons in different organizations, to get in contact with them currently at the time with the particular contact data. |
| As a user I want to store an organization, with several offices in different locations.                                                                |

## Further information

As an open standard we are of course trying to be compatible with existing models out there. So for this model other industry standards were analyzed. If you are interested in learning more about the concepts behind this model, check out the workgroup on GitHub.

[GitHub](https://github.com/openintegrationhub/Data-and-Domain-Models){: .btn .fs-5 .mb-4 .mb-md-0 }

Do you have ideas, requirements or suggestions? Let us know and help to make this model more powerful.

[Contribution Guide](https://github.com/openintegrationhub/Data-and-Domain-Models/blob/master/CONTRIBUTING.md){: .btn .fs-5 .mb-4 .mb-md-0 }
