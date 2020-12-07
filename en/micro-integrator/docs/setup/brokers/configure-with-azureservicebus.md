# Connecting to Azure Service Bus

This section describes how to configure WSO2 Micro Integrator to connect with Azure Service Bus.

The integration between this message broker and WSO2 Micro Integrator is straightforward. The guaranteed delivery integration pattern must be used to implement the mediation flow.

## Setting up the Micro Integrator with Azure Service Bus

Follow the instructions below to set up and configure.

1.  Download [Azure Service Bus](https://azure.microsoft.com/en-us/services/service-bus/).
2.  Download and install WSO2 Micro Integrator.

### Setting up Azure Service Bus

### Setting up the Micro Integrator to work with Azure Service Bus

1. Copy the following external .jar files to the `MI_HOME/lib` directory.
    - [geronimo-jms_2.0_spec-1.0-alpha-2.jar](https://mvnrepository.com/artifact/org.apache.geronimo.specs/geronimo-jms_2.0_spec/1.0-alpha-2)
    - [netty-transport-native-epoll-4.0.40.Final.jar](https://mvnrepository.com/artifact/io.netty/netty-transport-native-epoll/4.0.40.Final)
    - [proton-j-0.27.1.jar](https://mvnrepositor`y.com/artifact/org.apache.qpid/proton-j/0.27.1)
    - [Qpid-jms-client-0.32.0.jar](https://mvnrepository.com/artifact/org.apache.qpid/qpid-jms-client/0.32.0)

2. If you want the Micro Integrator to receive messages from an Azure Service Bus instance, or to send messages to an Azure Service Bus instance, you need to update the deployment.toml file with the relevant connection parameters.

     - Add the following configurations to enable the JMS listener with Azure Service Bus connection parameters.
     
        ```toml
        [[transport.jms.listener]]
        name = "azureQueueConsumerConnectionFactory"
        parameter.initial_naming_factory = "org.apache.qpid.jms.jndi.JmsInitialContextFactory"
        parameter.provider_url = "conf/jndi.properties"
        parameter.connection_factory_name = "SBCF"
        parameter.connection_factory_type = "queue"
        ```

    - Add the following configurations to enable the JMS sender with Azure Service Bus connection parameters.
        
        ```toml
        [[transport.jms.sender]]
        name = "azureQueueProducerConnectionFactory"
        parameter.initial_naming_factory = "org.apache.qpid.jms.jndi.JmsInitialContextFactory"
        parameter.provider_url = "conf/jndi.properties"
        parameter.connection_factory_name = "SBCF"
        parameter.connection_factory_type = "queue"
        ```
3.  Remove any existing Apache ActiveMQ client JAR files from the `MI_HOME/dropins/` and `MI_HOME/lib/` directories.  
4.  Remove the below line from the `MI_HOME/conf/etc/launch.ini` file.  

    ```text
    javax.jms,\
    ```
5.  Start Azure Service Bus. For instructions, see the [Azure Service Bus Documentation](https://docs.microsoft.com/en-us/azure/service-bus-messaging/).
6.  Start the Micro Integrator.

Now you have configured instances of Azure Service Bus and WSO2 Micro Integrator.

