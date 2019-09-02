# Transport Protocols

A transport is responsible for carrying messages that are in a specific
format. The Enterprise Integrator supports all the widely used
transports including HTTP/s, JMS, VFS and domain-specific transports
like FIX. You can easily add a new transport using the Axis2 transport
framework and plug it into the Enterprise Integrator. Each transport
provides a receiver, which the Enterprise Integrator uses to receive
messages, and a sender, which it uses to send messages. The transport
receivers and senders are independent of the Enterprise Integrator core.

<table>
	<tr>
		<th>Transport</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>PassThrough</td>
		<td>
			This is a non-blocking HTTP transport implementation based on HTTP Core NIO, and is the default HTTP transport shipped with WSO2 Micro Integrator. Although the PassThrough transport is somewhat similar to the NHTTP transport, it overcomes all the limitations of the NHTTP transport and provides a significant performance gain. The PassThrough Transport also has a simpler and cleaner model for forwarding messages back and forth.
		</td>
	</tr>
	<tr>
		<td>HTTP-NIO</td>
		<td>
			This is a module of the Apache Synapse project. The transport implementation is based on Apache HTTP Core - NIO and uses a configurable pool of non-blocking worker threads to grab incoming HTTP messages off the wire.
		</td>
	</tr>
	<tr>
		<td>HTTPS-NIO</td>
		<td>
			HTTPS-NIO transport This is also a module that comes from the Apache Synapse code base. This transport simply extend the <b>HTTP-NIO</b> implementation by adding SSL support. Therefore, they support all the configuration parameters supported by the HTTP-NIO receiver and sender plus the parameters in the following table. The sender can also <b>verify certification revocation</b>.
		</td>
	</tr>
	<tr>
		<td>HTTP Servlet</td>
		<td>
			The transport receiver implementation of the HTTP transport is available in the Carbon core component. The transport sender implementation comes from the Tomcat http connector.</br>
			<b>Note</b>:
			<ul>
  				<li>Unlike other WSO2 products, WSO2 Micro Integrator <b>does not</b> use the HTTP servlet transport by default. Instead it uses the <b>HTTP-NIO</b> transport.</li>
  				<li>This is a non-blocking HTTP transport implementation, which means that I/O the threads does not get blocked while received messages are processed.
  				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			The Virtual File System (VFS) 
		</td>
		<td>
			This transport is used to process files in the specified source directory. After processing the files, it moves them to a specified location or deletes them. Note that files cannot remain in the source directory after processing or they will be processed again, so if you need to maintain these files or keep track of which files have been processed, specify the option to move them instead of deleting them after processing. If you want to move files into a database, use the VFS transport and the <b>DBReport Mediator</b>.</br></br>
			<b>Note</b>: When you transfer a file to a remote FTP location via VFS, the integrator tries to detect the FTP location by navigating from the root folder first. If the integrator does not have <b>at least list permission</b> to the root (/), the file transfer fails.
		</td>
	</tr>
	<tr>
		<td>HTTPS Servlet</td>
		<td>
			Similar to the HTTP transport, the HTTPS transport consists of a receiver implementation which comes from the Carbon core component and a sender implementation which comes from the Apache Axis2 transport module. In fact, this transport uses exactly the same transport sender implementation as the HTTP transport. This is also a blocking transport implementation.
		</td>
	</tr>
	<tr>
		<td>RabbitMQ AMQP</td>
		<td>
			<a href="http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol">AMQP</a> is a wire-level messaging protocol that describes the format of the data that is sent across the network. If a system or application can read and write AMQP, it can exchange messages with any other system or application that understands AMQP, regardless of the implementation language. The RabbitMQ AMQP transport is implemented using the <a href="http://www.rabbitmq.com/java-client.html">RabbitMQ Java Client</a>. It allows you to send or receive AMQP messages by directly calling an AMQP broker (RabbitMQ).
		</td>
	</tr>
	<tr>
		<td>TCP</td>
		<td>
			The TCP transport allows you to send and receive SOAP messages over the Transmission Control Protocol (TCP) to the TCP Proxy through the same TCP connection. The TCP transport is included with the WSO2 Micro Integrator distribution, but it must be enabled before use.
		</td>
	</tr>
	<tr>
		<td>MSMQ</td>
		<td>
			The <b>msmq:</b> component is a transport for working with Microsoft Message Queuing . This component natively sends and receives direct allocated ByteBuffer instances. This allows you to access the JNI layer without expensive memory copying. In fact, using ByteBuffer created with the method allocateDirect can be passed to the JNI layer, and the native code is able to directly access the memory. The URI format: <code>msmq:msmqQueueName</code>.</br>
			<b>Note</b>:
			<ul>
				<li>The MSMQ examples only work on Windows, since they invoke Microsoft C++ API for MSMQ via JNDI invocation.</li>
				<li>Download the `axis2-transport-msmq-2.0.0-wso2v2.jar` file and add it to the `MI_HOME/dropins` directory. This file provides the JNI invocation required by MSMQ bridging.</li>
				<li>Make sure MQ installed and running. For more information, see <http://msdn.microsoft.com/en-us/library/aa967729.aspx>.
				</li>
				<li>Make sure that you have installed Visual C++ 2008 (VC9) and that it works with Microsoft Visual Studio 2008 Express.
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>MailTo</td>
		<td>
			When you use the Micro Integrator to mediate messages, the mediation sequence can be configured to send emails (over SMTP) or receive emails (Over POP3 or IMAP) by using the MailTo transport protocol.
		</td>
	</tr>
	<tr>
		<td>Local</td>
		<td>
			Apache Axis2's local transport implementation is used to make fast, in-VM (Virtual Machine) service calls and transfer data within proxy services. The transport does not have a receiver implementation.</br>
			<b>Note</b>:
			<ul>
				<li>WS-Security cannot be used with the local transport. Since the local is mainly used to make calls within the same VM, WS-Security is generally not required in scenarios where it is used.</li>
				<li>If you need to use local transport with callout mediator, you do not need to perform configuration mentioned in this section as callout mediator requires blocking local transport.</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>HL7</td>
		<td>
			The HL7 transport allows you to handle <a href="http://www.hl7.org/about/index.cfm?ref=common">Health Level 7 International (HL7)</a> messages.
		</td>
	</tr>
	<tr>
		<td>Financial Information eXchang (FIX)</td>
		<td>
			This transport implementation is a module developed under the Apache Synapse project. This transport is mainly used in conjunction with proxy services. This transport supports JMX. FIX transport does not support any global parameters. All the FIX configuration parameters should be specified at service level. QuickFix 4J configuration parameters can be found <a href="http://www.quickfixengine.org/quickfix/doc/html/configuration.html">here</a>.
		</td>
	</tr>
	<tr>
		<td>MQ Telemetry Transport (MQTT)</td>
		<td>
			This is a simple and lightweight network protocol for device communication. This is an easy to implement protocol that is based on the publish/subscribe model. MQTT is ideal for use in constrained environments such as the following:
			<ul>
   				<li>When the network is expensive, has low bandwidth, or is unreliable.</li>
   				<li>When running on an embedded device with limited processor or memory resources.</li>
			</ul>
			The MQTT transport implementation requires an MQTT server instance to be able to send and receive messages. The recommended MQTT server is the Mosquitto message broker.
		</td>
	</tr>
	<tr>
		<td>Websocket</td>
		<td>
			The WebSocket transport implementation of WSO2 Micro Integrator is based on the <a href="http://tools.ietf.org/html/rfc6455">WebSocket protocol</a> and consists of an Axis2 sender implementation for WebSockets and secure WebSockets. WebSocket is a protocol that provides full-duplex communication channels over a single TCP connection, and can be used by any client or server application. This transport supports bi-directional message mediation.
		</td>
	</tr>
	<tr>
		<td>UDP</td>
		<td>
			The UDP transport allows WSO2 Micro Integrator to handle messages in user datagram protocol (UDP) format.
		</td>
	</tr>
	<tr>
		<td>JMS</td>
		<td>
			The Java Message Service (JMS) transport in WSO2 Micro Integrator allows you to easily send and receive messages to queues and topics of any JMS service that implements the JMS specification.</br></br>
			Java Message Service (JMS) is a widely used API in Java-based Message Oriented Middleware(MOM) applications. It facilitates loosely coupled, reliable, and asynchronous communication between different components of a distributed application. It supports two asynchronous communication models for messaging as follows:
			<ul>
  				<li>Point-to-point model - In this model message communication happens from one JMS client to another JMS client through a dedicated queue.</li>
  				<li>Publish and subscribe model -  In this model message communication happens from one JMS client(publisher) to many JMS clients(subscribers) through a topic.</li>
			</ul> 
			JMS supports two models for messaging as follows:
			<ul>
  				<li>Queues : point-to-point.</li>
  				<li>Topics : publish and subscribe.</li>
			</ul> 
			The Micro Integrator supports the following messaging features introduced with JMS 2.0:
			<ul>
  				<li>Shared Topic Subscription</li>
  				<li>JMSX Delivery Count</li>
  				<li>JMS Message Delivery Delay</li>
			</ul>
			The JMS transport implementation comes from the WS-Commons Transports project, and it makes use of JNDI to connect to various JMS brokers. As a result, WSO2 Micro Integrator can work with any JMS broker that offers JNDI support.
		</td>
	</tr>
	<tr>
		<td>Multi-HTTPS</td>
		<td>
			This transport is similar to the HTTPS-NIO transport, but it allows you to have different SSL profiles with separate truststores and keystores for different hosts using the same WSO2 Micro Integrator. The Micro Integrator uses truststores and a keystores for SSL protocol implementation. It can listen to different host IPs and ports for incoming HTTPS connections, and each IP/Port will have a separate SSL profile configured.
		</td>
	</tr>
</table>