# Load Balancing with Message Forwarding Processor

## Introduction

This sample demonstrates how you can load balance messages using the
[message store](https://docs.wso2.com/display/EI650/Message+Stores) and
[message forwarding
processor](https://docs.wso2.com/display/EI650/Scheduled+Message+Forwarding+Processor)
.

## Prerequisites

-   Install [ActiveMQ](http://activemq.apache.org) and copy the ActiveMQ
    client JARs `           activemq-core-5.2.0.jar          ` and
    `           geronimo-j2ee-management_1.0_spec-1.0.jar          `
    into the `           <EI_HOME>/lib          ` directory to allow the
    EI to connect to the JMS provider.

-   Start ActiveMQ on its default port.
-   For a list of general prerequisites, see [Prerequisites to start the
    ESB
    samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)

## Building the sample

The XML configuration for this sample is as follows:

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
          <parameter name="cachableDuration">15000</parameter>
       </registry>
        <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
       <proxy name="StockQuoteProxy"
              transports="https http"
              startOnLoad="true">
          <description/>
          <target>
             <inSequence>
                <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
                <property name="OUT_ONLY" value="true"/>
                <store messageStore="JMSMS"/>
             </inSequence>
             <outSequence/>
             <faultSequence/>
          </target>
       </proxy>
       <endpoint name="SimpleStockQuoteService3">
          <address uri="http://localhost:9003/services/SimpleStockQuoteService"/>
       </endpoint>
       <endpoint name="SimpleStockQuoteService2">
          <address uri="http://localhost:9002/services/SimpleStockQuoteService"/>
       </endpoint>
       <endpoint name="SimpleStockQuoteService1">
          <address uri="http://localhost:9001/services/SimpleStockQuoteService"/>
       </endpoint>
       <sequence name="fault">
          <log level="full">
             <property name="MESSAGE" value="Executing default 'fault' sequence"/>
             <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
             <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
          </log>
          <drop/>
       </sequence>
       <sequence name="main">
          <in>
             <log level="full"/>
             <filter source="get-property('To')" regex="http://localhost:9000.*">
                <send/>
             </filter>
          </in>
          <out>
             <send/>
          </out>
          <description>The main sequence for the message mediation</description>
       </sequence>
       <messageStore class="org.apache.synapse.message.store.impl.jms.JmsStore" name="JMSMS">
          <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
          <parameter name="store.jms.cache.connection">false</parameter>
          <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
          <parameter name="store.jms.JMSSpecVersion">1.1</parameter>
          <parameter name="store.jms.destination">JMSMS</parameter>
       </messageStore>
       <messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="Forwarder3"
                         targetEndpoint="SimpleStockQuoteService3"
                         messageStore="JMSMS">
          <parameter name="client.retry.interval">1000</parameter>
          <parameter name="interval">1000</parameter>
          <parameter name="is.active">true</parameter>
       </messageProcessor>
       <messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="Forwarder1"
                         targetEndpoint="SimpleStockQuoteService1"
                         messageStore="JMSMS">
          <parameter name="client.retry.interval">1000</parameter>
          <parameter name="interval">1000</parameter>
          <parameter name="is.active">true</parameter>
       </messageProcessor>
       <messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="Forwarder2"
                         targetEndpoint="SimpleStockQuoteService2"
                         messageStore="JMSMS">
          <parameter name="client.retry.interval">1000</parameter>
          <parameter name="interval">1000</parameter>
          <parameter name="is.active">true</parameter>
       </messageProcessor>
    </definitions>
```

This configuration file `         synapse_sample_705.xml        ` is
available in the `         <EI_HOME>/samples/service-bus        `
directory.

**To build the sample**

1.  Start the EI with the sample 705 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
      
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

2.  Start three instances of the sample Axis2 server on HTTP ports 9001,
    9002 and 9003 and give unique names to each server. For instructions
    on starting the Axis2 server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

3.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

    Now you have a running EI instance and a back-end service deployed.
    In the next section, we will send a message to the back-end service
    through the EI using a sample client.

## Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <EI_HOME>/samples/axis2Client          ` directory:

    ``` bash
            ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=WSO2
    ```

## Analyzing the output

You can analyze the message sent by the EI to the secure service using
TCPMon.

On successful execution of the placeorder request, you will see the
following message on the back-end:

``` java
    Sun Aug 18 10:58:00 IST 2013 samples.services.SimpleStockQuoteService :: Accepted order #5 for : 18851 stocks of WSO2 at $ 61.782478265721714
```

If you use the stockqoute client to send the placeorder request several
times and observe the log on the back-end server, you will see that the
messages are distributed randomly among the back-end nodes since you
send the placeorder request randomly. However, if you send the request
message evenly (such as when using soapUI), you will see that the
messages are evenly distributed among the back-end nodes.
