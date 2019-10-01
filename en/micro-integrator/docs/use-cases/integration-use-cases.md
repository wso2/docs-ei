# Integration Use Cases

WSO2 Micro Integrator helps you build and execute the following use cases.

## Message routing

Message routing is one of the most fundamental requirements when integrating systems/services. It considers addressability, static/deterministic routing, content-based routing, header-based routing, rules-based routing, and policy-based routing as ways of routing a message. WSO2 Micro Integrator enables these routing capabilities using the concepts of mediators and endpoints.
<!--
![message routing](../assets/img/use-cases-overview/message-routing-new.png)
-->

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/routing-requests-based-on-message-content">message routing</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
			<ul>
				<li>
					<a href="../../use-cases/examples/message-routing/header-based-routing">Routing based on message header</a>
				</li>
				<li>
					<a href="../../use-cases/examples/message-routing/payload-based-routing">Routing based on message payload</a>
				</li>
				<li>
					<a href="../../use-cases/examples/message-routing/split-and-aggregate-responses">Splitting and aggregating response messages</a>
				</li>
			</ul>
		</td>
	</tr>
</table>

## Message transformation

The integration of systems that communicate in various message formats is a common business case in enterprise integration. WSO2 Micro Integrator facilitates this use case as the intermediary system bridging the communication gap among the systems.
<!--
![message transformation](../assets/img/use-cases-overview/message-transformation-new.png) 
-->
<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/transforming-message-content">message transformation</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
			<ul>
				<li>
					<a href="../../use-cases/examples/message-transformations/soap-to-json-conversion">Converting SOAP Messages to JSON</a>
				</li>
				<li>
					<a href="../../use-cases/examples/message-transformations/pox-to-json-conversion">Converting POX Messages to JSON</a>
				</li>
				<li>
					<a href="../../use-cases/examples/message-transformations/json-to-soap-conversion">Converting JSON Messages to SOAP</a>
				</li>
				<li>
					<a href="../../use-cases/examples/message-transformations/csv-to-other-formats-conversion">Converting CSV Messages to Other Formats</a>
				</li>
				<li>
					<a href="../../use-cases/examples/message-transformations/csv-conversion">Converting to CSV Message Formats</a>
				</li>
			</ul>
		</td>
	</tr>
</table>

For example, consider a service that returns data in XML format and a mobile client that accepts messages only in JSON format. To allow these two systems to communicate, the intermediary system needs to convert message formats during the communication. This allows the systems to communicate with each other without depending on the message formats supported by each system.

## Service orchestration

Service Orchestration is the process of exposing multiple fine-grained services using a single coarse-grained service. The service client will only have access to a single coarse-grained service, which encapsulates the multiple fine-grained services that are invoked in the process flow.
<!--
![service chaining](../assets/img/use-cases-overview/service-chaining-new.png)
-->

There are two distinct types of service orchestration:

- Synchronous service orchestration
- Asynchronous service orchestration

In both the above orchestration approaches, the WSO2 Micro Integrator can interact with services using two different patterns (depending on the use case):

**Service chaining**

Multiple services that need to be orchestrated are invoked one after the other in a synchronous manner. The input to one service is dependant on the output of another service. Invocation of services and input-output mapping is handled by the service orchestration layer (which is the WSO2 Micro Integrator). 

**Parallel or Sequential service invocations**

Multiple services are invoked simultaneously without any blocking until a response is received from another service.

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/exposing-several-services-as-a-single-service">service orchestration</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
		</td>
	</tr>
</table>

## Asynchronous message processing

Asynchronous messaging is a communication method wherein the system puts a message in a message queue and does not require an immediate response to continue processing. Asynchronous messaging is useful for the following:

- Delegate the request to some external system for processing
- Ensure delivery of a message to an external system
- Throttle message rates between two systems
- Batch processing of messages

Note the following about asynchronous message processing:

- Asynchronous messaging solves the problem of intermittent connectivity. The message receiving party does not need to be online to receive the message as the message is stored in a middle layer. This allows the receiver to retrieve the message when it comes online.
- Message consumers do not need to know about the message publishers. They can operate independently.

Disadvantages of asynchronous messaging includes the additional component of a message broker or transfer agent to ensure the message is received. This may affect both performance and reliability. There are various levels of message delivery reliability grantees from publisher to broker and from broker to subscriber. Wire level protocols like AMQP and MQTT can provide those.

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/storing-and-forwarding-messages">asynchronous messaging</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
			<ul>
				<li>
					<a href="../../use-cases/examples/jms_examples/consuming-jms">Consuming JMS Messages</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/producing-jms">Producing JMS Messages</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/consume-produce-jms">Consumining and Producing JMS Messages</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/dual-channel-http-to-jms">Dual Channel HTTP to JMS</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/quad-channel-jms-to-jms">Quad Channel JMS to JMS</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/guaranteed-delivery-with-failover">Guaranteed Delivery with Failover</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/publish-subscribe-with-jms">Publish and Subscribe with JMS</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/shared-topic-subscription">Shared Topic Subscriptions</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/detecting-repeatedly-redelivered-messages">Detecting Repeatedly Redilivered Messages</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/jms-map-message">JMS Map Messaging</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/specifying-a-delivery-delay-on-messages">Delivery Delay on Messages</a>
				</li>
				<li>
					<a href="../../use-cases/examples/jms_examples/rabbitmq-examples">RabbitMQ Examples</a>
				</li>
			</ul>
		</td>
	</tr>
</table>

## Connecting Web APIs/Cloud services

One of the most expected features from an integration solution is 'Hybrid-Cloud Integration', i.e., the ability to connect with third-party applications via public APIs that are exposed by various application developers. These external applications could either be in the cloud (SaaS) or on-premise. WSO2 Micro Integrator enables this capability by means of its wide range of connectors.

<!--
A connector is a collection of templates that define operations that can be called from a mediation flow. This allows you to define a mediation flow that can easily access a specific message processing logic contained in a connector. Generally, a connector wraps the API of an external service and provides a more convenient interface for the integration developer to work with.
-->

WSO2 Micro Integrator supports more than 150 connectors, where Salesforce, Gmail, Amazon S3, are among the most popular. Connectors can be downloaded from the WSO2 connector store. It is also possible to write your own custom connector to integrate WSO2 Micro Integrator with a new system.

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/storing-and-forwarding-messages">connecting web Apis and cloud services</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
		</td>
	</tr>
</table>

## Data integration

Data integration is an important part of an integration process. For example, consider a typical integration process that is managed using the Micro Integrator: Data stored in various, disparate datasources are required in order to complete the integration use case. 

The data services functionality that is embedded in the Micro Integrator can decouple the data from the datasource layer and exposing them as data services. The main integration flow defined in the Integrator will then have the capability of managing the data through the data service. Once the data service is defined, you can manipulate the data stored in the datasources by invoking the relevant operation defined in the data service. For example, you can perform the basic CRUD operations as well as other advanced operations.

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/data-integration-tutorial">data integration</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
		</td>
	</tr>
</table>

## Protocol switching

The Micro Integrator offers a wide range of integration capabilities from simple message routing to complicated systems that use integrated solutions. Different applications typically use different protocols for communication. Therefore, for two systems to successfully communicate, it is necessary to switch the protocol (that passes from one system) to the protocol compatible with the receiving application.
<!--
![protocol switching](../assets/img/use-cases-overview/protocol-switching-new.png)
-->

For example, messages that are received via HTTP may need to be sent to a JMS queue. Further, you can couple the protocol switching feature with the message transformation feature to handle use cases where the content of messages received via one protocol (such as HTTP) are first processed, and then sent out in a completely different message format and protocol.

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/protocol-switching-tutorial">protocol switching</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
			<ul>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Introduction_to_Switching_Transports_">Introduction to Transport Switching</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switch_from_FTP_Listener_to_Mail__Sender">Switching from FTP Listener to Mail Sender</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switching_from_HTTP_to_FIX_">Switching from HTTP to FIX</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switch_from_FIX_to_HTTP_">Switching from FIX to HTTP</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switch_from_FIX_to_AMQP_">Switching from FIX to AMQP</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switching_between_FIX_Versions_">Switching between FIX Versions</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switching_from_TCP_to_HTTP_S_">Switching from TCP to HTTP/S</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switching_from_UDP_to_HTTP_S_">Switching from UDP to HTTP/S</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/HTTP_to_MSMQ_and_MSMQ_to_HTTP_">Switching between HTTP to MSMQ</a>
				</li>
				<li>
					<a href="../../use-cases/examples/protocol-switching/Switching_from_HTTP_S_to_JMS_">Switching from HTTP/S to JMS</a>
				</li>
			</ul>
		</td>
	</tr>
</table>

## Gateway

The Gateway pattern is used for securely exposing APIs (representing business functionalities) to external and internal consumers. In technical terms, APIs provide an abstract layer for the internal business services, which allows you to meet consumer demand. APIs and proxy services in WSO2 Micro Integrator aggregates the back-end services/micro services into a unified services layer and the security policies for authentication and autharization are applied at the services layer. This ensures that only authorized consumers have access to the services and that the back-end is simplified.

You can implement the Gateway pattern by deploying WSO2 Micro Integrator in a “DMZ” (demilitarized zone) and thereby exposing the services to external service consumers. The DMZ pre-processes service requests coming from the public and routes only valid and authorized messages to the actual service platforms. Pre-processing typically consist of message validation, filtering, and transformation, orchestration, etc.

<table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/integration/service-gateway-tutorial">using the Micro Integrator as a service gateway</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
			<ul>
				<li>
					<a href="../../use-cases/examples/gateway/expose-http-app-as-soap">Expose HTTP-based Applications as a SOAP Service</a>
				</li>
				<li>
					<a href="../../use-cases/examples/gateway/expose-http-app-as-rest-api">Expose HTTP-based Applications as a REST Api</a>
				</li>
				<li>
					<a href="../../use-cases/examples/gateway/expose-file-based-system-over-http">Expose File-based Systems over HTTP</a>
				</li>
				<li>
					<a href="../../use-cases/examples/gateway/expose-proprietary-prot-over-stand-protocols">Expose Services on Proprietary Protocols over Standard Protocols</a>
				</li>
				<li>
					<a href="../../use-cases/examples/gateway/securing-backend-with-request-throttling">Securing Backend with Request Throttling</a>
				</li>
			</ul>
		</td>
	</tr>
</table>

## File processing

In many business domains, there are different use cases related to managing files. Also, there are file-based legacy systems that are tightly coupled with other systems. These files contain huge amounts of data, which requires a big effort for manual processing. It is not scalable with an increase in system load. This leads us to the requirement of automating the processing of files. The WSO2 Micro Integrator enables the following file processing capabilities:

- Reading, Writing, and Updating files:

  	Files can be located in the local file system or a remote location which can be accessed over protocols such as FTP, FTPS, SFTP, SMB. Therefore, the system used to process those files should capable of communicating over those protocols.

- Process data

  	The system should capable of extracting relevant information from the file. For example, if required to process XML files, the system should be capable of executing and XPath on the file content and extract relevant information.

- Execute some business logic

  	The system should be capable of performing actions that are required to construct a business use case. It should be capable of taking decisions and sending processed information to other systems over different communication protocols.

 <table>
	<tr>
		<td>
			<b>Tutorials</b></br>
			<ul>
				<li>
					Try the end-to-end use case on <a href="../../../use-cases/tutorials/file-processing">file processing</a>
				</li>
			</ul>
		</td>
		<td>
			<b>Examples</b>
			<ul>
				<li>
					<a href="../../use-cases/examples/file-processing/vfs-transport-examples">VFS Transport
				</li>
				<li>
					<a href="../../use-cases/examples/file-processing/Accessing_Windows_Share_Using_VFS_Transport">Accessing a Windows Share using VFS</a>
				</li>
				<li>
					<a href="../../use-cases/examples/file-processing/mailto-transport-examples">Using the MailTo Transport</a>
				</li>
			</ul>
		</td>
	</tr>
</table>

**Failure tracking**

To track failures in file processing, which can occur when a resource becomes unavailable, the VFS transport creates and maintains a failed records file. This text file contains a list of files that failed to be processed. When a failure occurs, an entry with the failed file name and the timestamp is logged in the text file. When the next polling iteration occurs, the VFS transport checks each file against the failed records file, and if a file is listed as a failed record, it will skip processing and schedule a move task to move that file.

## Periodic execution of integration processes

Task execution can be automated to run periodically. You can schedule a task to run in the time interval of 't' for 'n' number of times or to run once the
Micro Integrator starts. Furthermore, you can use cron expressions for more advanced executing time configuration.