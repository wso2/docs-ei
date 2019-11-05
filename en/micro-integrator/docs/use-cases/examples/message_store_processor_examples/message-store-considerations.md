# Using Message Stores - Special Considerations

!!! Info
    When you use the Message Broker profile as the message broker and configure a [Message Processor](_Message_Processors_) with `         max.delivery.attempts        ` , if the Message Broker profile does not get an acknowledgment from the Integration profile, it re-sends the message that results in duplicate messages being delivered. To avoid this, add the following line in the `         <MI_HOME>/bin/micro-integrator.sh        ` file: `         -DAndesAckWaitTimeOut=3600000 \` Individual message priorities can be set using the following property on the provider. For example, the value can be 0-9 for ActiveMQ.
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