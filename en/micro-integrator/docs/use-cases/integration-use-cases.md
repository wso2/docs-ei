# Integration Use Cases

WSO2 Micro Integrator helps you build and execute the following use cases.


## Integrating systems that communicate in heterogeneous message formats

The integration of systems that communicate in various message formats is a common business case in 
enterprise integration. Some intermediary system in the middle has to bridge the communication gap among the systems.

Let's consider a service that returns data in XML format. 
Assume that this service needs to be consumed by a mobile client, which accepts messages only in JSON format. 
To allow these two systems to communicate, the intermediary needs to convert message formats during the communication. 
This allows the systems to communicate with each other without depending on the message formats supported 
by each system.

![message transformation](../assets/img/use-cases-overview/message-transformation-new.png) 

## Bridging systems that communicate in different protocols (Protocol translation)

The Micro Integrator offers a wide range of integration capabilities from simple message routing to 
complicated systems that use integrated solutions. Different applications typically use different protocols for 
communication. Therefore, for two systems to successfully communicate, we should translate the protocol 
(that passes from one system) to the protocol compatible with the receiving application.

![protocol switching new](../assets/img/use-cases-overview/protocol-switching-new.png)

A useful feature of the WSO2 Micro Integrator is the ability to switch protocols. 
It can take a message that comes via HTTP and send it out to a JMS queue. 
Another example that demonstrates the power of this together with transformations feature is the ability to take 
messages that come in from one protocol like HTTP, process the content of this message and send this content out 
in a completely different message format and protocol.


## Connecting Web APIs/Cloud Services

One of the most expected features from an integration solution is 'Hybrid Cloud Integration' i.e., the ability 
to connect to third-party applications via public APIs that are exposed by the application developers where the 
external applications could either be in the cloud (SaaS) or on-premise. 
WSO2 Micro Integrator brings you in this capability by means of its wide range of connectors.

A connector is a collection of templates that define operations users can call from their mediation configurations 
to easily access specific logic for processing messages. Generally, a connector wraps the API of an external service 
and gives out a more convenient interface to the integration developer.
 
WSO2 Micro Integrator supports the complete list of connectors (number exceeds 150) provided with 
WSO2 ESB where the connectors for Salesforce, Gmail, Amazon S3, are among the most popular. 
Connectors can be downloaded from the WSO2 connector store. 
It is also possible to write your own custom connector to integrate WSO2 Micro Integrator with some new system.

## Gateway

Gateway pattern is used to expose business APIs, exposing business functionalities, securely accessible by 
external and internal consumers. In technical terms, APIs provide an abstract layer for the internal 
business services to cater the consumer demand.

Gateways functions as a single point entry for expose backend services/microservices as proxy services / APIs 
aggregating the backend services into a unified services layer and simplify the backend service contracts by 
introducing security policies for authentication and authorization appropriately, only allowing authorized 
consumers to access services.

Above can be achieved by deploying a WSO2 Micro Integrator in a “DMZ” (demilitarized zone) and exposing the services 
to external service consumers. The DMZ EI pre-processes service requests coming from the public and routes only valid 
and authorized messages to the actual service platforms.
Pre-processing steps typically consist of message validation, filtering, and transformation, orchestration, etc.


## Route messages between systems

Message routing is one of the most fundamental requirements of systems/services integration. 
It considers addressability, static/deterministic routing, content-based routing, header-based routing, 
rules-based routing, and policy-based routing as ways of routing a message. 
WSO2 Micro Integrator enables these routing capabilities by means of its mediator and endpoint concepts.

![message routing](../assets/img/use-cases-overview/message-routing-new.png)

## Service orchestration

Service Orchestration is the process of accessing multiple fine-grained services from a single coarse-grained service. 
The service client would only have the visibility to a single coarse-grained service and would be encapsulated 
from the multiple fine-grained services that are invoked in the process flow.

![service chaining](../assets/img/use-cases-overview/service-chaining-new.png)

There are two distinct types of service orchestration

- Synchronous service orchestration
- Asynchronous service orchestration

Each above service orchestration approach interact services with two patterns based on the use case

### Service chaining
Multiple services need to be orchestrated is invoked one after the other in a synchronous manner. 
The input to the one service is dependant on the output of another service. 
Invocation of services and input-output mapping is taken care of by the service orchestration layer. 

### Parallel or Sequential invocation of multiple independent services
This allows the services to be invoked simultaneously without been blocked until a response is received from another service.

## Data Integration

Data integration is an important part of an integration process. For example, consider the following scenario, 
where you have a typical integration process that is managed using the Micro Integrator. 
In this scenario, data stored in various, disparate data sources is required in order to complete the 
integration use case. 
The data services functionality that is embedded in the Micro Integrator allows you to manage this 
integration scenario by decoupling the data from the data source layer and exposing them as data services. 
The main integration flow defined in the Integrator will then have the capability of managing the data through 
the data service. Once the data service is defined, you can manipulate the data stored in the data sources 
by invoking the relevant operation defined in the data service. 
For example, you can perform the basic CRUD operations as well as other advanced operations.


## File processing

In many business domains, there are different use cases related to managing files. 
Also, there are file-based legacy systems that are tightly coupled with other systems.
These files contain huge amounts of data and it requires a big effort for manual processing. 
Also, it is not scalable with an increase in system load. This leads us to the requirement of 
automating the processing of files.

File processing system should be capable of:

- Reading, Writing and Updating files:
Files can be located in the local file system or remote location which access enabled over protocols such as 
FTP, FTPS, SFTP, SMB. Therefore the system used to process those files should capable of communicating 
over those protocols.

- Process data
The system should capable of extracting relevant information from the file. For example, 
if required to process XML files, the system should be capable of executing and XPath on the file content 
and extract relevant information.

- Execute some business logic
The system should be capable perform actions that required to construct a business use case. 
It should capable of taking decisions and sending processed information to other systems over different 
communication protocols.

The WSO2 Micro Integrator has capabilities to achieve above.

## Asynchronous message processing

Asynchronous messaging is a communication method wherein the system puts a message in a message queue and 
does not require an immediate response to continue processing. Below are a few places to use asynchronous messaging.

- Delegate the request to some external system for processing
- Ensure delivery of a message to an external system
- Throttle message rates between two systems
- Batch processing of messages

There are a few aspects of asynchronous message processing

- Asynchronous messaging solves the problem of intermittent connectivity. 
The message receiving party does not need to be online as the message is stored in a middle layer and it can 
receive it when it comes online.

- Message consumers do not need to know about the message publishers and they can operate independently to each other.

Disadvantages of asynchronous messaging include the additional component of a message broker or transfer agent 
to ensure the message is received. This may affect both performance and reliability.

There are various levels of message delivery reliability grantees from publisher to broker and from broker to subscriber.
Wire level protocols like AMQP and MQTT can provide those.

## Periodically execute an integration process

Execution of certain tasks periodically can be performed by configuring a “Scheduled Task” . 
The user can schedule a task to run in the time interval of 't' for 'n' number of times or to run once 
Micro Integrator starts. Furthermore, the user can use cron expressions for more advanced executing time configuration.
