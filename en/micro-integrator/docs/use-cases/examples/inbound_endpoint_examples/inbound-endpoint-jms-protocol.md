# Inbound Endpoint JMS Protocol Sample

### Introduction

This sample demonstrates how one way message bridging from JMS to HTTP
can be done using the inbound JMS endpoint.

### Prerequisites

-   Download and set up Apache ActiveMQ. For more information, see the
    [ActiveMQ getting started
    guide](http://activemq.apache.org/getting-started.html) .
-   Copy the following client libraries from the
    `           <AMQ_HOME>/lib          ` directory to the
    `           <EI_HOME>/lib          ` directory.

    **ActiveMQ 5.8.0 and above**  

    -   -   `              activemq-broker-5.8.0.jar             `
        -   `              activemq-client-5.8.0.jar             `
        -   `              geronimo-jms_1.1_spec-1.1.1.jar             `
        -   `              geronimo-j2ee-management_1.1_spec-1.0.1.jar             `
        -   `              hawtbuf-1.9.jar             `

    **Earlier version of ActiveMQ**

    -   -   `               activemq-core-5.5.1.jar              `

        -   `               geronimo-j2ee-management_1.0_spec-1.0.jar              `

        -   `               geronimo-jms_1.1_spec-1.1.1.jar              `

### Building the sample

The XML configuration for this sample is as follows:

```
     <definitions xmlns="http://ws.apache.org/ns/synapse">
       <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
          <parameter name="cachableDuration">15000</parameter>
       </registry>
       <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager">
          <parameter name="cachableDuration">15000</parameter>
       </taskManager>
       <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="jms_inbound" sequence="request" onError="fault" protocol="jms" suspend="false">
          <parameters>
             <parameter name="interval">1000</parameter>
             <parameter name="transport.jms.Destination">ordersQueue</parameter>
             <parameter name="transport.jms.CacheLevel">1</parameter>
             <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
             <parameter name="sequential">true</parameter>
             <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
             <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
             <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
             <parameter name="transport.jms.SessionTransacted">false</parameter>
             <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
          </parameters>
       </inboundEndpoint>
       <sequence name="request" onError="fault">
          <call>
             <endpoint>
                <address format="soap12" uri="http://localhost:9000/services/SimpleStockQuoteService"/>
             </endpoint>
          </call>
          <drop/>
       </sequence>
    </definitions>
```

This configuration file `         synapse_sample_901.xml        ` is
available in the `         <EI_HOME>/samples/service-bus        `
directory.

**To build the sample**

1.  Start the ESB Profile with the sample 901 configuration. For
    instructions on starting a sample ESB configuration, see [Starting
    the ESB with a sample
    configuration](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingaservicebussampleconfiguration)
    .  
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

2.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-StartingtheAxis2server)
    .

3.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Deployingsampleback-endservices)
    .

### Executing the sample

**To execute the sample client**

-   Log on to the ActiveMQ console using the
    <http://localhost:8161/admin> url.
-   Browse the queue `          ordersQueue         ` listening via the
    above endpoint.
-   Add a new message with the following content to the queue:

    ``` html/xml
            <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing">
                <soapenv:Body>
                    <m0:getQuote xmlns:m0="http://services.samples"> 
                        <m0:request>
                            <m0:symbol>IBM</m0:symbol>
                        </m0:request>
                    </m0:getQuote>
                </soapenv:Body>
            </soapenv:Envelope>
    ```

### Analyzing the output

You will see that the JMS endpoint gets the message from the queue and
sends it to the stock quote service.
