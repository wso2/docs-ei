# Detecting Repeatedly Redelivered Messages Using the JMSXDeliveryCount Property

In JMS 2.0, it is mandatory for JMS providers to set the
`         JMSXDeliveryCount        ` property, which allows an
application that receive a message to determine how many times the
message is redelivered.

If a message is being redelivered, it means that a previous attempt to
deliver the message failed due to some reason. If a message is being
redelivered multiple times, it can be because the message is *bad* in
some way. When such a message is being redelivered over and over again,
it wastes resources and prevents subsequent *good* messages from being
processed.

When you work with the ESB Profile of WSO2 Enterprise Integrator (WSO2
EI), you can detect such repeatedly redelivered messages using the
`         JMSXDeliveryCount        ` property that is set in messages.

Being able to detect repeatedly redelivered messages is particularly
useful because you can take necessary steps to handle such messages in a
proper manner. For example, you can consume such a message and send it
to a separate queue.

The following diagram illustrates how the ESB Profile of WSO2 EI can be
used to detect repeatedly redelivered messages, and store such messages
in an internal message store.

![](attachments/119130322/119130323.png){width="500"}

To demonstrate the scenario illustrated above, let's configure the JMS
inbound endpoint in the ESB Profile of WSO2 EI using HornetQ as the
message broker. This sample scenario includes the following sections:

-   [Prerequisites](#DetectingRepeatedlyRedeliveredMessagesUsingtheJMSXDeliveryCountProperty-Prerequisites)
-   [Configuring and starting the HornetQ message
    broker](#DetectingRepeatedlyRedeliveredMessagesUsingtheJMSXDeliveryCountProperty-ConfiguringandstartingtheHornetQmessagebroker)
-   [Building the sample
    scenario](#DetectingRepeatedlyRedeliveredMessagesUsingtheJMSXDeliveryCountProperty-Buildingthesamplescenario)
-   [Executing the sample
    scenario](#DetectingRepeatedlyRedeliveredMessagesUsingtheJMSXDeliveryCountProperty-Executingthesamplescenario)
-   [Analyzing the
    output](#DetectingRepeatedlyRedeliveredMessagesUsingtheJMSXDeliveryCountProperty-Analyzingtheoutput)

### Prerequisites

-   Download the
    [hornetq-all-new.jar](https://drive.google.com/file/d/0BzqPLt3PumqXNi1vM1hodFc1NWs/view)
    and copy it to the `          <EI_HOME>/lib/         ` directory.
-   Replace the
    `          geronimo-jms_1.1_spec-1.1.0.wso2v1.jar         ` file
    in the `          <EI_HOME>/wso2/lib/endorsed/         `
    directory with
    `                     javax.jms-api-2.0.1.jar                   `
-   Download HornetQ from <http://hornetq.jboss.org/downloads> and
    extract the tar.gz.

### Configuring and starting the HornetQ message broker

1.  Open the
    `           <HornetQ_HOME>/config/stand-alone/non-clustered/hornetq-jms.xml          `
    file in a text editor and add the following configuration:

    ``` xml
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
    
        <queue name="wso2">
              <entry name="/queue/mySampleQueue"/>
        </queue>
    
        <topic name="sampleTopic">
              <entry name="/topic/exampleTopic"/>
        </topic>
    ```

2.  Start the HornetQ message broker
    -   To start the message broker on Linux, navigate to the
        `            <HornetQ_HOME>/bin/           ` directory and
        execute the `            run.sh           ` command with
        root privileges.

### Building the sample scenario

The XML configuration for this sample scenario is as follows:

``` xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager">
            <parameter name="cachableDuration">15000</parameter>
        </taskManager>
         <sequence name="request" onError="fault">
            <log level="full"/>
            <filter regex="1"
                source="get-property('default','jms.message.delivery.count')" xmlns:ns="http://org.apache.synapse/xsd">
                <then>
                    <log>
                        <property name="DeliveryCounter" value="1"/>
                    </log>
                </then>
                <else>
                    <store messageStore="JMS-Redelivered-Store"/>
                    <log>
                        <property name="DeliveryCounter" value="more than 1"/>
                    </log>
                </else>
            </filter>
            <drop/>
        </sequence>
        <sequence name="fault">
            <log level="full">
                <property name="MESSAGE" value="Executing default &quot;fault&quot; sequence"/>
                <property expression="get-property('ERROR_CODE')" name="ERROR_CODE"/>
                <property expression="get-property('ERROR_MESSAGE')" name="ERROR_MESSAGE"/>
            </log>
            <drop/>
        </sequence>
        <sequence name="main">
            <log level="full"/>
            <drop/>
        </sequence>
        <messageStore name="JMS-Redelivered-Store"/>
        <inboundEndpoint name="jms_inbound" onError="fault" protocol="jms"
            sequence="request" suspend="false">
            <parameters>
                <parameter name="interval">1000</parameter>
                <parameter name="transport.jms.Destination">queue/mySampleQueue</parameter>
                <parameter name="transport.jms.CacheLevel">1</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
                <parameter name="sequential">true</parameter>
                <parameter name="java.naming.factory.initial">org.jnp.interfaces.NamingContextFactory</parameter>
                <parameter name="java.naming.provider.url">jnp://localhost:1099</parameter>
                <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
                <parameter name="transport.jms.SessionTransacted">false</parameter>
                <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
            </parameters>
        </inboundEndpoint>
    </definitions>
```

This configuration creates an inbound endpoint to the JMS broker and has
a simple sequence that logs the message status using the
`         JMSXDeliveryCount        ` value.

To build the sample

-   Start the ESB Profile of WSO2 EI with the sample configuration.

### Executing the sample scenario

-   Run the following java file to publish a message to the JMS queue:

    **SOAPPublisher.java**

    ``` java
            package JMSXDeliveryCount;
        
            import java.util.Properties;
            import java.util.logging.Logger;
        
            import javax.jms.ConnectionFactory;
            import javax.jms.Destination;
            import javax.jms.JMSContext;
            import javax.naming.Context;
            import javax.naming.InitialContext;
            import javax.naming.NamingException;
        
            public class SOAPPublisher {
                private static final Logger log = Logger.getLogger(SOAPPublisher.class.getName());
        
                // Set up all the default values
                private static final String param = "IBM";
        
                // with header for inbounds
                private static final String MESSAGE_WITH_HEADER =
                        "<soapenv:Envelope xmlns:soapenv=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                                "   <soapenv:Header/>\n" +
                                "<soapenv:Body>\n" +
                                "<m:placeOrder xmlns:m=\"http://services.samples\">\n" +
                                "    <m:order>\n" +
                                "        <m:price>" +
                                getRandom(100, 0.9, true) +
                                "</m:price>\n" +
                                "        <m:quantity>" +
                                (int) getRandom(10000, 1.0, true) +
                                "</m:quantity>\n" +
                                "        <m:symbol>" +
                                param +
                                "</m:symbol>\n" +
                                "    </m:order>\n" +
                                "</m:placeOrder>" +
                                "   </soapenv:Body>\n" +
                                "</soapenv:Envelope>";
                private static final String DEFAULT_CONNECTION_FACTORY = "QueueConnectionFactory";
                private static final String DEFAULT_DESTINATION = "queue/mySampleQueue";
                private static final String INITIAL_CONTEXT_FACTORY = "org.jnp.interfaces.NamingContextFactory";
                private static final String PROVIDER_URL = "jnp://localhost:1099";
        
                public static void main(String[] args) {
        
                    Context namingContext = null;
        
                    try {
        
                        // Set up the namingContext for the JNDI lookup
                        final Properties env = new Properties();
                        env.put(Context.INITIAL_CONTEXT_FACTORY, INITIAL_CONTEXT_FACTORY);
                        env.put(Context.PROVIDER_URL, System.getProperty(Context.PROVIDER_URL, PROVIDER_URL));
                        namingContext = new InitialContext(env);
        
                        // Perform the JNDI lookups
                        String connectionFactoryString =
                                System.getProperty("connection.factory",
                                                   DEFAULT_CONNECTION_FACTORY);
                        log.info("Attempting to acquire connection factory \"" + connectionFactoryString + "\"");
                        ConnectionFactory connectionFactory =
                                (ConnectionFactory) namingContext.lookup(connectionFactoryString);
                        log.info("Found connection factory \"" + connectionFactoryString + "\" in JNDI");
        
                        String destinationString = System.getProperty("destination", DEFAULT_DESTINATION);
                        log.info("Attempting to acquire destination \"" + destinationString + "\"");
                        Destination destination = (Destination) namingContext.lookup(destinationString);
                        log.info("Found destination \"" + destinationString + "\" in JNDI");
        
                        // String content = System.getProperty("message.content",
                        // DEFAULT_MESSAGE);
                        String content = System.getProperty("message.content", MESSAGE_WITH_HEADER);
        
                        try (JMSContext context = connectionFactory.createContext()) {
                            log.info("Sending  message");
                            // Send the message
                            context.createProducer().send(destination, content);
                        }
        
                    } catch (NamingException e) {
                        log.severe(e.getMessage());
                    } finally {
                        if (namingContext != null) {
                            try {
                                namingContext.close();
                            } catch (NamingException e) {
                                log.severe(e.getMessage());
                            }
                        }
                    }
                }
        
        
                private static double getRandom(double base, double varience, boolean onlypositive) {
                    double rand = Math.random();
                    return (base + (rand > 0.5 ? 1 : -1) * varience * base * rand) *
                            (onlypositive ? 1 : rand > 0.5 ? 1 : -1);
                }
            }
    ```

### Analyzing the output

When you analyze the output on the ESB console, you will see an entry
similar to the following:

``` java
    INFO - LogMediator To: , MessageID: ID:60868ca5-d174-11e5-b7de-f9743c9bcc9e, Direction: request, DeliveryCounter = 1
```
