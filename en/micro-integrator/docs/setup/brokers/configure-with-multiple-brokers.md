# Connectting to Multiple Brokers

If your system uses more than one broker, you need to add multiple broker configurations to the deployment.toml file (stored in the `MI_HOME/conf` directory. In such situations, each transport receiver should have a separate name.

## Configuring multiple broker types

The following example illustrates how to configure WSO2 Micro Integrator to listen to
both ActiveMQ and WSO2 MB messages.

1.  Download ActiveMQ (version 5.8.0 or later) from the [Apache ActiveMQ](http://activemq.apache.org/) site. 
2.  Download the WSO2 Message Broker from the [WSO2 Message Broker](http://wso2.com/products/message-broker/) site.
3.  Copy the following client libraries from `AMQ_HOME/lib` directory to `MI_HOME/lib` directory.  
    -   `            activemq-broker-5.8.0.jar           `
    -   `            activemq-client-5.8.0.jar           `
    -   `            geronimo-jms_1.1_spec-1.1.1.jar           `
    -   `            geronimo-j2ee-management_1.1_spec-1.0.1.jar           `
    -   `            hawtbuf-1.9.jar           `
4.  Add two JMS listener configurations to the deployment.toml file as shown below. Update connection parameters for the ActiveMQ and WSO2 MB brokers respectively.

    ```toml tab='ActiveMQ Broker'
    [[transport.jms.listener]]
    name = "jms1"
    parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
    parameter.broker_name = "activemq" 
    parameter.provider_url = "tcp://localhost:61616"
    parameter.connection_factory_name = "TopicConnectionFactory"
    parameter.connection_factory_type = "topic"
    ```

    ```toml tab='WSO2 Message Broker'
    [[transport.jms.listener]]
    name = "jms2"
    parameter.initial_naming_factory = "org.wso2.andes.jndi.PropertiesFileInitialContextFactory"
    parameter.broker_name = "wso2mb" 
    parameter.provider_url = "conf/jndi.properties"
    parameter.connection_factory_name = "TopicConnectionFactory"
    parameter.connection_factory_type = "topic"
    ```

    !!! Info
        Note that the transport receiver name is different in each configuration.
    
5.  Add a topic and a queue to the jndi.properties file located in `MI_HOME/confx` as follows:  

    ```xml
    <connection-factory name="QueueConnectionFactory">
          <xa>false</xa>
          <connectors>
             <connector-ref connector-name="netty"/>
          </connectors>
          <entries>
             <entry name="/QueueConnectionFactory"/>
          </entries>
    </connection-factory>
    <connection-factory name="TopicConnectionFactory">
          <xa>false</xa>
          <connectors>
             <connector-ref connector-name="netty"/>
          </connectors>
          <entries>
             <entry name="/TopicConnectionFactory"/>
          </entries>
    </connection-factory>
    ```

6.  Start both ActiveMQ and WSO2 MB.
7.  Start WSO2 Micro Integrator.

Now a proxy service can be created with reference to transport receiver JMS1 and/or JMS2. For example, the following proxy service is configured
with reference to both transport receivers.

```xml
<proxy
    xmlns="http://ws.apache.org/ns/synapse" 
    name="jmsMultipleListnerProxy" 
    transports="jms1,jms2" 
    statistics="disable" 
    trace="disable" 
    startOnLoad="true">
    <target>
        <inSequence>
            <log level="full"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <parameter name="transport.jms.ContentType">
        <rules>
            <jmsProperty>contentType</jmsProperty>
            <default>application/xml</default>
        </rules>
    </parameter>
    <description/>
</proxy>
```

## Connecting multiple ActiveMQ brokers

WSO2 Micro Integrator can be configured as described below to work with two ActiveMQ brokers. In this example, port 61616 is used for one
ActiveMQ instance and port 61617 is used for another ActiveMQ instance.

```toml tab='ActiveMQ Broker 1'
[[transport.jms.listener]]
name = "jms1"
parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
parameter.broker_name = "activemq" 
parameter.provider_url = "tcp://localhost:61616"
parameter.connection_factory_name = "TopicConnectionFactory"
parameter.connection_factory_type = "topic"
```

```toml tab='ActiveMQ Broker 2'
[[transport.jms.listener]]
name = "jms2"
parameter.initial_naming_factory = "org.apache.activemq.artemis.jndi.ActiveMQInitialContextFactory"
parameter.broker_name = "activemq" 
parameter.provider_url = "tcp://localhost:61617"
parameter.connection_factory_name = "TopicConnectionFactory"
parameter.connection_factory_type = "topic"
```

!!! Note 
    The name of the transport receivers as well as the ports are different in the two configurations.
