# Message entry points

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

WSO2 Micro Integrator can be configured to execute tasks periodically. You can schedule a task to run after a time interval of
't' for an 'n' number of times, or you can schedule the task to run once when the server starts. Alternatively, you can use cron expressions
to have more control over how the task should be scheduled; for example, you can use a Cron expression to schedule the task to run at 10 pm on the 20th day of every month.

According to the default task implementation, a task
can be configured to inject messages, either to a defined endpoint, to a
proxy service or a specific sequence. 

!!! Info
    In a clustered environment, tasks are distributed among server nodes according to the **round-robin** method by default. If required, you can change this default task handling behaviour so that tasks are distributed **randomly**, or according to a **specific rule**. This is a server-level setting that is configured in the `tasks-config.xml` file.

    -   See [Configuring the Task Scheduling Component](https://docs.wso2.com/display/ADMIN44x/Configuring+the+Task+Scheduling+Component)
    for instructions on configuring the task handling behaviour at server-level.
    -   You can also configure the task handling behaviour at task-level, by specifying the [Pinned Servers](#SchedulingESBTasks-pinned_servers) for a task. Note that this setting overrides the server-level configuration.

Also, note that a scheduled task will only run on one of the nodes (at a given time) in a clustered environment. The task will fail over to
another node only if the first node fails.