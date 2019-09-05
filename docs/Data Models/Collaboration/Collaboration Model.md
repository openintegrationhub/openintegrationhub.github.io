---
layout: default
title: Collaboration
parent: Master Data Models
has_children: true
nav_order: 4
---

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Collaboration Model**

## Resources

#### UML Diagram
[UML Diagram](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Collaboration/OIH_collaboration.png){: .btn .fs-5 .mb-4 .mb-md-0 }
#### JSON Schema
[Collabrotation](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/collaborationElement.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Calendar](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/calendarEvent.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[e-mail](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/email.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[TaskToTaskRelation](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/TaskToTaskRelationjson){: .btn .fs-5 .mb-4 .mb-md-0 }
#### Description Table
[Description Table](https://openintegrationhub.github.io//docs/Data%20Models/Collaboration/CollaborationDescriptionTable.html){: .btn .fs-5 .mb-4 .mb-md-0 }


## General Structure

Collaboration is a broad term. This Data Model represents three key areas of collaboration: `e-mail`, `calendar events` and `tasks`.

Through community feedback these were identified as the most important models in the collaboration domain.

Due to the extensive professional use of Microsoft Outlook the chosen e-mail standard is derived mostly from Outlook
An existing standard for calendar events is "iCalender" (RFC 5545). The properties of "iCalender" are incorporated and extended.

In all three areas it is important to know which person created a certain element, when and with which properties. Therefore all three models have identical properties and are set in relation to a central element.

The models inherit the most important properties from this main `collaboration element`.

![Collaboration](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Collaboration/OIH_collaboration.png)



## Further information
As an open standard we are of course trying to be compatible with existing models out there. So for this model other industry standards were analyzed. If you are interested in learning more about the concepts behind this model, check out the workgroup on GitHub.

[GitHub](https://github.com/openintegrationhub/Data-and-Domain-Models){: .btn .fs-5 .mb-4 .mb-md-0 }

Do you have ideas, requirements or suggestions? Let us know and help to make this model more powerful.

[Contribution Guide](https://github.com/openintegrationhub/Data-and-Domain-Models/blob/master/CONTRIBUTING.md){: .btn .fs-5 .mb-4 .mb-md-0 }
