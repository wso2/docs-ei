# JMS Message Store

The JMS message store persists messages in a JMS queue inside a JMS
Broker. The JMS message store can be configured by specifying the class
as `         org.apache.synapse.message.store.impl.jms.JmsStore        `
. Since the JMS message stores persist messages in a JMS queue in an
ordered manner, JMS message stores can be used to implement the
store-and-forward pattern.

-   [Required parameters](#JMSMessageStore-Requiredparameters)
-   [Additional JMS message store
    parameters](#JMSMessageStore-JMSParaAdditionalJMSmessagestoreparameters)
-   [Example JMS message store
    configurations](#JMSMessageStore-ExampleJMSmessagestoreconfigurations)

### Required parameters

When you add a JMS message store, it is required to specify values for
the following:

-   **Name** - A unique name for the JMS message store.
-   **Initial Context Factory** (
    `          java.naming.factory.initial         ` ) - The JNDI
    initial context factory class. This class must implement the
    `          java.naming.spi.InitialContextFactory         `
    interface. Initial Context Factory used to connect to the JMS
    broker.
-   **Provider URL** ( `          java.naming.provider.url         ` )
    - The URL of the JNDI provider. Url of the naming provider to be
    used by the context factory.

### Additional JMS message store parameters

| Parameter Name                                                                        | Value                                                                                  | Required                                                                           |
|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| JNDI Queue Name ( `             store.jms.destination            ` )                  | The message store queue name.                                                          | Though this is not a required parameter, we recommend specifying a value for this. |
| Connection factory ( `             store.jms.connection.factory            ` )        | The JNDI name of the connection factory that is used to create jms connections         | Though this is not a required parameter, we recommend specifying a value for this. |
| User Name ( `             store.jms.username            ` )                           | The user name to connect to the broker.                                                | No                                                                                 |
| Password ( `             store.jms.password            ` )                            | The password to connect to the broker.                                                 | No                                                                                 |
| JMS API Specification Version ( `             store.jms.JMSSpecVersion            ` ) | The JMS API version to be used. Possible values are 1.1 or 1.0.                        | No. The default value is 1.1.                                                      |
| vender.class.loader.enabled                                                           | Set to **false** when using IBM MQ, which requires skipping the external class loader. | No, except when using IBM MQ                                                       |
| store.jms.cache.connection                                                            | true/false Enable Connection caching                                                   | No                                                                                 |

If you need to ensure guaranteed delivery when you store incoming
messages to a JMS message store, and later deliver them to a particular
backend, click **Show Guaranteed Delivery Parameters** and specify
values for the following parameters:

-   **Enable Producer Guaranteed Delivery** (
    `          store.producer.guaranteed.delivery.enable         ` ) -
    Whether it is required to enable guaranteed delivery on the producer
    side.
-   **Failover Message Store** (
    `                     store.failover.message.store.name                   `
    ) - The message store to which the store mediator should send
    messages when the original message store fails.

### Example JMS message store configurations

Following is a sample JMS message store configuration that uses the
Message Broker profile of WSO2 EI as the message broker:

``` xml
    <messageStore name="JMSMS" class="org.apache.synapse.message.store.impl.jms.JmsStore" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
       <parameter name="java.naming.provider.url">/conf/jndi.properties</parameter>
       <parameter name="store.jms.destination">ordersQueue</parameter>
       <parameter name="store.jms.connection.factory">QueueConnectionFactory</parameter>
       <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
    </messageStore>
```

!!! info

When you use the Message Broker profile as the message broker and
configure a [Message Processor](_Message_Processors_) with
`         max.delivery.attempts        ` , if the Message Broker profile
does not get an acknowledgment from the Integration profile, it re-sends
the message that results in duplicate messages being delivered. To avoid
this, add the following line in the
`         <EI_HOME>/bin/integrator.sh        ` file:  
  
`         -DAndesAckWaitTimeOut=3600000 \        `


Following is a sample JMS message store configuration that uses ActiveMQ
as the message broker:

``` xml
    <messageStore name="JMSMS" class="org.apache.synapse.message.store.impl.jms.JmsStore" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
       <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
       <parameter name="store.jms.destination">ordersQueue</parameter>
       <parameter name="store.jms.connection.factory">QueueConnectionFactory</parameter>
       <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
    </messageStore>
```

!!! info

Note

When you configure a JMS message store with the Message Broker profile
or ActiveMQ, you need to copy the required client libraries to the
`         <EI_HOME>/lib        ` directory. If the relevant client
libraries are not copied, you will see a
`         java.lang.ClassNotFoundException        ` .

For information on the client libraries you need to copy when
configuring a JMS message store with the Message Broker profile, see
[Configure with the Message Broker
profile](https://docs.wso2.com/display/EI650/Configure+with+the+Broker+Profile#ConfigurewiththeBrokerProfile-clientLibs)
.

For information on the client libraries you need to copy when
configuring a JMS message store with ActiveMQ, see [Configure with
ActiveMQ](https://docs.wso2.com/display/EI650/Configure+with+ActiveMQ#ConfigurewithActiveMQ-clientLibs)
.


Individual message priorities can be set using the following property on
the provider. For example, the value can be 0-9 for ActiveMQ.

`         <property name="JMS_PRIORITY" value="9" scope="axis2"/>        `

!!! note

Note

If you are using ActiveMQ 5.12.2 and above, you need to set the
following system property on server start up for WSO2 EI's JMS message
store to work as expected.

``` bash
    -Dorg.apache.activemq.SERIALIZABLE_PACKAGES=“*"
```
    
    With ActiveMQ 5.12.2 and above, y ou need to set the above property
    because users are enforced to explicitly whitelist packages that can be
    exchanged using ObjectMessages, and due to this restriction the message
    processor fails to read messages from ActiveMQ with the following error:
    
``` bash
    ERROR - JmsConsumer [JMS-C-1] cannot receive message from store. Error:Failed to build body from content. Serializable class not available to broker. Reason: java.lang.ClassNotFoundException: Forbidden class org.apache.synapse.message.store.impl.commons.StorableMessage! This class is not trusted to be serialized as ObjectMessage payload.
```
    

For information on configuring the JMS message store with different
message brokers, see [Store and Forward Using JMS Message
Stores](https://docs.wso2.com/display/EI650/Store+and+Forward+Using+JMS+Message+Stores)
.
