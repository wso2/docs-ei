# Creating a Message Store

Follow the instructions given below to create a new Message Store artifact in WSO2 Integration Studio.

## Instructions

1.  If you have already created an [ESB Config project](../../creating-projects/#esb-config-project), right-click the project and go to **New → Message Store** to open the **New Message Store Artifact** dialog.
2.  Leave the **Create a new message-store artifact** option selected and click **Next**.
3.  Type a unique name for this message store, specify the type of store you are creating, and then specify values for the other fields required to create the store type you you selected.

	<table>
		<tr>
			<th>Message Store TYpe</th>
			<th>Description</th>
		</tr>
		<tr>
			<td><b>In Memory Message Store</b></td>
			<td>
				In memory message store is a basic Message Store that stores messages in an in-memory queue. Since the messages are stored in an in-memory queue in case of a ESB profile restart, all the messages stored will be lost. </br> 
				The in memory message store is lot faster than a persistent message store implementation, so it can be used to temporarily store messages for use cases such as the implementation of a high-speed store and forwarded pattern where message persistence is not a requirement.
			</td>
		</tr>
		<tr>
			<td><b>JMS Message Store</b></td>
			<td>
				The JMS message store persists messages in a JMS queue inside a JMS Broker. The JMS message store can be configured by specifying the class as org.apache.synapse.message.store.impl.jms.JmsStore. Since the JMS message stores persist messages in a JMS queue in an ordered manner, JMS message stores can be used to implement the store-and-forward pattern.</br> </br>
				<b>Required Parameters</b>:</br>
				<ul>
					<li>
						<b>Name</b>: A unique name for the JMS message store.
					</li>
					<li>
						<b>Initial Context Factory</b>: The JNDI initial context factory class. This class must implement the java.naming.spi.InitialContextFactory interface. Initial Context Factory used to connect to the JMS broker.
					</li>
					<li>
						<b>Provider URL</b>: The URL of the JNDI provider. Url of the naming provider to be used by the context factory.
					</li>
				</ul>
				<b>Additional Parameters</b>:</br>
				<ul>
					<li>
						<b>JNDI Queue Name</b>: The message store queue name. Though this is not a required parameter, we recommend specifying a value for this. 
					</li>
					<li>
						<b>Connection factory</b>: The JNDI name of the connection factory that is used to create jms connections. Though this is not a required parameter, we recommend specifying a value for this.
					</li>
					<li>
						<b>User Name</b>: The user name to connect to the broker.
					</li>
					<li>
						<b>Password</b>: The password to connect to the broker.
					</li>
					<li>
						<b>JMS API Specification Version</b>: The JMS API version to be used. Possible values are 1.1 (default) or 1.0.
					</li>
					<li>
						<b>vender.class.loader.enabled</b>: Set to false when using IBM MQ, which requires skipping the external class loader. Recommended when using IBM MQ.
					</li>
					<li>
						<b>store.jms.cache.connection</b>: true/false Enable Connection caching.
					</li>
				</ul></br></br>
				If you need to ensure guaranteed delivery when you store incoming messages to a JMS message store, and later deliver them to a particular backend, click Show Guaranteed Delivery Parameters and specify values for the following parameters:
				<code>Enable Producer Guaranteed Delivery</code>: `store.producer.guaranteed.delivery.enable `: Whether it is required to enable guaranteed delivery on the producer side.
				<code>Failover Message Store</code>: `store.failover.message.store.name`: The message store to which the store mediator should send messages when the original message store fails.
			</td>
		</tr>
		<tr>
			<td><b>RabbitMQ Message Store</b></td>
			<td>
				RabbitMQ message store persists messages in a RabbitMQ queue inside a RabbitMQ broker. The RabbitMQ message store can be configured by specifying the class as org.apache.synapse.message.store.impl.rabbitmq.RabbitmqStore and then setting all other required parameters to connect to a RabbitMQ broker.</br> </br>
				<b>Required Parameters</b>:</br>
				<ul>
					<li>
						<b>Name</b>: A unique name for the RabbitMQ message store.
					</li>
					<li>
						<b>RabbitMQ Server Host Name</b>: The address of the RabbitMQ broker.
					</li>
					<li>
						<b>RabbitMQ Server Host Port</b>: The port number of the RabbitMQ message broker.
					</li>
					<li>
						<b>SSL Enabled</b>: Whether or not SSL is enabled on the message store. When SSL Enabled is set to true, you can set the parameters relating to the SSL configuration. For descriptions of each of these parameters you can set, see SSL enabled RabbitMQ message store parameters.
					</li>
					<b>SSL-enabled RabbitMQ parameters</b>
					<ul>
						<li>
							<b>SSL Key Store Location</b>: The location of the keystore file.
						</li>
						<li>
							<b>SSL Key Store Type</b>: The type of the keystore used (e.g., JKS, PKCS12).
						</li>
						<li>
							<b>SSL Key Store Password </b>: The password to access the keystore.
						</li>
						<li>
							<b>SSL Trust Store Location </b>: The location of the Java keystore file containing the collection of CA certificates trusted by this application process (truststore).
						</li>
						<li>
							<b>SSL Trust Store Type </b>: The type of the truststore used.
						</li>
						<li>
							<b>SSL Trust Store Password </b>: The password to unlock the trust store file specified in rabbitmq.connection.ssl.truststore.location.
						</li>
						<li>
							<b>SSL Version</b>: SSL protocol version (e.g., SSL, TLSV1, TLSV1.2).
						</li>
					</ul>
				</ul>
			</td>
		</tr>
		<tr>
			<td><b>JDBC Message Store</b></td>
			<td>
				The JDBC message store can be used to store and retrieve messages more efficiently in comparison with other message stores. The JDBC message store implementation is a variation of the already existing synapse message store implementation and is designed in a manner similar to the message store of the ESB profile. The JDBC message store uses a JDBC connector to connect to external relational databases.</br> </br>
				<b>Required Parameters</b>:</br>
				<ul>
					<li><b>Name</b>: The name of the message store.</li>
					<li><b>Database Table</b>: The name of the database table.</li>
					<li><b>Connection Information</b>:</li>
					<li><b>RDBMS Type</b></li>
					<li><b>Driver</b>: The class name of the database driver.</li>
					<li><b>URL</b>: The JDBC URL of the database that the data will be written to.</li>
					<li><b>User</b>:The user name used to connect to the database.</li>
					<li><b>Password</b>: The password used to connect to the database.</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td><b>Resequence Message Store</b></td>
			<td>
				The Resequence Message Store is used to store a stream of related but out-of-sequence messages to put them back into the correct order. It collects and re-orders the stored messages based on a defined sequence number derived from some part of the message, so that they can be published to the output channel in a specific order. This is advantageous specially when the order of message delivery is important to avoid some messages arriving earlier than others.</br> </br>
				The resequencing store is an extension of the existing JDBC-based message store. Hence, it inherits most of its properties from the JDBC Message Store.</br></br>
				<b>Required Parameters</b>:</br>
				<ul>
					<li><b>Name</b>: The name of the message store.</li>
					<li><b>Database Table</b>: The name of the database table.</li>
					<li><b>Connection Information</b>:</li>
					<li><b>RDBMS Type</b></li>
					<li><b>Driver</b>: The class name of the database driver.</li>
					<li><b>URL</b>: The JDBC URL of the database that the data will be written to.</li>
					<li><b>User</b>:The user name used to connect to the database.</li>
					<li><b>Password</b>: The password used to connect to the database.</li>
					<li><b>Resequence Timeout (Seconds)</b>: The time the Message Processor waits for a message, which is missing to reorder them based on a specified order.</li>
					<li><b>Sequence ID Path</b>: The path from which, the Store identifies the sequence ID from the message content.</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td><b>WSO2 MB Message Store</b></td>
			<td></td>
		</tr>
		<tr>
			<td><b>Custom Message Store</b></td>
			<td>
				Custom Message Store allows users to create a message store with their own message store implementation. It can be configured using configuration by giving the fully qualified class name of the message store implementation as the class value. 
				Messages will be stored as specified in the underlying message store implementation. Parameter configuration can be used to pass any configuration parameters that is needed by the message store implementation class.</br></br>
				<b>Required Parameters</b>:</br>
				<ul>
					<li><b>Name</b>: Unique name of the message store.</li>
					<li><b>Provider Class</b>: Fully qualified name of the message store implementation class.</li>
				</ul>
			</td>
		</tr>
	</table>

4.  Click **Finish**. The message store is created in the `src/main/synapse-config/message-stores` folder under the ESB Config project you specified and appears in the editor. 

## Properties

You can click its icon in the editor to view its properties.

## Examples
..

## Guides
..
