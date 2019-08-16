# Message Store Properties

You can click its icon in the editor to view its properties.

## Properties: In Memory Message Store 

Listed below are the properties that can be configured for the In-Memory Message Store.

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

## Properties: JMS Message Store

Following is a sample JMS message store configuration that uses the
Message Broker profile of WSO2 EI as the message broker:

``` java tab='WSO2 MB'
    <messageStore name="JMSMS" class="org.apache.synapse.message.store.impl.jms.JmsStore" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
       <parameter name="java.naming.provider.url">/conf/jndi.properties</parameter>
       <parameter name="store.jms.destination">ordersQueue</parameter>
       <parameter name="store.jms.connection.factory">QueueConnectionFactory</parameter>
       <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
    </messageStore>
```

``` java tab='ActiveMQ'
    <messageStore name="JMSMS" class="org.apache.synapse.message.store.impl.jms.JmsStore" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
       <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
       <parameter name="store.jms.destination">ordersQueue</parameter>
       <parameter name="store.jms.connection.factory">QueueConnectionFactory</parameter>
       <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
    </messageStore>
```

Listed below are the properties that are required for the JMS Message Store.

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>A unique name for the JMS message store.</td>
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

Listed below are the additional properties that can be configured for the JMS Message Store.

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
      dsfd
    </td>
  </tr>
  <tr>
    <td>Failover Message Store</td>
    <td>
      ddd
    </td>
  </tr>
</table>

## Properties: RabbitMQ Message Store

Following is a basic sample RabbitMQ message store configuration:

``` java tab='Basic RabbitMQ'
    <messageStore class="org.apache.synapse.message.store.impl.rabbitMQ.RabbitMQStore" name="RabbitMS" xmlns="http://ws.apache.org/ns/synapse">
      <parameter name="store.rabbitmq.host.name">localhost</parameter>
      <parameter name="store.rabbitmq.queue.name">EIStore</parameter>
      <parameter name="store.rabbitmq.host.port">5672</parameter>
      <parameter name="store.rabbitmq.username">guest</parameter>
      <parameter name="store.rabbitmq.password">guest</parameter>
      <parameter name="store.rabbitmq.exchange.name">exchange</parameter>
    </messageStore>   
```

``` java tab='SSL-Enabled RabbitMQ'
    <messageStore xmlns="http://ws.apache.org/ns/synapse"
                  class="org.apache.synapse.message.store.impl.rabbitmq.RabbitMQStore"
                  name="store1">
       <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
       <parameter name="store.failover.message.store.name">store1</parameter>
       <parameter name="store.rabbitmq.host.name">localhost</parameter>
       <parameter name="store.rabbitmq.queue.name">WithoutClientCertQueue</parameter>
       <parameter name="store.rabbitmq.host.port">5671</parameter>
       <parameter name="rabbitmq.connection.ssl.enabled">true</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.location">path/to/truststore</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.type">JKS</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.password">truststorepassword</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.location">path/to/keystore</parameter>     
       <parameter name="rabbitmq.connection.ssl.keystore.type">PKCS12</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.password">keystorepassword</parameter>   
       <parameter name="rabbitmq.connection.ssl.version">SSL</parameter>   
    </messageStore>
```

Listed below are the properties that are required for the RabbitMQ Message Store.

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

Listed below are the optional properties for the RabbitMQ Message Store.

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
</table>

Listed below are the SSL properties that are required for the RabbitMQ Message Store.

!!! Info
  Configuring parameters that provide information related to keystores and truststores can be optional based on your broker configuration.
  For example, if `         fail_if_no_peer_cert        ` is set to `         false        ` in the RabbitMQ broker configuration, then you only need to specify `         <parameter name="rabbitmq.connection.ssl.enabled" locked="false">true</parameter>        `. Additionally, you can also set
  `         <parameter name="rabbitmq.connection.ssl.version" locked="false">true</parameter>        ` parameter to specify the SSL version. If `         fail_if_no_peer_cert        ` is set to `         true        ` , you need to provide keystore and truststore information.

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

## Properties: JDBC Message Store

The syntax of the JDBC message store can be different depending on whether you connect to the database using a connection pool, or using a
datasource. Click on the relevant tab to view the syntax based on how you want to connect to the database.

``` java tab="Connection Pool"
<messageStore class="org.apache.synapse.message.store.jdbc.JDBCMessageStore" name="MyStore" xmlns="http://ws.apache.org/ns/synapse">  
    <parameter name="store.jdbc.driver">Driver</parameter>
    <parameter name="store.jdbc.connection.url">ConnectionURL</parameter>
    <parameter name="store.jdbc.username">Username</parameter>
    <parameter name="store.jdbc.password">Password</parameter>
    <parameter name="store.jdbc.table">jdbcTable</parameter>  
</messageStore>
```

``` java tab="External Datasource"
<messageStore class="org.apache.synapse.message.store.jdbc.JDBCMessageStore" name="MyStore" xmlns="http://ws.apache.org/ns/synapse">
    <parameter name="store.jdbc.dsName">dsName</parameter>
    <parameter name="store.jdbc.table">jdbcTable</parameter>
</messageStore>
```

Listed below are the properties that are required for the JDBC Message Store.

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
    <td>Connection Information</td>
    <td>...</td>
  </tr>
  <tr>
    <td>RDBMS Type</td>
    <td>
      mmmmm
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

### Parameters (Required)

Following are descriptions of the fields that are displayed:

| Field          | Description                                                    |
|----------------|----------------------------------------------------------------|
| Name           | The name of the message store                                  |
| Database Table | The name of the database table.                                |
| Driver         | The class name of the database driver.                         |
| Url            | The JDBC URL of the database that the data will be written to. |
| User           | The user name used to connect to the database.                 |
| Password       | The password used to connect to the database.                  |

### Parameters (Connection pool)

Following are the parameters that should be specified and the
description for each:

| Parameter                                                    | Description                            | Required |
|--------------------------------------------------------------|----------------------------------------|----------|
| `                 store.jdbc.driver                `         | The class name of the database driver. | YES      |
| `                 store.jdbc.connection.url                ` | The database URL.                      | YES      |
| `                 store.jdbc.username                `       | The user name to access the database.  | YES      |
| `                 store.jdbc.password                `       | The password to access the database.   | NO       |
| `                 store.jdbc.table                `          | Table name of the database.            | YES      |

### Parameters (External Datasource)

Following are the parameters that should be specified and the description for each:

| Parameter                                            | Description                                | Required |
|:-----------------------------------------------------|--------------------------------------------|----------|
| `                 store.jdbc.dsName                ` | The name of the datasource to be looked up | YES      |
| `                 store.jdbc.table                `  | The table name of the database.            | YES      | 

### Paramaters (Carbon Datasource)

To make sure that the datasource appears in the **Datasource Name** list, you need to expose it as a JNDI datasource.

Following are descriptions of the fields that are displayed:

| Field           | Description                       |
|-----------------|-----------------------------------|
| Name            | The name for the message store    |
| Database Table  | The name of the database table.   |
| Datasource Name | The class name of the datasource. |


## Properties: Resequence Message Store

The following is a sample ESB configuration of the Resequence Message Store.

```
<?xml version="1.0" encoding="UTF-8"?>
    <messageStore xmlns="http://ws.apache.org/ns/synapse"
                  class="org.apache.synapse.message.store.impl.resequencer.ResequenceMessageStore"
                  name="RStore">
       <parameter name="store.resequence.timeout">-1</parameter>
       <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
       <parameter name="store.failover.message.store.name">RStore</parameter>
       <parameter xmlns:m0="http://services.samples"
                  name="store.resequence.id.path"
                  expression="substring-after(//m0:placeOrder/m0:order/m0:symbol,'-')"/>
       <parameter name="store.jdbc.password">root</parameter>
       <parameter name="store.jdbc.driver">com.mysql.jdbc.Driver</parameter>
       <parameter name="store.jdbc.username">root</parameter>
       <parameter name="store.jdbc.connection.url">jdbc:mysql://localhost:3306/resequenceDB</parameter>
       <parameter name="store.jdbc.table">tbl_resequence</parameter>
</messageStore>
```

Listed below are the properties that are required for the Resequence Message Store.

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
    <td>Connection Information</td>
    <td>..</td>
  </tr>
  <tr>
    <td>RDBMS Type</td>
    <td>
      ....
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

The following are the additional properties related to the Resequence Message Store.

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

Listed below are the properties that are required for the Custom Message Store.

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

## Properties: WSO2 MB Message Store

Listed below are the **required** parameters for configuring the WSO2 MB
Message Store.

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Message Store Name</td>
<td>Give a unique name for the JMS message store.</td>
</tr>
<tr class="even">
<td>Message Store Type</td>
<td>Select <strong>WSO2 MB Message Store</strong> from the list of options.</td>
</tr>
<tr class="odd">
<td>Initial Context Factory</td>
<td>This specifies the JNDI initial context factory class ( <code>             java.naming.factory.initial            </code> ). This class implements the implement the <code>             java.naming.spi.InitialContextFactory            </code> interface. The value is set to <code>             org.wso2.andes.jndi.PropertiesFileInitialContextFactory            </code> , by default.</td>
</tr>
<tr class="even">
<td>Queue Connection Factory</td>
<td><div class="content-wrapper">
<p>This is the connection factory URlfor connecting to WSO2 MB. By default, the value is set to <code>               amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5673'              </code> .</p>
!!! note
<p>Important!</p>
<p>Be sure to change the port to 5675.</p>

</div></td>
</tr>
<tr class="odd">
<td>JNDI Queue Name</td>
<td>The name of the queue in the broker that will store messages.</td>
</tr>
<tr class="even">
<td>JMS API Specification Version</td>
<td>The JMS API version to be used. Possible values are 1.1 or 1.0. The value is set to 1.1, by default.</td>
</tr>
</tbody>
</table>

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