# Configuring Transports

A transport protocol is responsible for carrying messages that are in a specific format. WSO2 Micro Integrator supports all the widely used transports including HTTP/S, JMS, VFS, as well as domain-specific transports like FIX. Each transport provides a receiver implementation for receiving messages, and a sender implementation for sending messages.

## Configuring the HTTP/HTTPS transport

The HTTP and HTTPS passthrough transports are enabled by defualt in the Micro Integrator. 

See the [complete list of HTTP/HTTPS parameters](../../../references/config-catalog/#http-transport).

## Configuring the VFS transport

The VFS transport is enabled by defualt in the Micro Integrator. 

See the [complete list of VFS parameters](../../../references/config-catalog/#vfs-transport).

## Configuring the Local transport

The local transport is enabled by defualt in the Micro Integrator. 

See the [complete list of local parameters](../../../references/config-catalog/#local-transport).

## Configuring the RabbitMQ transport

### Enabling the RabbitMQ transport 

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.listener]]
name = "rabbitMQListener"
parameter.hostname = "localhost"
parameter.port = 5672
parameter.username = "guest"
parameter.password = "guest"
parameter.connection_factory = ""
```

Download the "amqp-client-5.7.0.jar" and copy it into `MI_HOME/lib` directory.

### Enabling SSL

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.listener]]
parameter.ssl_enable = true
parameter.ssl_version = "SSL"
parameter.keystore_file_name ="$ref{keystore.tls.file_name}"
parameter.keystore_type = "$ref{keystore.tls.type}"
parameter.keystore_password = "$ref{keystore.tls.password}"
parameter.truststore_file_name ="$ref{truststore.file_name}"
parameter.truststore_type = "$ref{truststore.type}"
parameter.truststore_password = "$ref{truststore.password}"
```
See the complete list of server-level configurations for the [RabbitMQ Listener](../../../references/config-catalog/#rabbitmq-listener) and [RabbitMQ Sender](../../../references/config-catalog/#rabbitmq-sender).

### Configuring connection recovery

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.listener]]
parameter.retry_interval = "10"
parameter.retry_count = 5  
```

In case of a network failure or broker shutdown, WSO2 Micro Integrator will try to recreate the connection. The following parameters need to be configured in the transport listener to enable connection recovery in RabbitMQ.

If the parameters specified above are set, the Micro Integrator will retry 5 times with 10000 ms time intervals to reconnect when the connection is lost. If reconnecting also fails, the Micro Integrator will terminate the connection. If you do not specify values for the above parameters, the Micro Integrator uses 30000ms as the default retry interval and 3 as the default retry count.

Optionally, you can configure the following parameter in your proxy service when you define your mediation sequence:

<parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter> 

The parameter specified above sets the retry interval with which the RabbitMQ client tries to reconnect. Generally having this value less than the value specified as `rabbitmq.connection.retry.interval` will help synchronize the reconnection of the Micro Integrator and the RabbitMQ client.

### Configuring high throughput of message delivery

For increased performance and higher throughput in message delivery, configure the transport sender as shown below.

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.sender]]
<parameter name="CachedRabbitMQConnectionFactory" locked="false">
```
When configuring the proxy service, be sure to add the following connection factory parameter in the address URI: `rabbitmq.connection.factory=CachedRabbitMQConnectionFactory`.

## Configuring the TCP transport

To enable the TCP transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.tcp]
listener.enable = false
sender.enable = true
```

## Configuring the MSMQ transport

To enable the MSMQ transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.msmq]
listener.enable = false
sender.enable = false
```
<b>Note</b> the following:
<ul>
    <li>The MSMQ examples only work on Windows since they invoke the Microsoft C++ API for MSMQ via JNDI invocation.</li>
    <li>Download the `axis2-transport-msmq-2.0.0-wso2v2.jar` file and add it to the `MI_HOME/dropins` directory. This file provides the JNI invocation required by MSMQ bridging.</li>
    <li>Make sure that MQ is installed and running. For more information, see <http://msdn.microsoft.com/en-us/library/aa967729.aspx>.
    </li>
    <li>Make sure that you have installed Visual C++ 2008 (VC9) and that it works with Microsoft Visual Studio 2008 Express.
    </li>
</ul>

## Configuring the HL7 transport

To enable the HL7 transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.hl7]
listener.enable = false
sender.enable = false
```

Add the following sections to the deployment.toml file to enable the required message builders and formatters:

```toml tab='Message Builder'
[[custom_message_builders]]
content_type = "application/edi-hl7"
class="org.wso2.carbon.business.messaging.hl7.message.HL7MessageBuilder"
```

```toml tab='Message Formatter'
[[custom_message_formatters]]
content_type = "application/edi-hl7"
class="org.wso2.carbon.business.messaging.hl7.message.HL7MessageFormatter"
```

## Configuring the FIX transport

To enable the FIX transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.fix]
listener.enable = false
sender.enable = false   
```

Download Quickfix/J and in the distribution archive you will find all the dependencies listed below. Also please refer to [Quickfix/J](https://www.quickfixj.org/) documentation on configuring FIX acceptors and initiators. <b>Note</b> that the FIX transport does not support any global parameters. All the FIX configuration parameters should be specified at service level.

-	mina-core.jar
-	quickfixj-core.jar
-	quickfixj-msg-fix40.jar
-	quickfixj-msg-fix41.jar
-	quickfixj-msg-fix42.jar
-	quickfixj-msg-fix43.jar
-	quickfixj-msg-fix44.jar
-	slf4j-api.jar
-	slf4j-log4j12.jar

## Configuring the MQTT transport

To enable the MQTT transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.mqtt]
listener.enable = false
sender.enable = false
```
## Configuring the Websocket transport

To enable the Websocket transport sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.ws]
sender.enable = false
```

To enable the **secured** Websocket transport sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory.

```toml
[transport.wss]
sender.enable = false
```

## Configuring the UDP transport

To enable the MSMQ transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[transport.udp]
listener.enable = false
sender.enable =false
```
## Configuring the MailTo transport 

To enable the MSMQ transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml tab='MailTo listener'
[transport.mail.listener]
enable = true
```

```toml tab='MailTo sender'
[[transport.mail.sender]]
name = "mailto"
parameter.hostname = "smtp.gmail.com"
parameter.port = "587"
parameter.enable_tls = true
parameter.auth = true
parameter.username = "demo_user"
parameter.password = "mailpassword"
parameter.from = "demo_user@wso2.com"
```

## Configuring the JMS transport

To enable the JMS transport sender and listener in the Micro Integrator, you need to update the deployment.toml file (stored in the `MI_HOME/conf` directory) with the connection parameters for your JMS broker you are using. **Be sure** to add the required libraries to the `MI_HOME/lib` directory.

### ActiveMQ broker 



## Configuring the Multi-HTTPS transport

This transport is similar to the HTTPS passthrough transport, but it allows you to have different SSL profiles with separate truststores and keystores for different hosts using the same WSO2 Micro Integrator. It can listen to different host IPs and ports for incoming HTTPS connections, and each IP/Port will have a separate SSL profile configured.
