# Configure with SwiftMQ

This section describes how to configure WSO2 Micro Integrator to connect with SwiftMQ.

1. Download and set up SwiftMQ.
2. Download and install WSO2 Micro Integrator
3. Copy the following client libraries from `SMQ_HOME/lib` directory to `MI_HOME/lib` directory.

    -   jms.jar
    -   jndi.jar
    -   swiftmq.jar

    !!! Info
        Always use the standard client libraries that come with a particular version of SwiftMQ in order to avoid version incompatibility issues. Ww recommend that you remove old client libraries, if any, from all locations including `MI_HOME/lib `and `MI_HOME/droppins` before copying the libraries relevant to a given version.

4.  If you want the Micro Integrator to receive messages from a SwiftMQ instance, or to send messages to a SwiftMQ instance, you need to update the deployment.toml file with the relevant connection parameters.

    - Add the following configurations to enable the JMS listener with SwiftMQ connection parameters.
        ```toml
        [[transport.jms.listener]]
        name = "myTopicListener"
        parameter.initial_naming_factory = "com.swiftmq.jndi.InitialContextFactoryImpl"
        parameter.broker_name = "activemq"
        parameter.provider_url = "smqp://localhost:4001/timeout=10000"
        parameter.connection_factory_name = "TopicConnectionFactory"
        parameter.connection_factory_type = "topic"
        parameter.jms_spec_version = "1.0"
        ```

    - Add the following configurations to enable the JMS sender with SwiftMQ connection parameters.
        ```toml
        [[transport.jms.sender]]
        name = "myTopicSender"
        parameter.initial_naming_factory = "com.swiftmq.jndi.InitialContextFactoryImpl"
        parameter.broker_name = "activemq"
        parameter.provider_url = "smqp://localhost:4001/timeout=10000"
        parameter.connection_factory_name = "TopicConnectionFactory"
        parameter.connection_factory_type = "topic"
        parameter.jms_spec_version = "1.0"
        ```

You have now configured an instance of SwiftMQ and WSO2 Micro Intergrator.
