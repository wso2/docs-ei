# Connecting to Tibco EMS

This section describes how to configure WSO2 Micro Integrator to connect with Tibco EMS.

1. Download and set up Tibco EMS in your environment.
2. Download and install WSO2 Micro Integrator.
3. Copy the Tibco EMS client JAR files that are shipped with the distribution to the `MI_HOME/extensions` directory.

    -   tibcrypt.jar or jms-2.0.jar (If Tibco EMS 8.4 version is used,
        be sure to use jms-2.0.jar)
    -   tibjms.jar
    -   tibjmsadmin.jar
    -   tibjmsapps.jar
    -   tibrvjms.jar
    
3. If you want the Micro Integrator to receive messages from an instance of Tibco EMS, or to send messages to an instance of Tibco EMS, you need to update the deployment.toml file with the relevant connection parameters.

    - Add the following configurations to enable the JMS listener with Tibco connection parameters.
        ```toml
        [[transport.jms.listener]]
        name = "myTopicListener"
        parameter.initial_naming_factory = "com.tibco.tibjms.naming.TibjmsInitialContextFactory"
        parameter.broker_name = "activemq" 
        parameter.provider_url = "tcp://127.0.0.1:37222"
        parameter.connection_factory_name = "TopicConnectionFactory"
        parameter.connection_factory_type = "topic"
        parameter.cache_level = "consumer"
        ```

    - Add the following configurations to enable the JMS sender with Tibco connection parameters.
        ```toml
        [[transport.jms.sender]]
        name = "myTopicSender"
        parameter.initial_naming_factory = "com.tibco.tibjms.naming.TibjmsInitialContextFactory"
        parameter.broker_name = "activemq"
        parameter.provider_url = "tcp://127.0.0.1:37222"
        parameter.connection_factory_name = "TopicConnectionFactory"
        parameter.connection_factory_type = "topic"
        parameter.cache_level = "producer"
        ```