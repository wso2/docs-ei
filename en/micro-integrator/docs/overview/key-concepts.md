# Key Concepts of WSO2 Micro Integrator

## Proxy Services

A Proxy service is a virtual service that receives messages and optionally processes them before forwarding to a service at a given endpoint. This approach allows you to perform the necessary message transformations and introduce additional functionality to your services without changing your actual services. 

## REST APIs

A REST API in WSO2 Micro Integrator is analogous to a web application deployed in the web container. Like [proxy services](#proxy-services), the REST API will receive messages, performs the necessary transformations and processing, and then forwards the messages to a given endpoint. Each API is anchored at a user-defined URL context, much like how a web application deployed in a servlet container is anchored at a fixed URL context. An API will only process requests that fall under its URL context. An API is made of one or more **Resources**, which are logical components of an API that can be accessed by making a particular type of HTTP call. 

## Inbound Endpoints

An inbound endpoint in WSO2 Micro Integrator receives messages and then injects them directly from the transport layer to the mediation layer without going through the Axis2 engine. One of the advantages of using Inbound Endpoints is in its ability to create inbound messaging channels dynamically.

## Mediators

Mediators are individual processing units that perform a specific function on messages that pass through the Micro Integrator. The mediator takes the message received by the proxy service or REST API, carries out some predefined actions on it (such as transforming, enriching, filtering), and outputs the modified message. 

## Mediation Sequences

A mediation sequence is a set of [mediators](#mediators) organized into a logical flow, allowing you to implement pipes and filter patterns. The mediators in the sequence will perform the necessary message processing and route the message to the required destination. 

## Message Stores and Processors

A **Message Store** is used by a [mediation sequence](#mediation-sequences) to temporarily store messages before they are delivered to their destination. This approach is useful for serving traffic to back-end services that can only accept messages at a given rate, whereas incoming traffic arrives at different rates. 

## Templates

A large number of configuration files in the form of [sequences](#mediation-sequences) and [endpoints](#endpoints), and transformations can be required to satisfy all the mediation requirements of your system. To keep your configurations manageable, it is important to avoid scattering configuration files across different locations and to avoid duplicating redundant configurations. Templates help minimize this redundancy by creating prototypes that users can use and reuse when needed. WSO2 Micro Integrator can template [sequences](#mediation-sequences) and [endpoints](#endpoints).

## Connectors

Connectors allow your mediation flows to connect and interact with external services such as Twitter and Salesforce. Typically, connectors are used to wrap the API of an external service. It is also a collection of [mediation templates](#templates) that define specific operations that should be performed on the service. Each connector provides operations that perform different actions in that service. For example, the Twitter connector has operations for creating a tweet, getting a user's followers, and more.

To download a required connector, go to the [WSO2 Connector Store](https://store.wso2.com/store).

## Endpoints

A message exit point or an endpoint defines an external destination for a message. Typically, this is the address of a [proxy service](#proxy-services), which acts as the front end to the actual service. You can configure the endpoint artifacts with any attributes or semantics needed for communicating with that service. An endpoint could represent a URL, a mailbox, a JMS queue, a TCP socket, etc. along with the settings needed for the connection. 

## Error Handling

The main role of WSO2 Micro Integrator is to act as the backbone of an organization’s service-oriented architecture. It is the spine through which all the systems and applications within the enterprise (and external applications that integrate with the enterprise) communicate with each other. 

For example, the Micro Integrator often has to deal with many wire-level protocols, messaging standards, and remote APIs. But applications and networks can be full of errors. Applications crash. Network routers and links get into states where they cannot pass messages through with the expected efficiency. These error conditions are very likely to cause a fault or trigger a runtime exception in the Micro Integrator server.

When you define a mediation sequence, [Fault Sequences](../../references/synapse-properties/sequence-properties/#fault-sequences) are used to handle messages that are affected by mediation errors. You can also handle errors at the endpoint level by configuring the endpoint properties.

## Mediation Debugging

Message mediation is one of the operational modes of WSO2 Micro Integrator where it receives messages, performs various operations (such as message filterring, message transforming, etc.) according to given parameters, and forwards the message to the intended recepient. The Micro Integrator performs message mediation through various synapse artifacts such as [mediators](#mediators), [sequences](#mediation-sequences), [endpoints](#mediators), etc.

**Mediation Debugging** is the process of analysing whether the synapse artifacts in your message flow are operating as intended, and/or whether a combination of these artifacts (taken as a whole) are operating as intended. The development tool of WSO2 Micro Integrator (**WSO2 Integration Studio**) is shipped with the [mediation debugging tool](../../develop/debugging-mediation) for this purpose. You can also use other methods such as [wire logs](../develop/using-wire-logs.md), [service logs](../../develop/enabling-logs-for-services), and [REST API logs](../../develop/enabling-logs-for-api) to debug message mediation.

## Message Builders and Formatters

When a message comes in to WSO2 Micro Integrator, the receiving transport selects a **message builder** based on the message's content type. It uses that builder to process the message's raw payload data and converts it to common XML, which the mediation engine of WSO2 Micro Integrator can then read and understand. WSO2 Micro Integrator includes
message builders for text-based and binary content.

Conversely, before a transport sends a message out from WSO2 Micro Integrator, a **message formatter** is used to build the outgoing stream from the message back into its original format. As with message builders, the message formatter is selected based on the message's content type. You can implement new message builders and formatters for custom requirements.

## Message Tracing 

Message tracing helps you to track issues after an integration process finishes and thereby, allows you to identify and fix issues by identifying the root cause. It is used to trace, track and visualize a body of a message in each intermediate stage of its transmission. This is useful for a number of reasons, including auditing and debugging.

[Endpoints](#endpoints) have a trace attribute, which turns on detailed trace information for messages being sent to the endpoint. These are available in the trace.log file, which is configured in the `MI_HOME/repository/conf/log4j.properties` file. Setting the trace log level to TRACE logs detailed trace information including message payloads.

## Transports

A transport protocol is responsible for carrying messages that are in a specific format. WSO2 Micro Integrator supports all the widely used transports including HTTP/S, JMS, VFS, as well as domain-specific transports like FIX. Each transport provides a receiver implementation for receiving messages, and a sender implementation for sending messages.

## Registry

WSO2 Micro Integrator uses a registry to store various configurations and artifacts such as [sequences](#mediation-sequences) and [endpoints](#endpoints). A registry is simply a content store and a metadata repository. Various SOA artifacts such as services, WSDLs, and configuration files can be stored in a registry and referred to by a key, which is a path similar to a UNIX file path. The WSO2 Micro Integrator uses a [file-based registry](../../setup/file_based_registry) that is configured by default. When you develop your integration artifacts, you can also define and use a [local registry](../../develop/creating-artifacts/registry/creating-local-registry-entries).

## Data Services 

The data in your organization can be a complex pool of information that is stored in heterogeneous systems, ranging from RDBMSs to Excel files, and Google spreadsheets, etc. Data services are created for the purpose of decoupling the data from its infrastructure. In other words, when you create a data service in WSO2 Micro Integrator, the data that is stored in a storage system (such as an RDBMS) can be exposed in the form of a service. This allows users (that may be any application or system) to access the data without interacting with the original source of the data. Data services are, thereby, a convenient interface for interacting with the database layer in your organization.

A data service in WSO2 Micro Integrator is a SOAP-based web service by default. However, you also have the option of creating REST resources, which allows applications and systems consuming the data service to have both SOAP-based, and RESTful access to your data.

### Datasources

Your organization's data can be stored in various data storage systems, which are the datasources. The following datasources are supported: Relational databases, CSV files, Microsoft Excel Sheets, Google Spreadsheets, RDF, MongoDB, Cassandra, and Web Resources. Additionally, you can also useJNDIdatasources, and create custom datasources.

### RESTful data services

You can enable RESTful access to your data by defining RESTful resources for the relevant data in your data service. REST resources in the Micro Integrator support both JSON and XML media types out of the box. Therefore, a resource can receive requests and send responses in either medium. You can secure your resources with HTTP(S) Basic Auth via [WSO2 Identity Server](http://wso2.com/products/identity-server/).

### OData Services

RESTful data services in WSO2 Micro Integrator supports OData ([OData](http://www.odata.org/) protocol version 4 - OASIS standards),
which makes RESTful data access easier. In a normal data service, you
will write SQL queries for CRUD operations that will be performed on the
data. In other words, to be able to GET, UPDATE, POST, or DELETE data in
a database, the data service should have separate SQL queries written
for each purpose. However, when you enable OData for your RESTful data
service, these CRUD operations will be enabled automatically, which
allows RESTful data access using CRUD operations out of the box.

Currently, OData support is only available for RDBMS datasources and Cassandra datasources using the following endpoint: `http://localhost:9763/odata/t/{tenantId}/{dataserviceName}/{datasourceId}/`

### Data Federation

A data service defined in WSO2 Micro Integrator has the ability to aggregate the data that is stored in various, disparate datasources and present the data as a single output. For example, the data of employees in a company may be stored in various data stores (details of employment history, details of the physical office, contact information, etc.). Data federation allows users to consume all this data through a single request to the data service. The data service will aggregate the relevant data from each of the disparate datasources and present it as one response to the request. Data federation can be achieved in two ways:

-   Expose multiple datasources using a single data service.
-   Use Nested Queries in your data service. This will allow you to feed
    the result you get from one query as input to another query. That
    is, data can be combined into a single response or resource.

### Distributed Transactions

A distributed transaction is a set of operations that should be performed on two or more distributed RDBMS data stores. If the operation on one data store (node) fails, the entire set of operations will fail in all the data stores. In other words, a distributed transaction is an example of a batch process, where multiple requests are grouped into one server call and processed as one unit by the data service.

Data services in WSO2 Micro Integrator supports distributed transactions, which allows
data consumers to perform such transactions easily by using one data
service as the interface. Note that distributed transactions can only be
performed for IN-ONLY operations that will insert, update, or delete
data in the data stores. These are not applicable to operations for
retrieving data.

A transaction manager is set up in the middle of these transactions for
effective coordination and management. This feature uses the Java
Transaction API (JTA), which allows distributed transactions to be
carried out across multiple XA resources in a Java environment. You can
also override this transaction manager.

### Batch Processing

A data service is an interface that receives requests from data
consumers and performs the requested tasks in the relevant data stores.
Batch processing allows a data service to group multiple requests into a
batch and process it as a single request. Batch processing can only be
used for IN-ONLY operations that will insert, update, or delete data in
the data stores, and not for operations that retrieve data.

Data services in WSO2 Micro Integrator support two scenarios of batch requesting: Client-side batch requests and server-side batch requests.

For example, consider the task of entering details of new employees into
a database table. Typically, the client consuming the data can do this
by sending separate requests with each employee record. Alternatively,
the client can group the individual requests into a single batch, and
send one batch request to the data service. In this example, the data
service will have one operation defined for inserting data into the
database. However, when batch processing is enabled, it is possible to
insert multiple records into that database, using this operation.
Therefore, the client can invoke this operation using a single request,
to insert multiple records. This is client-side batch requesting.

Consider another example, where the client needs to enter the employee’s
bank details along with the personal details, but the bank details
should be inserted to a different data store. In this example, the data
service will have two separate operations for inserting data into two
separate data stores, and the client is invoking both operations using
a single request (also called a request box). This is server-side batch
requesting.

Note that batch requests are transactional if the data store is an RDBMS or another system that supports transactions. Transactional requests succeed or fail as a batch. That is, if one individual request
fails, all the requests in the batch will fail to make sure that the
data is synchronized. Server-side batch requests work for local
transactions (performed on one node of the data store), as well as distributed transactions (performed on multiple nodes of the data store).

### Data Transformation

XSLT transformation is used in data services to transform the result of an already defined operation into a different result. The user can define the transformation xslt and provide the url of the transformation file in the result element.

### Managed Data Access

Most businesses require secure and managed data access across these federated data stores.

### Streaming

Data service streaming helps manage large data chunks sent back to the
client by the data service as the response to a request. When streaming
is enabled, the data is sent to the client as it is generated without
memory building up in the server. By default, streaming is enabled in
data services.

### Namespaces

The service namespace uniquely identifies a Web service and is specified by the `<targetNamespace>` element in the WSDL that
represents the service. A data service is simply a Web service with
specialized functionality. When developing a data service, you get to
apply namespaces at various levels. As a data service implementation is
based on XML, namespace handling is useful for making sure that there
are no conflicting element names in the XML. Although namespaces are
optional for data services, in some scenarios they are necessary.
