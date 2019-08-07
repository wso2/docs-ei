# JMS Message Store

The JMS message store persists messages in a JMS queue inside a JMS
Broker. The JMS message store can be configured by specifying the class
as `         org.apache.synapse.message.store.impl.jms.JmsStore        `
. Since the JMS message stores persist messages in a JMS queue in an
ordered manner, JMS message stores can be used to implement the
store-and-forward pattern.

## Sample configuration

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

## Special considerations

!!! Info
    When you use the Message Broker profile as the message broker and configure a [Message Processor](_Message_Processors_) with `         max.delivery.attempts        ` , if the Message Broker profile does not get an acknowledgment from the Integration profile, it re-sends the message that results in duplicate messages being delivered. To avoid this, add the following line in the `         <EI_HOME>/bin/integrator.sh        ` file: `         -DAndesAckWaitTimeOut=3600000 \` Individual message priorities can be set using the following property on the provider. For example, the value can be 0-9 for ActiveMQ.
    `<property name="JMS_PRIORITY" value="9" scope="axis2"/>        `

!!! Note
    If you are using ActiveMQ 5.12.2 and above, you need to set the following system property on server start up for WSO2 EI's JMS message store to work as expected.
    ```
    -Dorg.apache.activemq.SERIALIZABLE_PACKAGES=“*"
    ```

!!! Note
    With ActiveMQ 5.12.2 and above, y ou need to set the above property
    because users are enforced to explicitly whitelist packages that can be
    exchanged using ObjectMessages, and due to this restriction the message
    processor fails to read messages from ActiveMQ with the following error:
    ```
    ERROR - JmsConsumer [JMS-C-1] cannot receive message from store. Error:Failed to build body from content. Serializable class not available to broker. Reason: java.lang.ClassNotFoundException: Forbidden class org.apache.synapse.message.store.impl.commons.StorableMessage! This class is not trusted to be serialized as ObjectMessage payload.
    ```
