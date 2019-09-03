# Message entry points

## Proxy services

Proxy services are virtual services that receive messages and optionally
process them before forwarding them to a service at a given endpoint.
This approach allows you to perform necessary transformations and
introduce additional functionality without changing your existing
service. Any available transport can be used to receive and send
messages from the proxy services. A proxy service is externally visible
and can be accessed using a URL similar to a normal web service address.

## REST APIs

A REST API in WSO2 Micro Integrator is analogous to a web application
deployed in the integration server. Each API is anchored at a
user-defined URL context, much like how a web application deployed in a
servlet container is anchored at a fixed URL context. An API will only
process requests that fall under its URL context. 

A REST API contains one or more resources, which is a logical component of an API that can be
accessed by making a particular type of HTTP call.

A REST API resource is used by the WSO2 Micro Integrator mediation
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

## Inbound endpoints

An inbound endpoint is a message entry point that can inject messages directly from the transport layer to the mediation layer, without going through the Axis2 engine. One of the advantages of using Inbound Endpoints is in its ability to create inbound messaging channels dynamically. There are three types of inbound endpoints:

### Listening Inbound Endpoints

A listening inbound endpoint listens on a given port for requests that are coming in. When a request is available it is injected to a given sequence. Listening inbound endpoints support two-way operations and are synchronous.

<table>
    <tr>
        <th>Inbound Protocol</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>HTTP Inbound Protocol</td>
        <td>
            The HTTP inbound protocol is used to separate endpoint listeners for each HTTP inbound endpoint so that messages are handled separately. The HTTP inbound endpoint can bypass the inbound side axis2 layer and directly inject messages to a given sequence or API. For proxy services, messages will be routed through the axis2 transport layer in a manner similar to normal transports. You can start dynamic HTTP inbound endpoints without restarting the server.
        </td>
    </tr>
    <tr>
        <td>HTTPS Inbound Protocol</td>
        <td>
            Similar to the HTTP inbound protocol.
        </td>
    </tr>
    <tr>
        <td>HL7 Inbound Protocol</td>
        <td>
            The HL7 inbound protocol is an alternative to the HL7 transport. The HL7 inbound endpoint implementation is fully asynchronous and is based on the **Minimal Lower Layer Protocol(MLLP)** implemented on top of event driven I/O.
        </td>
    </tr>
    <tr>
        <td>CXF WS-RM Inbound Protocol</td>
        <td>
            WS­ReliableMessaging allows SOAP messages to be reliably delivered between distributed applications, regardless of software or hardware failures. The CXF WS­-RM inbound endpoint allows a client (RM Source) to communicate with the Micro Integrator with a guarantee that a message sent will be delivered.
        </td>
    </tr>
    <tr>
        <td>WebSocket Inbound Protocol</td>
        <td>
            The WebSocket Inbound protocol is based on the [WebSocket protocol](http://tools.ietf.org/html/rfc6455) and allows full-duplex message mediation.
        </td>
    </tr>
    <tr>
        <td>Secure WebSocket Inbound Protocol</td>
        <td>
           The secure WebSocket inbound protocol is based on the [WebSocket protocol](http://tools.ietf.org/html/rfc6455) and allows full-duplex, secure message mediation.
        </td>
    </tr>
</table>

### Polling Inbound Endpoints

A polling inbound endpoint polls periodically for data and, when data is available, the data is injected to a given sequence. For example, the JMS inbound endpoint checks the JMS queue periodically for messages and when a message is available that message is injected to a specified sequence. Polling inbound endpoints support one way operations and are
asynchronous.

<table>
    <tr>
        <th>Inbound Protocol</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>File Inbound Protocol</td>
        <td>
            The file inbound protocol is an alternative to the VFS transport. It uses the <b>VFS</b> transport to process files in a specified source directory. After processing the files, it moves them to a specified location or deletes them. Note that if files remain in the source directory after processing, they will be processed again. Therefore, if you need to maintain these files or keep track of the files that are processed, specify the option to move them instead of deleting them after processing.
        </td>
    </tr>
    <tr>
        <td>JMS Inbound Protocol</td>
        <td>
            The JMS inbound protocol is an alternative to the JMS transport. The JMS inbound protocol implementation requires an active JMS server instance to be able to receive messages, and you need to place the client JARs for your JMS server in the Micro Integrator.
        </td>
    </tr>
    <tr>
        <td>Kafka Inbound Protocol</td>
        <td>
            The Kafka inbound endpoint provides the functionalilties of the <a href="http://kafka.apache.org/documentation.html">Kafka</a> messaging system. Kafka maintains feeds of messages in topics. Producers write data to topics and consumers read from topics. The Kafka inbound endpoint serves as a message consumer by creating a connection to ZooKeeper and requesting messages for a topic, topics, or topic filters.
        </td>
    </tr>
</table>

### Event-Based Inbound Endpoints

An event-based inbound endpoint polls only once to establish a connection with the remote server and then consumes events.

<table>
    <tr>
        <th>Inbound Protocol</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>RabbitMQ Inbound Protocol</td>
        <td>
            <a href="http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol">AMQP</a> is a wire-level messaging protocol that describes the format of the data that is sent across the network. If a system or application can read and write AMQP, it can exchange messages with any other system or application that understands AMQP regardless of the implementation language.
        </td>
    </tr>
    <tr>
        <td>MQTT Inbound Protocol</td>
        <td>
            MQ Telemetry Transport (MQTT) is a lightweight broker-based publish/subscribe messaging protocol, designed to be open, simple, lightweight and easy to implement. These characteristics make it ideal for use in constrained environments:
            <ul>
                <li>Where the network is expensive, has low bandwidth or is unreliable.</li>
                <li>When running on an embedded device with limited processor or memory resources.</li>
            </ul>
        </td>
    </tr>
</table>

## Scheduled Tasks

WSO2 Micro Integrator can be configured to execute tasks periodically. You can schedule a task to run after a time interval of
't' for an 'n' number of times, or you can schedule the task to run once when the server starts. Alternatively, you can use cron expressions to have more control over how the task should be scheduled. For example, you can use a Cron expression to schedule the task to run at 10 pm on the 20th day of every month.

According to the default task implementation, a task can be configured to inject messages, either to a defined endpoint, to a
proxy service or a specific sequence. If required, you can use a custom task scheduling implementation.