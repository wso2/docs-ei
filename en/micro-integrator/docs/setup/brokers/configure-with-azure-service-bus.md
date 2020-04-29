# Connecting to Azure Service Bus

This section describes how to configure WSO2 Micro Integrator to connect with RabbitMQ.

## Copy the JAR files

Copy the following jars to <MI_HOME>/lib folder.

- 	qpid-jms-client-0.32.0.jar
-	geronimo-jms_2.0_spec-1.0-alpha-2.jar
-	proton-j-0.27.1.jar
-	netty-transport-native-epoll-4.0.40.Final.jar / netty-all-4.1.24.Final.jar

## Setting up the Micro Integrator

### JMS Listener
  
If you want the Micro Integrator to receive messages from an ASB instance, or to send messages to an ASB instance, you need to update the deployment.toml file with the relevant connection parameters.

- Add the following configurations to enable the JMS listener with ASB connection parameters.
    ```toml
    [[transport.jms.listener]]
    name = "myQueueListener"
    parameter.initial_naming_factory = "org.apache.qpid.jms.jndi.JmsInitialContextFactory"
    parameter.provider_url = "conf/jndi.properties"
    parameter.connection_factory_name = "QueueConnectionFactory"
    parameter.connection_factory_type = "queue"
    parameter.cache_level = "consumer"
    ```

- Add the following configurations to enable the JMS sender with ASB connection parameters.
    ```toml
    [[transport.jms.sender]]
    name = "myQueueSender"
    parameter.initial_naming_factory = "org.wso2.andes.jndi.PropertiesFileInitialContextFactory"
    parameter.provider_url = "conf/jndi.properties"
    parameter.connection_factory_name = "QueueConnectionFactory"
    parameter.connection_factory_type = "queue"
    parameter.cache_level = "producer"
    ```

- Add the following configurations to specify the jndi connection factory details:
    ```toml
    [transport.jndi.connection_factories]
    connectionfactory.TopicConnectionFactoryName = "amqps://<namespace>.servicebus.windows.net?amqp.idleTimeout=150000&jms.username=<topic_policy_name>&jms.password=<primary_key_of_topic_level_policy>"
    connectionfactory.QueueConnectionFactoryName = "amqps://<Namespace>.servicebus.windows.net?amqp.idleTimeout=150000&jms.username=<queue_policy_name>&jms.password=<primary_key_of_queue_level_policy >"

    ```

## Update the launch.ini file

Remove the below line from the <EI_HOME>/conf/etc/launch.ini file.

```bash
javax.jms,\
```

## Update Azure primary keys

Make sure you regenerate the primary key in Azure if it consists a + character. You will run into a security issue if you have a + character in your primary key. This is due to bug in AQPID.

## Start the ASB server

## Start the Micro Integrator

Now, you have both WSO2 Message Broker and the Micro Integrator configured and running.
