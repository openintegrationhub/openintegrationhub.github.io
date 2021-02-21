---
layout: default
title: Documents
parent: Master Data Models
nav_order: 5
grand_parent: Reference Documentation
---

![prod](https://img.shields.io/badge/Status-Production-brightgreen.svg)

# **Documents Model**

## Resources

#### UML Diagram

[Basic Version](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/DocumentModel.png){: .btn .fs-5 .mb-4 .mb-md-0 }
[Metadata](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/OIHDataModelDocumentMetadataSpecification.svg){: .btn .fs-5 .mb-4 .mb-md-0 }
[Extended Version](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/OIHDataModelDocuments.svg){: .btn .fs-5 .mb-4 .mb-md-0 }

#### JSON Schema

[Document](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/extended/Document.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Folder](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/extended/Folder.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Object](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/extended/Object.json){: .btn .fs-5 .mb-4 .mb-md-0 }
[Relation](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/extended/Relationjson){: .btn .fs-5 .mb-4 .mb-md-0 }
[Shared Definitions](https://github.com/openintegrationhub/openintegrationhub.github.io/blob/master/assets/DataModels/Documents/extended/sharedDocuments.json){: .btn .fs-5 .mb-4 .mb-md-0 }

#### Description Table

[Description Table]({{ site.baseurl }}{% link docs/ReferenceDocumentation/DataModels/Documents/DocumentDescriptionTable.md %}){: .btn .fs-5 .mb-4 .mb-md-0 }

## General Structure

One challenge of this model is to fit a wide array of services dealing with documents.
The features provided by heterogeneous services like Dropbox and other more advanced systems like SharePoint or ELO are far apart.

In order to be suitable for all scenarios, this model is split into several implementations.

- `Basic` Sharing documents between services
- `Comfort` Sharing documents and metadata between services
- `Extended` Sharing documents, metadata, policies and sub-resources between services

The `basic model` describes a basic implementation for sharing documents and files without the need of handling metadata or additional information.

![Documents Basic Version](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/DataModels/Documents/DocumentModel.png)

For the `comfort model`, Metadata definitions can be additionally queried.

![Generic Metadata](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/DataModels/Documents/OIHDataModelDocumentMetadataSpecification.png)

The `extended model` specification does contain all properties that are required in order to handle additional functionalities of DMS/ECM/EIM systems.

![Documents Extended Version](https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/DataModels/Documents/OIHDataModelDocuments.png)

The following may give you a better understanding of the variety of services and possible use cases.

### Managing documents by specialized systems

The following table classifies the complexity of document management or sharing services currently available.

| Type                     | Description                                                                                                                                                                                                                              | Example                                      |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| File storage             | A service that can store documents (folders/ documents) in a hierarchically organized structure including limited metadata capabilities                                                                                                  | FTP, S3, Network fileshare                   |
| Online file share        | A service that can store information (folders/ documents) in a hierarchically organized structure. Allows sharing content easily. Some services do provide metadata capabilities.                                                        | DropBox, OneDrive, Box                       |
| ECM/EIM/Content services | A service that captures, stores, delivers, manages and organizes information based on additional metadata or hierarchically organized structures. In most cases DMS functionalities can be seen as a fundamental part of these services. | Alfresco, ELO, M-Files, OpenText, SharePoint |

### Business services creating documents

In addition to systems that have been specifically designed to capture, store, deliver and manage documents and information, there are additional services that have been designed in order to produce content.

| Service/ System | Sample document types                   |
| --------------- | --------------------------------------- |
| Inbox services  | Incoming Invoices, Notifications, ...   |
| ERP             | Outgoing Invoices, Purchase Orders, ... |

| User Stories                                                                                            |
| :------------------------------------------------------------------------------------------------------ |
| As a user I want to automatically send an email with the invoice created by the ERP system.             |
| As a user I want to automatically store generated outgoing documents in the document management system. |
| As a user I want to use inbox services for digitalizing invoices for further processing.                |

### Business services consuming documents

From a companywide perspective, there are a variety of systems that generate or receive documents that are related to business transactions of other systems. If a document management system is used, these documents are centralized in a repository that contains information that refers to the business transaction.

Therefore ERP or CRM systems can view a list of related documents from document management systems or third party sources. The following table lists systems that create business transactions.

| Service/ System | Sample document types                                                      |
| --------------- | -------------------------------------------------------------------------- |
| CRM             | Invoices, Purchase Orders, Billing documents, E-Mails, Communication, etc. |
| ERP             | Invoices, Purchase Orders, Billing documents, etc.                         |

| User Stories                                                                                                                                                                                            |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| As a user I want to view stored documents of the current invoice transaction in the ERP system. This gives me additional information like attached terms and conditions or the related delivery docket. |
| As a user I want to add additional documents while displaying the customer record in the CRM system.                                                                                                    |
| As a user I want to pass all documents uploaded to a specific DropBox folder to be passed to the ERP system.                                                                                            |

## Further information

As an open standard we are of course trying to be compatible with existing models out there. So for this model other industry standards were analyzed. If you are interested in learning more about the concepts behind this model, check out the workgroup on GitHub.

[GitHub](https://github.com/openintegrationhub/Data-and-Domain-Models){: .btn .fs-5 .mb-4 .mb-md-0 }

Do you have ideas, requirements or suggestions? Let us know and help to make this model more powerful.

[Contribution Guide](https://github.com/openintegrationhub/Data-and-Domain-Models/blob/master/CONTRIBUTING.md){: .btn .fs-5 .mb-4 .mb-md-0 }
