# Key Concepts

Following are definitions of some of the concepts and terminology
associated with each of the profiles of WSO2 EI .

#### Load balancing

The load balancer automatically distributes incoming traffic across
multiple WSO2 product instances. It enables you to achieve greater
levels of fault tolerance in your cluster and provides the required
balancing of load needed to distribute traffic. For more information,
see [Clustering the ESB
Profile](https://docs.wso2.com/display/EI611/Clustering+the+ESB+Profile)
.

![load balancing](attachments/119129502/119129509.png "load balancing")

------------------------------------------------------------------------

Following are the key concepts with respect to the key
constructs/artifacts of WSO2 EI.

![constructs of WSO2
EI](attachments/119129502/119129513.png "constructs of WSO2 EI")

#### Message entry points

##### Proxy services

Proxy services are virtual services that receive messages and optionally
process them before forwarding them to a service at a given
[endpoint](https://docs.wso2.com/display/EI611/Working+with+Endpoints) .
This approach allows you to perform necessary transformations and
introduce additional functionality without changing your existing
service. Any available transport can be used to receive and send
messages from the proxy services. A proxy service is externally visible
and can be accessed using a URL similar to a normal web service address.

For more information, see [Working with Proxy
Services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
.

##### REST APIs

A REST API in Enterprise Integrator is analogous to a web application
deployed in the Enterprise Integrator runtime. Each API is anchored at a
user-defined URL context, much like how a web application deployed in a
servlet container is anchored at a fixed URL context. An API will only
process requests that fall under its URL context. A REST API defines one
or more resources, which is a logical component of an API that can be
accessed by making a particular type of HTTP call.

A REST API resource is used by the WSO2 Enterprise Integrator mediation
engine to mediate incoming requests, forward them to a specified
endpoint, mediate the responses from the endpoint, and send the
responses back to the client that originally requested them. We can
create an API resource to process defined HTTP request method/s
that are sent to the back-end service. The In sequence handles incoming
requests and sends them to the back-end service, and the Out sequence
handles the responses from the back-end service and sends them back to
the requesting client.

REST APIs allow you to send messages directly into the Enterprise
Integrator using REST.

For more information, see [Working with
APIs](https://docs.wso2.com/display/EI650/Working+with+APIs) .

##### Inbound endpoints

An inbound endpoint is a message source that can be configured
dynamically. In the Enterprise Integrator, when it comes to the existing
Axis2 based transports, only the HTTP transport works in
a multi-tenant mode. Inbound endpoints support all transports to work in
a multi-tenant mode.

For more information, see [Working with Inbound
Endpoints](https://docs.wso2.com/display/EI650/Working+with+Inbound+Endpoints)
.

##### Tasks

A task allows you to run a piece of code triggered by a timer. WSO2
Enterprise Integrator provides a default task implementation, which you
can use to inject a message to the Enterprise Integrator at a scheduled
interval. You can also write your own custom tasks by implementing a
Java interface.

For more information, see [Scheduling ESB
Tasks](https://docs.wso2.com/display/EI650/Scheduling+ESB+Tasks) .

#### Message processing units

##### Mediators

Mediators are individual processing units that perform a specific
function, such as sending, transforming, or filtering messages. WSO2
Enterprise Integrator includes a comprehensive mediator library that
provides functionality for implementing widely used [Enterprise
Integration Patterns
(EIPs)](https://docs.wso2.com/display/EI650/Enterprise+Integration+Patterns)
. You can also easily write a custom mediator to provide additional
functionality using various technologies such as Java, scripting, and
Spring.

For more information, see [ESB
Mediators](https://docs.wso2.com/display/EI650/ESB+Mediators) .

##### Sequences

A sequence is a set of mediators organized into a logical flow, allowing
you to implement pipes and filter patterns. You can add sequences to
proxy services and REST APIs.

For more information, see [Mediation
Sequences](https://docs.wso2.com/display/EI650/Mediation+Sequences) .

#### Message exit points

A message exit point or an endpoint defines an external destination for
a message. An endpoint can connect to any external service after
configuring it with any attributes or semantics needed for communicating
with that service. For example, an endpoint could represent a URL,a
mailbox, a JMS queue, or a TCP socket, along with the settings needed to
connect to it.

You can specify an endpoint as an [address
endpoint](https://docs.wso2.com/display/EI650/Address+Endpoint) , [WSDL
endpoint](https://docs.wso2.com/display/EI650/WSDL+Endpoint) , a load
balancing endpoint and more. An endpoint is defined independently of
transports, allowing you to use the same endpoint with multiple
transports . When you configure a message mediation sequence or a proxy
service to handle the incoming message, you specify which transport to
use and the endpoint where the message will be sent.

For more information, see [Working with
Endpoints](https://docs.wso2.com/display/EI650/Working+with+Endpoints) .

#### Message stores and processors

Message stores and message processors are used to store and forward
messages while guaranteeing reliable message delivery. For more
information, see [Working with Message Stores and Message
Processors](https://docs.wso2.com/display/EI650/Working+with+Message+Stores+and+Message+Processors)
.

Templates

Templates help you to manage your configurations without scattering or
duplicating them by creating prototypes that you can use and reuse when
required. Templates improve re-usability and readability of your ESB
configurations (XML files). There are two types of templates available
in the ESB profile as Sequence templates and Endpoint templates. For
more information on templates, see [Working with
Templates](https://docs.wso2.com/display/EI650/Working+with+Templates) .

#### Connectors

A connector is a collection of templates that define operations that can
be called from the Enterprise Integrator and is used when connecting the
Enterprise Integrator to external third party APIs. WSO2 Enterprise
Integrator provides a variety of connectors via the [WSO2 Connector
Store](https://store.wso2.com/store/pages/top-assets) .

For information on using a connector in your EI configuration, see
[Using the Gmail
Connector](https://docs.wso2.com/display/EI650/Using+the+Gmail+Connector)
.

#### Transports

A transport is responsible for carrying messages that are in a specific
format. The Enterprise Integrator supports all the widely used
transports including HTTP/s, JMS, VFS and domain-specific transports
like FIX. You can easily add a new transport using the Axis2 transport
framework and plug it into the Enterprise Integrator. Each transport
provides a receiver, which the Enterprise Integrator uses to receive
messages, and a sender, which it uses to send messages. The transport
receivers and senders are independent of the Enterprise Integrator core.

For more information, see [Carrying
Messages](https://docs.wso2.com/display/EI650/Carrying+Messages) .

#### Message builders and formatters

When a message comes into the Enterprise Integrator, the receiving
transport selects a **message builder** based on the message's content
type. It uses that builder to process the message's raw payload data and
convert it into common XML, which the Enterprise Integrator mediation
engine can then read and understand. WSO2 Enterprise Integrator includes
message builders for text-based and binary content.

Conversely, before a transport sends a message out from the Enterprise
Integrator, a **message formatter** is used to build the outgoing stream
from the message back into its original format. As with message
builders, the message formatter is selected based on the message's
content type.

You can implement new message builders and formatters using the Axis2
framework. For more information, see [Working with Message Builders and
Formatters](https://docs.wso2.com/display/EI650/Working+with+Message+Builders+and+Formatters)
.

------------------------------------------------------------------------

#### Applying security to artifacts

You can apply security to artifacts of the ESB profile using WSO2
Integration Studio. The Quality of Service (QoS) component implements
security. For more information, see [Applying Security to a Proxy
Service](https://docs.wso2.com/display/EI650/Applying+Security+to+a+Proxy+Service)
.

------------------------------------------------------------------------

#### Logging messages

You can use the Log mediator to log mediated messages. For more
information on the usage of the log mediator, see [Log
Mediator](https://docs.wso2.com/display/EI650/Log+Mediator) .

------------------------------------------------------------------------

#### Message tracing

Message tracing helps you to track issues after an integration process
finishes and thereby, allows you to identify and fix issues by
identifying the root cause. It is used to trace, track and visualize a
body of a message in each intermediate stage of its transmission. T his
is useful for a number of reasons, including auditing and debugging. For
instructions on how to do message tracing, see [Monitoring WSO2 EI with
EI
Analytics](https://docs.wso2.com/display/EI620/Monitoring+WSO2+EI+with+EI+Analytics#MonitoringWSO2EIwithEIAnalytics-Step5-Analyzestatistics)
.

------------------------------------------------------------------------

#### Debugging mediation

Message mediation mode is one of the operational modes of WSO2 EI where
EI functions as an intermediate message router. A unit of the mediation
flow is a mediator. A sequence is a series of mediators, where each
mediator is a unit entity that can input a message, carry out a
predefined processing task on the message, and output the message for
further processing. Debugging is where you want to know if these units,
which function as separate entities are operating as intended, or if a
combination of these units are operating as a whole as intended. For
more information, see [Debugging
Mediation](https://docs.wso2.com/display/EI650/Debugging+Mediation) .


#### Enterprise Integration Patterns

Enterprise Application Integration (EAI) enables you to connect business
applications with heterogeneous systems. The [EIP patterns
guide](https://docs.wso2.com/display/EIP/Enterprise+Integration+Patterns+with+WSO2+Enterprise+Integrator)
demonstrates how the patterns that are invented integration solution
architects over the years can be simulated using various constructs in
the ESB profile.


#### ESB tooling

You can use WSO2 Integration Studio to create various integration
artifacts that you can build and deploy to the ESB profile of WSO2 EI in
order to process requests.

<!--

#### Data service

The data in your organization can be a complex pool of information that
is stored in heterogeneous systems, ranging from RDBMSs to Excel files,
and Google spreadsheets, etc. Data services are created for the purpose
of decoupling the data from its infrastructure. In other words, when you
create a data service in WSO2 EI, the data that is stored in a storage
system (such as an RDBMS) can be exposed in the form of a service. This
allows users (that may be any application or system) to access the data
without interacting with the original source of the data. Data services
are, thereby, a convenient interface for interacting with the database
layer in your organization.

A data service in WSO2 EI is a SOAP-based web service, by default.
However, you also have the option of creating REST resources. Therefore,
the applications and systems consuming the data service can have both
SOAP-based, and RESTful access to your data.

#### Datasources

Your organization's data can be stored in various data storage systems,
which are thedatasources. Data services in WSO2 EI support the
followingdatasources: Relational databases, CSV files, Microsoft Excel
Sheets, Google Spreadsheets, RDF, MongoDB, Cassandra, and Web Resources.
Additionally, you can also useJNDIdatasources, and create
customdatasources. Read about using variousdatasourceswith data services
defined in WSO2 EI.

#### RESTful data services

A data service exposes your data (stored in various data stores) as a
service. You can enable RESTful access to your data, by defining RESTful
resources, for the relevant data, in your data service. REST resources
in WSO2 EI support both JSON and XML media types out of the box.
Therefore, a resource can receive requests, and send responses in either
medium. Secure resources with HTTP(S) Basic Auth integrated to
enterprise identity systems (via [WSO2 Identity
Server](http://wso2.com/products/identity-server/) ).

#### OData services

RESTful data services in WSO2 EI supports OData (
[OData](http://www.odata.org/) protocol version 4 - OASIS standards) ,
which makes RESTful data access easier. In a normal data service, you
will write SQL queries for CRUD operations that will be performed on the
data. In other words, to be able to GET, UPDATE, POST, or DELETE data in
a database, the data service should have separate SQL queries written
for each purpose. However, when you enable OData for your RESTful data
service, these CRUD operations will be enabled automatically, which
allows RESTful data access using CRUD operations out of the box.

Currently, OData support is only available for RDBMS datasources and
Cassandra datasources. Odata services will now be accessible from the
following endpoints:

-   For super tenant:
    http://localhost:9763/odata/{dataserviceName}/{datasourceId}/
-   For normal tenants:
    http://localhost:9763/odata/t/{tenantId}/{dataserviceName}/{datasourceId}/

#### Data Federation

A data service defined in WSO2 EI has the ability to aggregate the data
that is stored in various, disparate datasources, and present the
aggregated data as a single output. For example, the data of employees
in a company may be stored in various data stores (details of employment
history, details of the physical office, contact information, etc.).
Data federation allows users to consume all this data through a single
request to the data service. The data service will aggregate the
relevant data from each of the disparate datasourcesand present it as
one response to the request. Data federation can be achieved in two
ways:

-   Expose multiple datasources using a single data service.
-   Use Nested Queries in your data service. This will allow you to feed
    the result you get from one query as input to another query. That
    is, data can be combined into a single response or resource.

#### Distributed transactions

A distributed transaction is a set of operations that should be
performed on two or more, distributed RDBMS data stores. If the
operation on one data store (node) fails, the entire set of operations
will fail in all the data stores. In other words, a distributed
transaction is an example of a batch process, where multiple requests
are grouped into one server call and processed as one unit by the data
service.

Data services in WSO2 EI support distributed transactions, which allows
data consumers to perform such transactions easily by using one data
service as the interface. Note that distributed transactions can only be
performed for IN-ONLY operations that will insert, update, or delete
data in the data stores. These are not applicable to operations for
retrieving data.

A transaction manager is set up in the middle of these transactions for
effective coordination and management. This feature uses Java
Transaction API (JTA), which allows distributed transactions to be
carried out across multiple XA resources in a Java environment. You can
also override this transaction manager.

#### Batch processing

A data service is an interface that receives requests from data
consumers and performs the requested tasks in the relevant data stores.
Batch processing allows a data service to group multiple requests into a
batch and process it as a single request. Batch processing can only be
used for IN-ONLY operations that will insert, update, or delete data in
the data stores, and not for operations that retrieve data.

Data services in WSO2 EI support two scenarios of batch requesting:
Client-side batch requests, and server-side batch requests.

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
should be insertedtoa different data store. In this example, the data
service will have two separate operations for inserting data into two
separate data stores, and the client is invoking both operations, using
a single request (also called a request box). This is server-side batch
requesting.

Note that batch requests are transactional if the data store is
anRDBMS, or another system that supports transactions. Transactional
requests succeed or fail as a batch. That is, if one individual request
fails, all the requests in the batch will fail to make sure that the
data is synchronized. Server-side batch requests work for local
transactions (performed on one node of the data store), as well as
distributed transactions (performed on multiple nodes of the data
store).

#### Data transformation

XSLT transformation is used in data services to transform the result of
an already defined operation into a different result. The user can
define the transformation xslt and provide the url of the transformation
file in the result element.

#### Managed data access

Most businesses require secure and managed data access across these
federated data stores .

#### Streaming

Data service streaming helps manage large data chunks sent back to the
client by the data service as the response to a request. When streaming
is enabled, the data is sent to the client as it is generated, without
memory building up in the server. By default, streaming is enabled in
data services.

#### Namespaces

The service namespace uniquely identifies a Web service and is specified
by the `         <targetNamespace>        ` element in the WSDL that
represents the service. A data service is simply a Web service with
specialized functionality. When developing a data service, you get to
apply namespaces at various levels. As a data service implementation is
based on XML, namespace handling is useful for making sure that there
are no conflicting element names in the XML. Although namespaces are
optional for data services, in some scenarios they are necessary. For
more information, see Defining Namespaces .

#### Error Handling

The main role of WSO2 Enterprise Integrator (WSO2 EI) is to act as the
backbone of an organization’s service-oriented architecture. It is the
spine through which all the systems and applications within the
enterprise (and external applications that integrate with the
enterprise) communicate with each other. For example, an ESB (which is
contained in WSO2 EI) often has to deal with many wire-level protocols,
messaging standards, and remote APIs. But applications and networks can
be full of errors. Applications crash. Network routers and links get
into states where they cannot pass messages through with the expected
efficiency. These error conditions are very likely to cause a fault or
trigger a runtime exception in the ESB.
-->
