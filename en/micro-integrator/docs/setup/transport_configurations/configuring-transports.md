# Configuring Transports

A transport protocol is responsible for carrying messages that are in a specific format. WSO2 Micro Integrator supports all the widely used transports including HTTP/S, JMS, VFS, as well as domain-specific transports like FIX. Each transport provides a receiver implementation for receiving messages, and a sender implementation for sending messages.

## Configuring the HTTP/HTTPS transport

The HTTP and HTTPS passthrough transports are enabled by defualt in the Micro Integrator. 

See the [complete list of HTTP/HTTPS parameters](../../../references/config-catalog/#http-transport).

## Configuring the VFS transport

The VFS transport is enabled by defualt in the Micro Integrator. The VFS transport supports the **SFTP protocol** with **Secure Sockets Layer (SSL)**. The configuration is identical to other protocols with the only difference being the URL prefixes and parameters. 

For more information, see [VFS URL parameters](../../references/synapse-properties/vfs-transport-parameters).

See the [complete list of VFS parameters](../../../references/config-catalog/#vfs-transport).

## Connecting to the Local transport

The local transport is enabled by defualt in the Micro Integrator. 

See the [complete list of local parameters](../../../references/config-catalog/#local-transport).

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

To enable the MailTo transport listener and sender, set the following parameters to `true` in the deployment.toml file (stored in the `MI_HOME/conf` directory).

-	Enabling the MailTo listener

	```toml
	[transport.mail.listener]
	enable = true
	```

-	Configuring the MailTo sender

	```toml
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

	!!! Note
		In addition to enabling the MailTO transport, the following parameters are used in the above configuration to set a default email account as the mail sender. You can override this default mail sender by specifying an email sender account within your mediation sequence.

		-	`mail.smtp.from` : The email address from which mails will be sent.
		-	`mail.smtp.user` : The user name of the email account (mail sender). Note that in some email service providers, the user name is the same as the email address specified for the 'From' parameter.
		-	`mail.smtp.password` : The password of the email account (mail sender).

	If you want to use multiple mail boxes to send emails, make a copy of the default MailTo sender configuration in the `MI_HOME/conf/deployment.toml` file and change the transport sender name. For example, add `mailtoWSO2` as the name.

	For a list of parameters supported by the MailTo transport sender, see [SMTP Package Summary](https://javaee.github.io/javamail/docs/api/com/sun/mail/smtp/package-summary.html). In addition to the parameters described there, the MailTo transport sender supports the following parameters.

	-	`transport.mail.SMTPBccAddresses` : If one or more e-mail addresses need to be specified as BCC addresses for outgoing mails, this parameter can be used. You can enter a comma-separated list of e-mail addresses.
	-	`transport.mail.Format` : Format of the outgoing mail. Possible values are <b>Text</b> and <b>Multipart</b>.

## Configuring the JMS transport

To enable the JMS transport sender and listener in the Micro Integrator, you need to update the deployment.toml file (stored in the `MI_HOME/conf` directory) with the connection parameters for your JMS broker you are using. **Be sure** to add the required libraries to the `MI_HOME/lib` directory.

See the following topics for instructions on how to configure the Micro Integrator with different types of brokers:

-	[Connecting to ActiveMQ](../../setup/brokers/configure-with-ActiveMQ.md)
-	[Connecting to Apache Artemis](../../setup/brokers/configure-with-Apache-Artemis.md)
-	[Connecting to HornetQ](../../setup/brokers/configure-with-HornetQ.md)
-	[Connecting to IBM WebSphere App Server](../../setup/brokers/configure-with-IBM-websphere-app-server.md)
-	[Connecting to IBM WebSphere MQ](../../setup/brokers/configure-with-IBM-websphereMQ.md)
-	[Connecting to JBossMQ](../../setup/brokers/configure-with-JBossMQ.md)
-	[Connecting to MSMQ](../../setup/brokers/configure-with-MSMQ.md)
-	[Connecting to RabbitMQ](../../setup/brokers/configure-with-rabbitMQ.md)
-	[Connecting to SwiftMQ](../../setup/brokers/configure-with-SwiftMQ.md)
-	[Connecting to Tibco EMS](../../setup/brokers/configure-with-Tibco-EMS.md)
-	[Connecting to Oracle Weblogic](../../setup/brokers/configure-with-WebLogic.md)
-	[Connecting to WSO2 MB](../../setup/brokers/configure-with-WSO2-MB.md)
-	[Connecting to Multiple Brokers](../../setup/brokers/configure-with-multiple-brokers.md)

## Configuring the Multi-HTTPS transport

This transport is similar to the HTTPS passthrough transport, but it allows you to have different SSL profiles with separate truststores and keystores for different hosts using the same WSO2 Micro Integrator. It can listen to different host IPs and ports for incoming HTTPS connections, and each IP/Port will have a separate SSL profile configured.
