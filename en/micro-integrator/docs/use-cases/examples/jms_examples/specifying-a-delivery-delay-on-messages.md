# Using the ESB Profile of WSO2 Enterprise Integrator as a JMS Producer and Specifying a Delivery Delay on Messages

In a normal message flow, JMS messages that are sent by the JMS producer
to the JMS broker are forwarded to the respective JMS consumer without
any delay.

With the delivery delay messaging feature introduced with JMS 2.0, you
can specify a delivery delay time value in each JMS message so that the
JMS broker will not deliver the message until after the specified
delivery delay has elapsed. Specifying a delivery delay is useful if
there is a scenario where you do not want a message consumer to receive
a message that is sent until a specified time duration has elapsed. To
implement this, you need to add a delivery delay to the JMS producer so
that the publisher does not deliver a message until the specified
delivery delay time interval is elapsed.

The following diagram illustrates how you can use the ESB Profile of
WSO2 Enterprise Integrator (WSO2 EI) as a JMS producer and specify a
delivery delay on messages when you do not want the message consumer to
receive a message until a specified time duration has elapsed.

![](attachments/119130324/119130325.png)

To demonstrate the scenario illustrated above, let's configure the JMS
transport sender in the ESB Profile of WSO2 EI using HornetQ as the
message broker. This sample scenario includes the following sections:

-   [Prerequisites](#UsingtheESBProfileofWSO2EnterpriseIntegratorasaJMSProducerandSpecifyingaDeliveryDelayonMessages-Prerequisites)
-   [Configuring and starting the HornetQ message
    broker](#UsingtheESBProfileofWSO2EnterpriseIntegratorasaJMSProducerandSpecifyingaDeliveryDelayonMessages-ConfiguringandstartingtheHornetQmessagebroker)
-   [Configuring the JMS transport sender in the ESB Profile of WSO2
    EI](#UsingtheESBProfileofWSO2EnterpriseIntegratorasaJMSProducerandSpecifyingaDeliveryDelayonMessages-ConfiguringtheJMStransportsenderintheESBProfileofWSO2EI)
-   [Building the sample
    scenario](#UsingtheESBProfileofWSO2EnterpriseIntegratorasaJMSProducerandSpecifyingaDeliveryDelayonMessages-Buildingthesamplescenario)
-   [Executing the sample
    scenario](#UsingtheESBProfileofWSO2EnterpriseIntegratorasaJMSProducerandSpecifyingaDeliveryDelayonMessages-Executingthesamplescenario)
-   [Analyzing the
    output](#UsingtheESBProfileofWSO2EnterpriseIntegratorasaJMSProducerandSpecifyingaDeliveryDelayonMessages-Analyzingtheoutput)

### Prerequisites

-   Download the
    [hornetq-all-new.jar](https://drive.google.com/file/d/0BzqPLt3PumqXNi1vM1hodFc1NWs/view)
    and copy it to the `          <EI_HOME>/lib         ` directory.
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

### Configuring the JMS transport sender in the ESB Profile of WSO2 EI

-   To enable the JMS transport sender, un-comment the JMS transport
    sender configuration in the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file and
    update the sender configuration so that it is as follows:

    ``` xml
            <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender">
                <parameter name="myQueueConnectionFactory" locked="false">
                      <parameter name="java.naming.factory.initial" locked = "false">org.jnp.interfaces.NamingContextFactory</parameter>
                      <parameter name="java.naming.provider.url" locked="false">jnp://localhost:1099</parameter>
                      <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                      <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                      <parameter name="transport.jms.Destination">queue/mySampleQueue</parameter>
                      <parameter name="transport.jms.JMSSpecVersion">2.0</parameter>
                </parameter>
        
                <parameter name="default" locked="false">
                      <parameter name="java.naming.factory.initial" locked = "false">org.jnp.interfaces.NamingContextFactory</parameter>
                      <parameter name="java.naming.provider.url" locked="false">jnp://localhost:1099</parameter>
                      <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                      <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                      <parameter name="transport.jms.Destination">queue/mySampleQueue</parameter>
                      <parameter name="transport.jms.JMSSpecVersion">2.0</parameter>
                </parameter>
            </transportSender>
    ```

### Building the sample scenario

The XML configuration for this sample scenario is as follows:

``` xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
        <proxy name="JMSDelivery" startOnLoad="true" trace="disable" transports="https http">
            <description/>
            <target>
                <inSequence>
                    <property name="OUT_ONLY" value="true"/>
                    <property name="FORCE_SC_ACCEPTED" scope="axis2" value="true"/>
                    <property action="remove" name="Content-Length" scope="transport"/>
                    <property action="remove" name="MIME-Version" scope="transport"/>
                    <property action="remove" name="Transfer-Encoding" scope="transport"/>
                    <property action="remove" name="User-Agent" scope="transport"/>
                    <property action="remove" name="Accept-Encoding" scope="transport"/>
                    <property name="messageType" scope="axis2" value="application/xml"/>
                    <property action="remove" name="Content-Type" scope="transport"/>
                    <log level="full"/>
                    <send>
                        <endpoint>
                            <address uri="jms:transport.jms.ConnectionFactory=myQueueConnectionFactory"/>
                        </endpoint>
                    </send>
                </inSequence>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
        <proxy name="JMSDeliveryDelayed" startOnLoad="true" trace="disable" transports="https http">
            <description/>
            <target>
                <inSequence>
                    <property name="OUT_ONLY" value="true"/>
                    <property name="FORCE_SC_ACCEPTED" scope="axis2" value="true"/>
                    <property action="remove" name="Content-Length" scope="transport"/>
                    <property action="remove" name="MIME-Version" scope="transport"/>
                    <property action="remove" name="Transfer-Encoding" scope="transport"/>
                    <property action="remove" name="User-Agent" scope="transport"/>
                    <property action="remove" name="Accept-Encoding" scope="transport"/>
                    <property name="messageType" scope="axis2" value="application/xml"/>
                    <property action="remove" name="Content-Type" scope="transport"/>
                    <property name="JMS_MESSAGE_DELAY" scope="axis2" value="10000"/>
                    <log level="full"/>
                    <send>
                        <endpoint>
                            <address uri="jms:/transport.jms.ConnectionFactory=myQueueConnectionFactory"/>
                        </endpoint>
                    </send>
                </inSequence>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
        <sequence name="fault">
            <!-- Log the message at the full log level with the ERROR_MESSAGE and the ERROR_CODE-->
            <log level="full">
                <property name="MESSAGE" value="Executing default 'fault' sequence"/>
                <property expression="get-property('ERROR_CODE')" name="ERROR_CODE"/>
                <property expression="get-property('ERROR_MESSAGE')" name="ERROR_MESSAGE"/>
            </log>
            <!-- Drops the messages by default if there is a fault -->
            <drop/>
        </sequence>
        <sequence name="main">
            <in>
                <!-- Log all messages passing through -->
                <log level="full"/>
                <!-- ensure that the default configuration only sends if it is one of samples -->
                <!-- Otherwise Synapse would be an open proxy by default (BAD!)               -->
                <filter regex="http://localhost:9000.*" source="get-property('To')">
                    <!-- Send the messages where they have been sent (i.e. implicit "To" EPR) -->
                    <send/>
                </filter>
            </in>
            <out>
                <send/>
            </out>
            <description>The main sequence for the message mediation</description>
        </sequence>
        <!-- You can add any flat sequences, endpoints, etc.. to this synapse.xml file if you do
        *not* want to keep the artifacts in several files -->
    </definitions>
```

This configuration creates two proxy services
`         JMSDeliveryDelayed        ` and `         JMSDelivery        `
. The `         JMSDeliveryDelayed        ` proxy service sets a
delivery delay of 10 seconds on the message that it forwards, whereas
the `         JMSDelivery        ` proxy service does not set a delivery
delay on the message.

To build the sample

-   Start the ESB Profile of WSO2 EI with the sample configuration.

### Executing the sample scenario

-   Run the following java file, which acts as the JMS consumer that
    consumes messages from the queue:

    **QueueConsumer.java**

    ``` java
            package DeliveryDelay;
        
            import java.sql.Timestamp;
            import java.util.Date;
            import java.util.Properties;
        
            import javax.jms.Connection;
            import javax.jms.ConnectionFactory;
            import javax.jms.MessageConsumer;
            import javax.jms.Session;
            import javax.jms.TextMessage;
            import javax.jms.Queue;
            import javax.naming.Context;
            import javax.naming.InitialContext;
        
            /**
             * Sample consumer to demonstrate JMS 2.0 feature :
             * Message Delivery Delay
             * Classic API is used
             */
        
            public class QueueConsumer {
            private static final String DEFAULT_CONNECTION_FACTORY = "QueueConnectionFactory";
            private static final String DEFAULT_DESTINATION = "queue/mySampleQueue";
            private static final String INITIAL_CONTEXT_FACTORY = "org.jnp.interfaces.NamingContextFactory";
            private static final String PROVIDER_URL = "jnp://localhost:1099";
        
            public static void main(final String[] args) {
            try {
                        runExample();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
        
            public static void runExample() throws Exception {
            Connection connection = null;
            Context initialContext = null;
            try {
        
            // Step 1. Create an initial context to perform the JNDI lookup.
            final Properties env = new Properties();
                        env.put(Context.INITIAL_CONTEXT_FACTORY, INITIAL_CONTEXT_FACTORY);
                        env.put(Context.PROVIDER_URL, System.getProperty(Context.PROVIDER_URL, PROVIDER_URL));
                        initialContext = new InitialContext(env);
        
        
            // Step 2. perform a lookup on the Queue
            Queue queue = (Queue) initialContext.lookup(DEFAULT_DESTINATION);
        
        
            // Step 3. perform a lookup on the Connection Factory
            ConnectionFactory cf = (ConnectionFactory) initialContext.lookup(DEFAULT_CONNECTION_FACTORY);
        
        
            // Step 4. Create a JMS Connection
                        connection = cf.createConnection();
        
        
            // Step 5. Create a JMS Session
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        
            // Step 6. Create a JMS Message Consumer
            MessageConsumer messageConsumer = session.createConsumer(queue);
        
            // Step 7. Start the Connection
                        connection.start();
            System.out.println("JMS consumer stated on the queue " + DEFAULT_DESTINATION +
            "\n");
        
            //Clear the queue, if there is any previous messages in the queue
            TextMessage tempMessage;
            do{
                            tempMessage = (TextMessage) messageConsumer.receive(1); 
                        } while(tempMessage != null);
        
            // Step 8.1. Receive the message one
            TextMessage firstMessage = (TextMessage) messageConsumer.receive(); 
            long first = System.currentTimeMillis();
            System.out.println("Consumer received message: [ "+new Timestamp(new Date(first).getTime())+" ] " + firstMessage.getText() + "\n");
        
            // Step 8.2. Receive delayed
            TextMessage secondMessage = (TextMessage) messageConsumer.receive();    
            long second = System.currentTimeMillis();
            System.out.println("Consumer received dealyed message: [ "+new Timestamp(new Date(second).getTime())+" ] " + secondMessage.getText() + "\n");
            System.out.println("Time difference between two messages : "+(second-first)/1000+"s");
                    } finally {
        
            // Step 9. Close JMS resources
            if (connection != null) {
                            connection.close();
                        }
        
            // Also the initialContext
            if (initialContext != null) {
                            initialContext.close();
                        }
                    }
                }
            }
    ```

-   Run the following command from
    `           <EI_HOME>/sample/axis2Client          ` to invoke the
    two proxy services:

    ``` java
            ant stockquote -Daddurl=http://localhost:8280/services/JMSDelivery -Dmode=placeorder -Dsymbol=MSFT && ant stockquote -Daddurl=http://localhost:8280/services/JMSDeliveryDelayed -Dmode=placeorder -Dsymbol=MSFT
    ```

### Analyzing the output

You will see that two messages are received by the Java consumer with a
time difference of more than 10 seconds.

This is because the `         JMSDeliveryDelayed        ` proxy service
sets a delivery delay of 10 seconds on the message that it forwards,
whereas the `         JMSDelivery        ` proxy service does not set a
delivery delay on the message.

  
