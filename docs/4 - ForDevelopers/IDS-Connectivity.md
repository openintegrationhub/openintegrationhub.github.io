---
layout: default
title: IDS-Connectivity
nav_order: 5
parent: For Developers
---

# IDS-Connectivity

As part of a Fraunhofer IAIS study the OIH now offers basic IDS ([International Data Spaces](https://internationaldataspaces.org/)) 
connectivity. The following sections describe which components are necessary for registering data with the DSC 
([Dataspace Connector](https://www.dataspace-connector.io/)) and IDS broker, how these components work together and how 
to integrate these functionalities into OIH flows.  

**Table of contents:**

- [Components](#components)
- [Concept](#concept)
- [Flow examples](#flow-examples)

## Components

In order to realize some basic IDS connectivity the following components are necessary:
- [IDS-Wrapper](https://github.com/openintegrationhub/IDS-wrapper)
- [OIH-IDS Gateway](https://github.com/openintegrationhub/IDS-gateway)
- [IDS-SQL Adapter](https://github.com/openintegrationhub/IDS-SQL-Adapter)

## Concept

<img src="https://raw.githubusercontent.com/openintegrationhub/openintegrationhub.github.io/master/assets/images/ids-overview.png" alt="IDS-Overview">

The above diagram gives an example of how an interaction with the IDS can be realized and follows these steps:

**Register data:**<br>
**1.** Flow 1 is triggered by a webhook or cron job event and receives or polls data from a data source.<br>
**2.** Each row of data is described with a UID and saved in an SQL database.<br>
**3.** The data row is registered with the DSC (Dataspace connector/provider) via POST request.<br>
**4.** The data row is registered with the IDS broker via POST reuqest to DSC.<br> 
**5.** DSC registers the data.<br>
**6.** The data can be discovered by an IDS consumer.<br>

**Request data:**<br>
**7.** An IDS consumer requests data by using the UID.<br>
**8.** DSC requests the data by sending a GET request containing the flow ID to the IDS-Wrapper.<br>
**9.** IDS-Wrapper triggers flow 2 by a POST request containing the data UID.<br>
**10.** Flow 2 fetches the data from the database.<br>
**11.** The fetched data is sent back to the IDS-Wrapper via POST request.<br>
**12.** IDS-Wrapper sends the data back to the DSC as a GET response.<br>
**13.** DSC responds to the IDS broker by sending the data artifact.<br>
**14.** IDS broker sends requested data back to the IDS consumer. <br>

The actions and triggers used within the flows are explained in more detail in the repository READMEs of the single 
components listed under [Components](#components).

## Flow examples
The following section gives an example of how to configure the two flows seen in the diagram under [Concept](#concept). 
They both use the IDS-Gateway and IDS-SQL Adapter to interact with the other components of the IDS ecosystem. In Flow 1 
of this test scenario data is fetched from an SQL database, described with a UID, written to another SQL database and then 
registered with the DSC and IDS broker. The process of requesting the data by providing a UID is performed in Flow 2.  

### **Flow 1** 
  
The first flow is triggered by a webhook or cron job event, fetches data and registers it with the DSC and IDS-broker by
describing it with an individual UID. The POST request that registers the data contains the endpoint address of Flow 2 
combined with the UID of the data set
<br>(i.e. `OIH-Address/hook/60e7eec81eee16001b551637&filter="117fdf380c983d6acf80d63d96c504b1"`).

**Example of Flow 1 config:**
```
{
  status: 'inactive',
  name: 'SQL to SQL to DSC (Flow 1)',
  description: 'This is an example flow to use the OIH-IDS Gateway in combination with the IDS-SQL adapter',
  graph: {
    nodes: [
      {
        id: 'step_1',
        componentId: '',
        name: 'Connector X',
        function: 'getData',
        description: 'Data source component',
        fields: {
        }
      },
      {
        id: 'step_2',
        componentId: '',
        name: 'IDS-SQL Adapter',
        function: 'addData',
        description: 'Extracts predefined columns from incoming data row and stores it in database with an individually generated UID',
        fields: {
          databaseType: '',
          user: '',
          password: '',
          databaseUrl: '',
          port: '',
          databaseName: '',
          query: 'INSERT INTO oih_ids_iot_test_data_sink(uidIDS,Column2,Column3) VALUES (\'{uidIDS}\',\'{VALUE1}\',\'{VALUE2}\')'
        }
      },
      {
        id: 'step_3',
        componentId: '',
        name: 'OIH-IDS Gateway',
        function: 'registerRessource',
        description: 'Registers incoming data with the DataSpaceConnector and IDS Broker',
        fields: {
          urlDataSpaceConnector: 'https://DataSpaceConnector-ADDRESS/admin/api/resources/resource',
          urlBroker: 'https://DataSpaceConnector-ADDRESS/admin/api/broker/register?broker=https://IDS-Broker-ADDRESS/infrastructure',
          user: '',
          password: '',
          body: {
            title: 'MachineX-Data',
            description: 'Aggregated sensor data',
            representations: [
              {
                type: 'JSON',
                name: 'MaschineX',
                source: {
                  type: 'http-get',
                  url: 'https://IDS-Wrapper-ADDRESS?endpoint=OIH-ADDRESS/hook/FLOW2-ID&uid={uidIDS}'
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
      },
      {
        source: 'step_2',
        target: 'step_3'
      }
    ]
  },
  owners: [
    {
      id: '',
      type: 'user'
    },
    {
      id: '',
      type: 'user'
    }
  ],
  createdAt: '',
  updatedAt: '',
  id: ''
}
```
### **Flow 2**

The second flow is webhook-based and triggered by a GET request that contains the resource UIDs as filter parameter. It 
then creates an SQL query to fetch the data and sends it back to the DSC.

**Example of Flow 2 config:**
```
{
  status: 'inactive',
  name: 'OIH-IDS Gateway (Flow 2)',
  description: 'This is a webhook-based flow and expects a filter with uids of the resources',
  graph: {
    nodes: [
      {
        id: 'step_1',
        componentId: '',
        name: 'OIH-IDS Gateway',
        function: 'getRessourceQuery',
        description: 'Creates a SQL query from incoming data',
        fields: {
          query: 'SELECT * FROM public.oih_ids_iot_test_data_sink WHERE uidids in ({filter})'
        }
      },
      {
        id: 'step_2',
        componentId: '',
        name: 'IDS-SQL Adapter',
        function: 'getDataPolling',
        description: 'Exemplary flow node',
        fields: {
          databaseType: 'postgresql',
          user: '',
          password: '',
          databaseUrl: '',
          port: '',
          databaseName: ''
        }
      },
      {
        id: 'step_3',
        componentId: '',
        name: 'OIH-IDS Adapter',
        function: 'postIDS',
        description: 'Sends incoming data to IDS-Wrapper',
        fields: {
          uri: 'http://IDS-Wrapper-ADDRESS/webhook'
        }
      }
    ],
    edges: [
      {
        source: 'step_1',
        target: 'step_2'
      },
      {
        source: 'step_2',
        target: 'step_3'
      }
    ]
  },
  owners: [
    {
      id: '',
      type: 'user'
    }
  ],
  createdAt: '',
  updatedAt: '',
  id: ''
}
```


