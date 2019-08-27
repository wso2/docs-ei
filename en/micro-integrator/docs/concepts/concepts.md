# Key Concepts

Following are the key concepts with respect to the key
constructs/artifacts of WSO2 EI.

## Message entry points

### Proxy services

Proxy services are virtual services that receive messages and optionally
process them before forwarding them to a service at a given
[endpoint](https://docs.wso2.com/display/EI611/Working+with+Endpoints) .
This approach allows you to perform necessary transformations and
introduce additional functionality without changing your existing
service. Any available transport can be used to receive and send
messages from the proxy services. A proxy service is externally visible
and can be accessed using a URL similar to a normal web service address.

### REST APIs

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

### Inbound endpoints

An inbound endpoint is a message entry point that can inject messages
directly from the transport layer to the mediation layer, without going
through the Axis2 engine. The following diagram illustrates the inbound
endpoint architecture.

Out of the existing transports only the HTTP transport supports
multi-tenancy, this is one limitation that is overcome with the
introduction of the inbound architecture. Another limitation when it
comes to conventional Axis2 based transports is that the transports do
not support dynamic configurations. With the ESB profile inbound
endpoints, it is possible to create inbound messaging channels
dynamically, and there is also built-in cluster coordination as well as
multi-tenancy support for all transports.

For detailed information on each type of inbound endpoint available with
the ESB profile, see WSO2 EI Inbound Endpoints.

**Synapse configuration**: Following is a sample inbound endpoint configuration:

```
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                    name="HttpListenerEP"
                    sequence="TestIn"
                    onError="fault"
                    protocol="http"
                    suspend="false">
  <parameters>
    <parameter name="inbound.http.port">8085</parameter>
  </parameters>
</inboundEndpoint>
```

In an inbound endpoint configuration, the common inbound endpoint parameters are specified as attributes of the `<inboundEndpoint>` element whereas the protocol specific parameters are specified as `<parameter>` elements.

**Listening Inbound Endpoints**: A listening inbound endpoint listens on a given port for requests that
are coming in. When a request is available it is injected to a given
sequence. Listening inbound endpoints support two way operations and are synchronous.

**Polling Inbound Endpoints**: A polling inbound endpoint polls periodically for data and when data is
available the data is injected to a given sequence. For example,  the
JMS inbound endpoint checks the JMS queue periodically for messages and
when a message is available that message is injected to a specified
sequence. Polling inbound endpoints support one way operations and are
asynchronous.

**Event-Based Inbound Endpoints**: An event-based inbound endpoint polls only once to establish a
connection with the remote server and then consumes events.

#### File Inbound Protocol

The file inbound protocol is a multi-tenant capable alternative to the VFS transport. The file inbound protocol uses the [VFS transport](https://docs.wso2.com/display/EI650/VFS+Transport) to process files in a specified source directory. After processing the files, it moves them to a specified location or deletes them. Note that files cannot remain in the source directory after processing or they will be processed again, so if you need to maintain these files or keep track of the files that are processed, specify the option to move them instead of deleting them after processing.

#### Kafka Inbound Protocol

Kafka is a distributed, partitioned, replicated commit log service. It provides the functionality of a messaging system. Kafka maintains feeds of messages in topics. Producers write data to topics and consumers read from topics. For more information on Apache Kafka, go to [Apache Kafka documentation](http://kafka.apache.org/documentation.html) .
			The kafka inbound endpoint acts as a message consumer. It creates a connection to ZooKeeper and requests messages for a topic, topics or topic filters. In order to use the kafka inbound endpoint, you need to download and install [Apache Kafka](http://kafka.apache.org/downloads.html) . The recommended version is `         kafka_2.9.2-0.8.1.1        ` .
			To configure the kafka inbound endpoint, copy the following client libraries from the `<KAFKA_HOME>/libs        ` directory to the `<EI_HOME>/lib` directory.
			-   `          kafka_2.9.2-0.8.1.1.jar         `
			-   `           scala-library-2.9.2.jar          `
			-   `           zkclient-0.3.jar          `
			-   `           zookeeper-3.3.4.jar          `
			-   `           metrics-core-2.2.0.jar          `
			**Note**:    
			-   If you are using `          kafka_2.x.x-0.8.2.0         ` or later, you also need to add the `          kafka-clients-0.8.x.x.jar         ` file to the `          <EI_HOME>/lib         ` directory.
    		-   If you are using a newer version of ZooKeeper, follow the steps below:
        		1.  Create a directory named `             conf            ` inside the `             <EI_HOME>/repository            ` directory.
        		2.  Create a directory named identity inside the `             <EI_HOME>/repository/conf            ` directory.
        		3.  Add the [jaas.conf](attachments/119130492/119130493.conf) file to the `             <EI_HOME>/repository/conf/identity            ` directory. This is required because Kerberos authentication is enforced on newer versions of ZooKeeper.

#### JMS Inbound Protocol

The JMS inbound protocol is a multi-tenant capable alternative to the
JMS transport. The JMS inbound protocol implementation requires an
active JMS server instance to be able to receive messages, and you need
to place the client JARs for your JMS server in the ESB profile
classpath.

This section provides information on using the Broker Profile of WSO2 EI
or Apache ActiveMQ as the JMS server, but other implementations such as
Apache Qpid and Tibco are also supported.

Configuration parameters for a JMS inbound endpoint are XML fragments
that represent JMS connection factories.

#### HTTP Inbound Protocol

The HTTP inbound protocol is used to separate endpoint listeners for
each HTTP inbound endpoint so that messages are handle separately. The
HTTP inbound endpoint can bypass the inbound side axis2 layer and
directly inject messages to a given sequence or API. For proxy services,
messages will be routed through the axis2 transport layer in a manner
similar to normal transports. You can start dynamic HTTP inbound
endpoints without restarting the server.

Following is a sample HTTP inbound endpoint configuration:


!!! Note
    -   A sequence should be designed with a call/respond or send/receive sequence. It is not recommended to use a sequence that has in and out mediators.
    -   If a send mediator is used within the inbound endpoint sequence, specify a receiving sequence. If you do not specify a receiving sequence, the response will dispatch to the `           main          ` sequence.

#### HTTPS Inbound Protocol

The HTTPS inbound protocol is used to separate endpoint listeners for
each HTTPS inbound endpoint so that messages are handle separately. This
transport can bypass the inbound side axis2 layer and directly inject
messages to a given sequence or API. You can start dynamic HTTPS inbound
endpoints without restarting the server.

Configuration parameters for a HTTPS inbound endpoint are XML fragments
that represent various properties.


!!! Note
    -   A sequence should be designed with a call/respond or send/receive sequence. It is not recommended to use a sequence that has in and out mediators.
    -   If a send mediator is used within the inbound endpoint sequence, specify a receiving sequence . If you do not specify a receiving sequence, the response will dispatch to the `           main          ` sequence.

#### WebSocket Inbound Protocol

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) WebSocket
protocol implementation is based on the [WebSocket
protocol](http://tools.ietf.org/html/rfc6455) , and allows full-duplex
message mediation.

#### Secure WebSocket Inbound Protocol

The secure WebSocket inbound protocol implementation is based on the
[WebSocket protocol](http://tools.ietf.org/html/rfc6455) , and allows
full-duplex, secure message mediation.

#### HL7 Inbound Protocol

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) HL7 inbound
protocol is a multi-tenant capable alternative to the HL7 transport. The
HL7 inbound endpoint implementation is fully asynchronous and is based
on the **Minimal Lower Layer Protocol(MLLP)** implemented on top of
event driven I/O.

#### CXF WS-RM Inbound Protocol

WS­ReliableMessaging allows SOAP messages to be reliably delivered
between distributed applications, regardless of software or hardware
failures. The CXF WS­-RM inbound endpoint allows a client (RM Source) to
communicate with the ESB profile of WSO2 EI (RM Destination) with a
guarantee that a message sent will be delivered.

!!! Note
    To configure the CXF WS-RM Inbound endpoint, you need to install the **CXF WS Reliable Messaging** feature.

#### RabbitMQ Inbound Protocol

[AMQP](http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)
is a wire-level messaging protocol that describes the format of the data
that is sent across the network. If a system or application can read and
write AMQP, it can exchange messages with any other system or
application that understands AMQP regardless of the implementation
language.

Configuration parameters for a RabbitMQ inbound endpoint are XML
fragments that represent various properties.

#### MQTT Inbound Protocol

MQ Telemetry Transport (MQTT) is a lightweight broker-based
publish/subscribe messaging protocol, designed to be open, simple,
lightweight and easy to implement. These characteristics make it ideal
for use in constrained environments.

For example,

-   When the network is expensive, has low bandwidth or is unreliable.

-   When running on an embedded device with limited processor or memory
    resources.

To configure the MQTT inbound endpoint, you need to specify XML
fragments that represents various parameters.

### Tasks

A task allows you to run a piece of code triggered by a timer. WSO2
Enterprise Integrator provides a default task implementation, which you
can use to inject a message to the Enterprise Integrator at a scheduled
interval. You can also write your own custom tasks by implementing a
Java interface.

## Message processing units

### Mediators

Mediators are individual processing units that perform a specific
function, such as sending, transforming, or filtering messages. WSO2
Enterprise Integrator includes a comprehensive mediator library that
provides functionality for implementing widely used [Enterprise
Integration Patterns
(EIPs)](https://docs.wso2.com/display/EI650/Enterprise+Integration+Patterns)
. You can also easily write a custom mediator to provide additional
functionality using various technologies such as Java, scripting, and
Spring.

### Sequences

A sequence is a set of mediators organized into a logical flow, allowing
you to implement pipes and filter patterns. You can add sequences to
proxy services and REST APIs.

### Message stores and processors

A **message store** is used to temporarily store messages before they
are delivered to their destination by a **message
processor**. This approach is useful for serving
traffic to back-end services that can only accept messages at a given
rate, whereas incoming traffic to the Micro Integrator arrives at different
rates. You must have added a message store before you can add a message processor.

To store incoming traffic in a message store, use the Store mediator, and then use a message processor to deliver messages to the back-end service at a
given rate. Using message processors and message stores allows you to
implement different messaging and integration patterns.

Multiple message processors can use the same message store. For example,
in a clustered environment, each of the nodes would have an instance of
the same message processor, each of which would connect to the same
message store and evenly consume messages. The message store
acts as a manager of these consumers and their connections and ensures
that messages are processed by only one message processor, preventing
message duplication. You can further control which nodes a message
processor runs on by specifying pinned servers.

!!! Info
    You can increase performance of message processors either by **increasing the member count** or by having multiple message processors. If you increase the member count, it will create multiple child processors of the message processor.

**Message Stores**

<table>
	<tr>
		<th>Message Store Type</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>
			JDBC Message Store
		</td>
		<td>
			The JDBC message store can be used to store and retrieve messages more efficiently in comparison with other message stores. The JDBC message store implementation is a variation of the already existing synapse message store implementation and is designed in a manner similar to the message store of the ESB profile. The JDBC message store uses a JDBC connector to connect to external relational databases.</br></br>
			The advantages of using a JDBC message store instead of any other message store are as follows:
			<ul>
				<li>
					<b>Easy to connect</b>: You only need to have a JDBC connector to connect to an external relational database.
				</li>
				<li>
					<b>Quick transactions</b>: JDBC message stores are capable of handling a large number of transactions per second.
				</li>
				<li>
					<b>Ability to work with a high capacity for a long period of time</b>: Since JDBC stores use databases as the medium to store data, it can store a large volume of data and is capable of handling data for a longer period of time.
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			JMS Message Store
		</td>
		<td>
			The JMS message store persists messages in a JMS queue inside a JMS Broker. The JMS message store can be configured by specifying the class as  <code>org.apache.synapse.message.store.impl.jms.JmsStore</code>. Since the JMS message stores persist messages in a JMS queue in an ordered manner, JMS message stores can be used to implement the store-and-forward pattern.
		</td>
	</tr>
	<tr>
		<td>
			RabbitMQ Message Store
		</td>
		<td>
			RabbitMQ message store persists messages in a RabbitMQ queue inside a RabbitMQ broker. The RabbitMQ message store can be configured by specifying the class as <code>org.apache.synapse.message.store.impl.rabbitmq.RabbitmqStore</code> and then setting all other required parameters to connect to a RabbitMQ broker.
		</td>
	</tr>
	<tr>
		<td>
			Resequence Message Store
		</td>
		<td>
			The Resequence Message Store is used to store a stream of related but out-of-sequence messages to put them back into the correct order. It collects and re-orders the stored messages based on a defined sequence number derived from some part of the message, so that they can be published to the output channel in a specific order. This is advantageous specially when the order of message delivery is important to avoid some messages arriving earlier than others.</br>
			The resequencing store is an extension of the existing JDBC-based message store. Hence, it inherits most of its properties from the **JDBC Message Store**.
		</td>
	</tr>
	<tr>
		<td>
			WSO2 MB Message Store
		</td>
		<td>
			WSO2 Enterprise Integrator (WSO2 EI) is shipped with a separate message broker profile (WSO2 MB). You can easily set up this WSO2 MB profile as the **message store** for the ESB. Explained below are the parameters you need to configure for a WSO2 MB Message Store. Also, find details of other configurations that are relevant to the WSO2 MB Message Store.  
		</td>
	</tr>
	<tr>
		<td>
			In-Memory Message Store
		</td>
		<td>
			In memory message store is a basic **Message Store** that stores messages in an in-memory queue. Since the messages are stored in an in-memory queue in case of a ESB profile restart, all the messages stored will be lost.</br>
			The in memory message store is lot faster than a persistent message store implementation, so it can be used to temporarily store messages for use cases such as the implementation of a high-speed store and forwarded pattern where message persistence is not a requirement.</br></br>
			<b>Note</b>: In memory message stores are not recommended for use in production as well as in scenarios where large scale message storing is required. You can use an external message store (e.g., **JMS message store**) for such scenarios.
		</td>
	</tr>
	<tr>
		<td>Custom Message Store</td>
		<td>
			**Custom Message Store** allows users to create a message store with their own message store implementation. It can be configured using configuration by giving the fully qualified class name of the message store implementation as the class value.
			Messages will be stored as specified in the underlying message store implementation. Parameter configuration can be used to pass any configuration parameters that is needed by the message store implementation class.
		</td>
	</tr>
</table>

**Message Processors**

<table>
	<tr>
		<th>Message Processor</th>
		<th>
			Description
		</th>
	</tr>
	<tr>
		<td>
			Message Sampling Processor
		</td>
		<td>
			The message sampling processor consumes messages in a **message store** and sends them to a configured **sequence**. This process happens in a preconfigured interval. This message processor does not ensure reliable messaging.
		</td>
	</tr>
	<tr>
		<td>Scheduled Failover Message Forwarding Processor</td>
		<td>
			The scheduled failover message forwarding processor is a **message processor** that ensures reliable message delivery. This message processor is useful when it comes to scenarios where a message store failure takes place and it is necessary to ensure guaranteed message delivery.</br></br>
			The only difference of the scheduled failover message forwarding processor from the scheduled message forwarding processor is that the scheduled message forwarding processor forwards messages to a defined endpoint, whereas the scheduled failover message forwarding processor forwards messages to a target message store.
		</td>
	</tr>
	<tr>
		<td>Scheduled Message Forwarding Processor'</td>
		<td>
			The scheduled message forwarding processor is a **message processor** that consumes messages in a message store and sends them to an **endpoint**. If a message is successfully delivered to the endpoint, the processor deletes the message from the message store. In case of a failure, it will retry after a specified interval.
		</td>
	</tr>
	<tr>
		<td>Custom Message Processor</td>
		<td>
			Existing message processor implementations are created using the [Quartz](http://quartz-scheduler.org/) enterprise job scheduler. If needed, you can create your own implementations of message processors by creating a Java class that implements the `MessageProcessor` interface. You then select the **Add Custom Message Processor** option when adding a message processor and specify your implementation class.
			Note that message processors go through several life-cycle stages, so you must take great care when creating your own implementation. Because existing implementations are tested and proven under high loads, the best practice is to use the existing implementations whenever possible.
		</td>
	</tr>
</table>

### Templates

The synapse configuration language is a very powerful and robust way of driving enterprise data/messages through the Micro Integrator mediation engine. However, a large number of configuration files in the form of **sequences**, **endpoints**, **proxy services**, and transformations can be required to satisfy all the mediation requirements of your system. To keep your configurations manageable, it's important to avoid scattering configuration files across different locations and to avoid duplicating redundant configurations.

**Templates** help minimize this redundancy by creating prototypes that users can use and reuse when needed. This is very much analogous to classes and instances of classes: a template is a class that can be used to wield instance objects such as templates and endpoints. Thus, templates are an ideal way to improve reusability and readability of synapse configurations. Additionally, users can use predefined templates that reflect common **enterprise integration patterns** for rapid development of message/mediation flows in the Micro Integrator.

**Endpoint Template**: Defines a templated form of an endpoint. An endpoint template can parameterize an endpoint defined inline. An endpoint template would be useless without a template endpoint referring to it.

**Sequence Template**: A **Sequence Template** is a parametrized **sequence** providing an abstract or generic form of a sequence defined in the Micro Integrator. Parameters of a template are defined in the form of XPath statement/s. Callers can invoke the template by populating the parameters with static values/XPath expressions using the **Call Template** Mediator, which makes a sequence template into a concrete sequence.

!!! Info
    The **Call Template** mediator allows you to construct a sequence by passing values into a **sequence template**. This is currently only supported for special types of mediators such as the **Iterator** and **Aggregate Mediators**, where actual XPath operations are performed on a different SOAP message, and not on the message coming into the mediator.

Sequence template parameters can be referenced using an XPath expression defined inside the in-line sequence. For example, the parameter named "foo" can be referenced by the Property mediator (defined inside the in-line sequence of the template) in the following ways:

`<property name=”fooValue” expression=”$func:foo” />`

or

`<property name=”fooValue” expression=”get-property('foo','func')” />`

Using function scope or "?func?" in the XPath expression allows us to refer to a particular parameter value passed externally by an invoker
such as the Call Template mediator.

### Connectors

Connectos allow your message flows to connect to and interact with services such as Twitter and Salesforce. A connector is a collection of **templates** that define specific operations. Typically, connectors are used to wrap the API of an external service such as Twitter or a Google Spreadsheet. Each connector provides operations that perform different actions in that service. For example, the Twitter connector has operations for creating a tweet, getting a user's followers, and more.

To download a required connector, go to the [WSO2 Connector Store](https://store.wso2.com/store).

## Message exit points

A message exit point or an endpoint defines an external destination for a message. Typically, this is the address of a proxy service, which acts as the front end to the actual service. An endpoint can connect to any external service after
configuring it with any attributes or semantics needed for communicating with that service. For example, an endpoint could represent a URL,a mailbox, a JMS queue, or a TCP socket, along with the settings needed to connect to it.

An endpoint is defined independently of transports, allowing you to use the same endpoint with multiple transports. When you configure a message mediation sequence or a proxy service to handle the incoming message, you specify which transport to use and the endpoint where the message will be sent.

You can specify an endpoint as an [address
endpoint](https://docs.wso2.com/display/EI650/Address+Endpoint) , [WSDL
endpoint](https://docs.wso2.com/display/EI650/WSDL+Endpoint) , a load
balancing endpoint and more. 

### Endpoints

 For example, the endpoint for the simple stock quote sample is `http://localhost:9000/services/SimpleStockQuoteService`.

**Named endpoints**: You can use the `name` attribute to create a named endpoint. You can reuse a named endpoint by referencing it in another endpoint using the `key` attribute. For example, if there is an endpoint named *foo* , you can reference the *foo* endpoint in any other endpoint where you want to use *foo*: `<endpoint key="foo"/>`. This approach allows you to reuse existing endpoints in multiple places.

Indirect and Resolving endpoints are endpoint configurations with a key
which refers to an existing endpoint.

**Indirect Endpoints**: The **Indirect Endpoint** refers to an actual
[endpoint](_Working_with_Endpoints_) by a key. This endpoint fetches the
actual endpoint at runtime. Then it delegates the message sending to the
actual endpoint. When endpoints are stored in the
[registry](https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry)
and referred, this endpoint can be used. The `         key        ` is a
static value for this endpoint.

**Resolving Endpoint**: The **Resolving Endpoint** refers to an actual endpoint using a dynamic key. The key is an XPath expression.

1. The XPath is evaluated against the current message and key is
calculated at run time.  
2. Resolving endpoint fetches the actual endpoint using the calculated
key.  
3. Resolving endpoint delegates the message sending it to the actual
endpoint.

When endpoints are stored in the registry and referred, this endpoint
can be used.

!!! Info
	The XPath expression specified in a Resolving endpoint configuration derives an existing [endpoint](_Working_with_Endpoints_) rather than the URL of the endpoint to which the message is sent. To derive the endpoint URL to which the message is sent via an XPath expression, use the [Header Mediator](https://docs.wso2.com/display/EI650/Header+Mediator#HeaderMediator-ToHeader).

**Load-balanced Group**: The **Load-balanced Group** distributes the messages (load) arriving at
it among a set of listed [endpoints](_Working_with_Endpoints_) or static
members by evaluating the load balancing policy and other relevant
parameters.

<table>
	<tr>
		<th>Endpoint</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Address Endpoint</td>
		<td>
			Defined by specifying the EPR (Endpoint Reference) and other attributes of the configuration.
		</td>
	</tr>
	<tr>
		<td>Default Endpoint</td>
		<td>
			Defined for adding QoS and other configurations to the endpoint that is resolved by the <b>To</b> address of the message context. All the configurations such as the message format for the endpoint, the method to optimize attachments, and security policies for the endpoint can be specified as in the <b>Address Endpoint</b>. This endpoint differs from the address endpoint because the <b>URI</b> property is not present in the default endpoint.
		</td>
	</tr>
	<tr>
		<td>HTTP Endpoint</td>
		<td>
			Allows you to define REST endpoints using <b>URI templates</b> similar to the REST API. The URI templates allow a RESTful URI to contain variables that can be populated during mediation runtime using property values whose names have the <code>uri.var.</code> prefix. An HTTP endpoint can also define the particular HTTP method to use in the RESTful invocation. You can create HTTP endpoints by specifying values for the parameters given below. Alternatively, you can specify one parameter as the HTTP endpoint by using multiple other parameters, and then pass that to define the HTTP endpoint.
		</td>
	</tr>
	<tr>
		<td>Failover Endpoint</td>
		<td>
			With leaf endpoints, if an error occurs during message transmission, the message will be lost. The failed message will not be retried again. These errors occur very rarely, but still message failures can occur. With some applications, these message losses are acceptable, but if even rare message failures are not acceptable, use the **Failover** endpoint.</br>
			A **Failover Group** is a list of leaf endpoints grouped together for the purpose of passing an incoming message from one endpoint to another if a failover occurs. The first endpoint in failover group is considered the primary endpoint. An incoming message is first directed to the primary endpoint, and all other endpoints in the group serve as back-ups.</br>
			If the primary endpoint fails, the next active endpoint is selected as the primary endpoint, and the failed endpoint is marked as inactive. Thus, failover group ensures that a message is delivered as long as there is at least one active endpoint among the listed endpoints. The Micro Integrator switches back to the primary endpoint as soon as it becomes available. This behaviour is known as dynamic failover.
			<b>Note</b>: An endpoint failure occurs when an endpoint is unable to invoke a service. An endpoint, which responds with an error is not considered a failed endpoint.
		</td>
	</tr>
	<tr>
		<td>Dynamic Load-Balance Endpoint</td>
		<td>
			This endpoint distributes its messages (load) among application members by evaluating the load-balancing policy and any other relevant parameters. These application members will be discovered using the <b>membershipHandler</b> class, which generally uses a group communication mechanism to discover the application members. The <b>class</b> attribute of the <b>membershipHandler</b> element should be an implementation of <b>org.apache.synapse.core.LoadBalanceMembershipHandler</b>. You can specify <b>membershipHandler</b> properties using the <b>property</b> elements. The <b>policy </b> attribute of the <b>dynamicLoadbalance</b> element specifies the load-balancing policy (algorithm) to be used for selecting the next member that will receive the message.</br></br>
			<b>Note</b>: Currently only the <b>roundRobin</b> policy is supported. The <b>failover</b> attribute determines if the next member should be selected once the currently selected member has failed and defaults to true.
		</td>
	</tr>
	<tr>
		<td>Template Endpoint</td>
		<td>
			Template endpoints are created based on predefined <b>Endpoint Templates</b>. Parameters of the endpoint template used as the target are copied to the endpoint configuration. However, you can make modifications by removing some of the copied parameters and/or adding new parameters.
		</td>
	</tr>
	<tr>
		<td>WSDL Endpoint</td>
		<td>
			This definition is based on a specified WSDL document. The WSDL document can be specified in 2 ways:
			<ul>
				<li>As a URI.</li>
				<li>As an inlined definition within the endpoint configuration.</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>Recepient List Endpoint</td>
		<td>
			A Recipient List endpoint can contain multiple child endpoints or member elements. It routes cloned copies of messages to each child recipient. This will assume that all immediate child endpoints are identical in state (state is replicated) or state is not maintained at those endpoints.
		</td>
	</tr>
</table>

#### Endpoint states

At any given time, the state of the endpoint can be one of the following:

<table>
	<tr>
		<th>State</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Active</td>
		<td>
			Endpoint is running and handling requests.</br></br>
			When the Micro Integrator starts, endpoints are in the "Active" state and ready to handle messages. If the user does not put the endpoint into the OFF state, it will be in the "Active" state until an error occurs.</br></br>
			The endpoint can be configured to stay in the "Active" state or to go to "Timeout" or "Suspended" based on the error codes you configure for those states. When an error occurs, the endpoint checks to see whether it is a "Timeout" error first, and if not, it checks to see whether it is a "Suspended" error. If the error is not defined for either "Timeout" or "Suspended," the error will be ignored and the endpoint will stay Active.
		</td>
	</tr>
	<tr>
		<td>Timeout</td>
		<td>
			Endpoint encountered an error but can still send and receive messages. If it continues to encounter errors, it will be suspended.</br></br>
			When an endpoint is in the "Timeout" state, it will continue to attempt to receive messages until one message succeeds or the maximum retry setting has been reached. If the maximum is reached at which point the endpoint is marked as "Suspended." If one message succeeds, the endpoint is marked as "Active".</br></br>
			For example, let's assume the number of retries is set to 3. When an error occurs and the endpoint is set to the "Timeout" state, the Micro Integrator can try to send up to three more messages to the endpoint. If the next three messages sent to this endpoint result in an error, the endpoint is put in the "Suspended" state. If one of the messages succeeds before the retry maximum is met, the endpoint will be marked as "Active."
		</td>
	</tr>
	<tr>
		<td>Suspended</td>
		<td>
			Endpoint encountered errors and cannot send or receive messages. Incoming messages to a suspended endpoint result in a fault.</br></br>
			A "Suspended" endpoint cannot send or receive messages. When an endpoint is put into this state, the Enterprise Integrator waits until after an initial duration has elapsed (default is 30 seconds) before attempting to send messages to this endpoint again. If the message succeeds, the endpoint is marked as "Active." If the next message fails, the endpoint is marked as "Suspended" or "Timeout" depending on the error, and the Enterprise Integrator waits before retrying messages using the following formula: <code>Min(current suspension duration * progressionFactor, maximumDuration)</code>.</br></br>
			You configure the initial suspension duration, progression factor, and maximum duration as part of the <b>suspendOnFailure</b> settings. On each retry, the suspension duration increases, up to the maximum duration.
		</td>
	</tr>
	<tr>
		<td>OFF</td>
		<td>
			Endpoint is not active. To put an endpoint into the OFF state, or to move it from OFF to Active, you must use JMX.
		</td>
	</tr>
</table>

## Transports

A transport is responsible for carrying messages that are in a specific
format. The Enterprise Integrator supports all the widely used
transports including HTTP/s, JMS, VFS and domain-specific transports
like FIX. You can easily add a new transport using the Axis2 transport
framework and plug it into the Enterprise Integrator. Each transport
provides a receiver, which the Enterprise Integrator uses to receive
messages, and a sender, which it uses to send messages. The transport
receivers and senders are independent of the Enterprise Integrator core.

## Message builders and formatters

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
framework.

## Applying security to artifacts

You can apply security to artifacts of the ESB profile using WSO2
Integration Studio. The Quality of Service (QoS) component implements
security.


## Logging messages

You can use the Log mediator to log mediated messages. 

## Message tracing

Message tracing helps you to track issues after an integration process
finishes and thereby, allows you to identify and fix issues by
identifying the root cause. It is used to trace, track and visualize a
body of a message in each intermediate stage of its transmission. T his
is useful for a number of reasons, including auditing and debugging.

## Debugging mediation

Message mediation mode is one of the operational modes of WSO2 EI where
EI functions as an intermediate message router. A unit of the mediation
flow is a mediator. A sequence is a series of mediators, where each
mediator is a unit entity that can input a message, carry out a
predefined processing task on the message, and output the message for
further processing. Debugging is where you want to know if these units,
which function as separate entities are operating as intended, or if a
combination of these units are operating as a whole as intended.

## Load balancing

The load balancer automatically distributes incoming traffic across
multiple WSO2 product instances. It enables you to achieve greater
levels of fault tolerance in your cluster and provides the required
balancing of load needed to distribute traffic.


## Enterprise Integration Patterns

Enterprise Application Integration (EAI) enables you to connect business
applications with heterogeneous systems. The [EIP patterns
guide](https://docs.wso2.com/display/EIP/Enterprise+Integration+Patterns+with+WSO2+Enterprise+Integrator)
demonstrates how the patterns that are invented integration solution
architects over the years can be simulated using various constructs in
the ESB profile.


## WSO2 Integration Studio

You can use WSO2 Integration Studio to create various integration
artifacts that you can build and deploy to the ESB profile of WSO2 EI in
order to process requests.

## Data service

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

### Datasources

Your organization's data can be stored in various data storage systems,
which are thedatasources. Data services in WSO2 EI support the
followingdatasources: Relational databases, CSV files, Microsoft Excel
Sheets, Google Spreadsheets, RDF, MongoDB, Cassandra, and Web Resources.
Additionally, you can also useJNDIdatasources, and create
customdatasources. Read about using variousdatasourceswith data services
defined in WSO2 EI.

### RESTful data services

A data service exposes your data (stored in various data stores) as a
service. You can enable RESTful access to your data, by defining RESTful
resources, for the relevant data, in your data service. REST resources
in WSO2 EI support both JSON and XML media types out of the box.
Therefore, a resource can receive requests, and send responses in either
medium. Secure resources with HTTP(S) Basic Auth integrated to
enterprise identity systems (via [WSO2 Identity
Server](http://wso2.com/products/identity-server/) ).

### OData services

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

### Data Federation

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

### Distributed transactions

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

### Batch processing

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

### Data transformation

XSLT transformation is used in data services to transform the result of
an already defined operation into a different result. The user can
define the transformation xslt and provide the url of the transformation
file in the result element.

### Managed data access

Most businesses require secure and managed data access across these
federated data stores .

### Streaming

Data service streaming helps manage large data chunks sent back to the
client by the data service as the response to a request. When streaming
is enabled, the data is sent to the client as it is generated, without
memory building up in the server. By default, streaming is enabled in
data services.

### Namespaces

The service namespace uniquely identifies a Web service and is specified
by the `         <targetNamespace>        ` element in the WSDL that
represents the service. A data service is simply a Web service with
specialized functionality. When developing a data service, you get to
apply namespaces at various levels. As a data service implementation is
based on XML, namespace handling is useful for making sure that there
are no conflicting element names in the XML. Although namespaces are
optional for data services, in some scenarios they are necessary. For
more information, see Defining Namespaces .

### Error Handling

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
