# Message Store Properties

See the topics given below for details.

## In Memory Message Store Properties 

Listed below are the properties that can be configured when [creating an In-Memory Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Message Store Name</td>
    <td>A name to identify the In-Memory message store.</td>
  </tr>
  <tr>
    <td>Message Store Type</td>
    <td>
      Select <b>In-Memory Message Store</b> from the drop-down list.
    </td>
  </tr>
</table>

## JMS Message Store Properties

### Required Properties

The following properties are required when [creating a JMS Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Message Store Name</td>
    <td>A unique name for the JMS message store.</td>
  </tr>
  <tr>
    <td>Message Store Type</td>
    <td>Select <b>JMS Message Store</b> from the list.</td>
  </tr>
  <tr>
    <td>Initial Context Factory</td>
    <td>
      The JNDI initial context factory class. This class must implement the java.naming.spi.InitialContextFactory interface. Initial Context Factory used to connect to the JMS broker.
    </td>
  </tr>
  <tr>
    <td>Provider URL</td>
    <td>
      The URL of the JNDI provider. URL of the naming provider to be used by the context factory.
    </td>
  </tr>
</table>

### Required Properties

The following optional properties can be configured when [creating a JMS Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>JNDI Queue Name</td>
    <td>
      The message store queue name. Though this is not a required parameter, we recommend specifying a value for this.
    </td>
  </tr>
  <tr>
    <td>Connection factory</td>
    <td>
      The JNDI name of the connection factory that is used to create jms connections. Though this is not a required parameter, we recommend specifying a value for this.
    </td>
  </tr>
  <tr>
    <td>User Name</td>
    <td>
      The user name to connect to the broker.
    </td>
  </tr>
  <tr>
    <td>Password</td>
    <td>
      The password to connect to the broker.
    </td>
  </tr>
  <tr>
    <td>JMS API Specification Version</td>
    <td>
      The JMS API version to be used. Possible values are 1.1 (default) or 1.0.
    </td>
  </tr>
  <tr>
    <td>vender.class.loader.enabled</td>
    <td>
      Set to false when using IBM MQ, which requires skipping the external class loader. Recommended when using IBM MQ.
    </td>
  </tr>
  <tr>
    <td>store.jms.cache.connection</td>
    <td>
      true/false Enable Connection caching.
    </td>
  </tr>
  <tr>
    <td>Enable Producer Guaranteed Delivery</td>
    <td>
      See the <a href="#properties-guaranteed-delivery-of-messages">Properties for Guaranteed Delivery of Messages</a>.
    </td>
  </tr>
  <tr>
    <td>Failover Message Store</td>
    <td>
      See the <a href="#properties-guaranteed-delivery-of-messages">Properties for Guaranteed Delivery of Messages</a>.
    </td>
  </tr>
</table>

## RabbitMQ Message Store Properties

###  Required Properties

The following optional properties can be configured when [creating a RabbitMQ Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>A unique name for the RabbitMQ message store.</td>
  </tr>
  <tr>
    <td>RabbitMQ Server Host Name</td>
    <td>The address of the RabbitMQ broker.</td>
  </tr>
  <tr>
    <td>RabbitMQ Server Host Port</td>
    <td>The port number of the RabbitMQ message broker.</td>
  </tr>
  <tr>
    <td>SSL Enabled</td>
    <td>
      Whether or not SSL is enabled on the message store. When SSL Enabled is set to true, you can set the parameters relating to the SSL configuration. For descriptions of each of these parameters you can set, see SSL enabled RabbitMQ message store parameters.
    </td>
  </tr>
</table>

### Optional Properties

The following optional properties can be configured when [creating a RabbitMQ Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>RabbitMQ Queue Name (store.rabbitmq.queue.name)</td>
    <td>
      The message store queue name. Though this is not a required parameter, we recommend specifying a value for this.
    </td>
  </tr>
  <tr>
    <td>RabbitMQ Exchange Name (store.rabbitmq.exchange.name)</td>
    <td>
      The name of the RabbitMQ exchange to which the queue is bound (If the ESB profile has to declare this exchange it will be declared as a direct exchange).
    </td>
  </tr>
  <tr>
    <td>Routing key (store.rabbitmq.route.key)</td>
    <td>
      The exchange and queue binding value. Messages will be routed using this. This is considered only when both the exchange name and type are provided. Else messages will be routed using the default exchange and queue name as the routing key.
    </td>
  </tr>
  <tr>
    <td>User Name (store.rabbitmq.username)</td>
    <td>
      The user name to connect to the broker.
    </td>
  </tr>
  <tr>
    <td>Password (store.rabbitmq.password)</td>
    <td>
      The password to connect to the broker.
    </td>
  </tr>
  <tr>
    <td>Virtual Host (store.rabbitmq.virtual.host)</td>
    <td>
      The virtual host name of the broker.
    </td>
  </tr>
  <tr>
    <td>Enable Producer Guaranteed Delivery</td>
    <td>
      See the <a href="#properties-guaranteed-delivery-of-messages">Properties for Guaranteed Delivery of Messages</a>.
    </td>
  </tr>
  <tr>
    <td>Failover Message Store</td>
    <td>
      See the <a href="#properties-guaranteed-delivery-of-messages">Properties for Guaranteed Delivery of Messages</a>.
    </td>
  </tr>
</table>

### SSL Properties

!!! Note
    Configuring parameters that provide information related to keystores and truststores can be optional based on your broker configuration. For example, if `         fail_if_no_peer_cert        ` is set to `         false        ` in the RabbitMQ broker configuration, then you only need to specify `         <parameter name="rabbitmq.connection.ssl.enabled" locked="false">true</parameter>        `. Additionally, you can also set `         <parameter name="rabbitmq.connection.ssl.version" locked="false">true</parameter>        ` parameter to specify the SSL version. If `         fail_if_no_peer_cert        ` is set to `         true        ` , you need to provide keystore and truststore information.

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>SSL Key Store Location</td>
    <td>The location of the keystore file.</td>
  </tr>
  <tr>
    <td>SSL Key Store Type</td>
    <td>
      The type of the keystore used (e.g., JKS, PKCS12).
    </td>
  </tr>
  <tr>
    <td>SSL Key Store Password</td>
    <td>
      The password to access the keystore.
    </td>
  </tr>
  <tr>
    <td>SSL Trust Store Location</td>
    <td>
      The location of the Java keystore file containing the collection of CA certificates trusted by this application process (truststore).
    </td>
  </tr>
  <tr>
    <td>SSL Trust Store Type</td>
    <td>
      The type of the truststore used.
    </td>
  </tr>
  <tr>
    <td>SSL Trust Store Password</td>
    <td>
      The password to unlock the trust store file specified in rabbitmq.connection.ssl.truststore.location.
    </td>
  </tr>
  <tr>
    <td>SSL Version</td>
    <td>
      SSL protocol version (e.g., SSL, TLSV1, TLSV1.2).
    </td>
  </tr>
</table>

## JDBC Message Store Properties

### Required Properties

The following properties are required when [creating a JDBC Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>
      The name of the message store.
    </td>
  </tr>
  <tr>
    <td>Database Table</td>
    <td>
      The name of the database table.
    </td>
  </tr>
  <tr>
    <td>Driver</td>
    <td>
      The class name of the database driver.
    </td>
  </tr>
  <tr>
    <td>URL</td>
    <td>
      The JDBC URL of the database that the data will be written to.
    </td>
  </tr>
  <tr>
    <td>User</td>
    <td>
      The user name used to connect to the database.
    </td>
  </tr>
  <tr>
    <td>Password</td>
    <td>
      The password used to connect to the database.
    </td>
  </tr>
</table>

### Connection Pool Properties

The syntax of the JDBC message store can be different depending on whether you connect to the database using a connection pool, or using a datasource. Given below are the connection pool properties:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>store.jdbc.driver </td>
    <td>
      The class name of the database driver.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.connection.url</td>
    <td>
      The database URL.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.username </td>
    <td>
      The user name to access the database.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.password</td>
    <td>
      The password to access the database.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.table  </td>
    <td>
      Table name of the database.
    </td>
  </tr>
</table>

### External Datasource Properties

The syntax of the JDBC message store can be different depending on whether you connect to the database using a connection pool, or using a datasource. Given below are the external datasource properties:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>store.jdbc.dsName</td>
    <td>
      The name of the datasource to be looked up.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.table</td>
    <td>
      The table name of the database.
    </td>
  </tr>
</table>

### Internal Datasource Properties

To make sure that the datasource appears in the **Datasource Name** list, you need to expose it as a JNDI datasource.

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>The name for the message store.</td>
  </tr>
  <tr>
    <td>Database Table</td>
    <td>
      The name of the database table.
    </td>
  </tr>
  <tr>
    <td>Datasource Name</td>
    <td>
      The class name of the datasource.
    </td>
  </tr>
</table>

## Resequence Message Store Properties

### Required Properties

The following properties are required when [creating a Resequencer Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Name</th>
    <th>
      The name of the message store.
    </th>
  </tr>
  <tr>
    <td>Database Table</td>
    <td>
      The name of the database table.
    </td>
  </tr>
  <tr>
    <td>Driver</td>
    <td>
      The class name of the database driver.
    </td>
  </tr>
  <tr>
    <td>URL</td>
    <td>
      The JDBC URL of the database that the data will be written to.
    </td>
  </tr>
  <tr>
    <td>User</td>
    <td>
      The user name used to connect to the database.
    </td>
  </tr>
  <tr>
    <td>Password</td>
    <td>
      The password used to connect to the database.
    </td>
  </tr>
  <tr>
    <td>Resequence Timeout (Seconds)</td>
    <td>
      The time the Message Processor waits for a message, which is missing to reorder them based on a specified order.
    </td>
  </tr>
  <tr>
    <td>Sequence ID Path</td>
    <td>
      The path from which, the Store identifies the sequence ID from the message content.
    </td>
  </tr>
</table>

### Optional Properties

The following optional properties can be configured when [creating a Resequencer Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
   <thead>
      <tr>
         <th>Property</th>
         <th>Description</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>store.resequence.timeout</td>
         <td>
            The time the Processor waits for a message, which is missing to reorder them based on a specified order. If the current message sequence is 3, its predecessor would be 2 and the successor would be 4, and thereby, if 4 is not present, then there is a gap in the sequence. Therefore, the processor waits until the specified set time-out exceeds, and selects the next minimum sequence ID available as the predecessor. However, the time-out will be a rough estimate. The durations could vary depending on the load of the machine. Specify a positive integer for the count and the timeout in seconds. If you specify the count as -1, then the processor will wait indefinitely until the correct sequence ID is present to fill the gap.
         </td>
      </tr>
      <tr>
         <td>
            store.producer.guaranteed.delivery.enable
         </td>
         <td>Whether you want to enable guaranteed delivery or not. For more information, see <b>Guaranteed Delivery with Failover Message Store</b> and <b>Scheduled Failover Message Forwarding Processor</b>. Set True/False.</td>
      </tr>
      <tr>
         <td>
            store.failover.message.store.name</pre>
         </td>
         <td>The name of the Message Store used if the original message store fails. For more information, see <b>Guaranteed Delivery with Failover Message Store</b> and <b>Scheduled Failover Message Forwarding Processor</b>. An appropriate String value</td>
      </tr>
      <tr>
         <td>store.resequence.id.path</td>
         <td>
            <p>The path from which, the Store identifies the sequence ID from the message content. The path could be either XPath or JSON. You can specify it in the expression field. Set a positive integer as the sequence ID. The store expects the sequence ID to start from 1.</p>
         </td>
      </tr>
      <tr>
         <td>store.jdbc.password</td>
         <td>The password to access the database. An appropriate String value</td>
      </tr>
      <tr>
         <td>store.jdbc.driver</td>
         <td>The class name of the database driver. An appropriate String value.</td>
      </tr>
      <tr>
         <td>store.jdbc.username</td>
         <td>The username to access the database. An appropriate String value.</td>
      </tr>
      <tr>
         <td>store.jdbc.connection.url</td>
         <td>The database URL. An appropriate String value of a URL.</td>
      </tr>
      <tr>
         <td>store.jdbc.table</td>
         <td>Table name of the database in which, messages will be stored. An appropriate String value</td>
      </tr>
   </tbody>
</table>

## Properties: Custom Message Store

The following properties can be configured when [creating a Custom Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>A unique name for the message store.</td>
  </tr>
  <tr>
    <td>Provider Class</td>
    <td>
      Fully qualified name of the message store implementation class.
    </td>
  </tr>
</table>

## WSO2 MB Message Store Properties

The following properties can be configured when [creating a WSO2 MB Message Store](../../develop/creating-artifacts/creating-a-message-store.md).

<table>
   <thead>
      <tr>
         <th>
            <p>Parameter Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>Message Store Name</td>
         <td>Give a unique name for the JMS message store.</td>
      </tr>
      <tr>
         <td>Message Store Type</td>
         <td>Select <strong>WSO2 MB Message Store</strong> from the list of options.</td>
      </tr>
      <tr>
         <td>Initial Context Factory</td>
         <td>This specifies the JNDI initial context factory class ( <code>             java.naming.factory.initial            </code> ). This class implements the implement the <code>             java.naming.spi.InitialContextFactory            </code> interface. The value is set to <code>             org.wso2.andes.jndi.PropertiesFileInitialContextFactory            </code> , by default.</td>
      </tr>
      <tr>
         <td>Queue Connection Factory</td>
         <td>
            <div class="content-wrapper">
               <p>This is the connection factory URlfor connecting to WSO2 MB. By default, the value is set to <code>               amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5673'              </code> .</p>
               <b>Important!</b>
               <p>Be sure to change the port to 5675.</p>
            </div>
         </td>
      </tr>
      <tr>
         <td>JNDI Queue Name</td>
         <td>The name of the queue in the broker that will store messages.</td>
      </tr>
      <tr>
         <td>JMS API Specification Version</td>
         <td>The JMS API version to be used. Possible values are 1.1 or 1.0. The value is set to 1.1, by default.</td>
      </tr>
   </tbody>
</table>

## Properties: Guaranteed Delivery of Messages

If you need to ensure guaranteed delivery of your messages, specify values for the following parameters:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Enable Producer Guaranteed Delivery</td>
    <td>
      This flag specifies whether guaranteed delivery is enabled on the producer side. The value is set to <code>false</code> by default.
    </td>
  </tr>
  <tr>
    <td>Failover Message Store</td>
    <td>
      The message store to which the store mediator should send messages when the original message store fails.
    </td>
  </tr>
</table> 