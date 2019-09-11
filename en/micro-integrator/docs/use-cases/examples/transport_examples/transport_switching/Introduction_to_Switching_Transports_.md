# Introduction to Switching Transports

### Introduction

This sample demonstrates how the ESB receives a messages over the JMS
transport and forwards it over a HTTP/S transport.. In this sample, the
client sends a request message to the proxy service exposed in JMS. The
ESB forwards this message to the HTTP EPR of the simple stock quote
service hosted on the sample Axis2 server, and returns the reply back to
the client through a JMS temporary queue.

### Prerequisites

-   Configure WSO2 ESB's JMS transport with ActiveMQ and enable the JMS
    transport listener and sender. For information on how to configure
    with ActiveMQ, and how to enable the JMS transport listener and
    sender, see [Configure with
    ActiveMQ](https://docs.wso2.com/display/EI650/Configure+with+ActiveMQ)
    .

    !!! Note
        If you are using ActiveMQ version 5.8.0 or above, you need to copy the [hawtbuf-1.2.jar](http://repo1.maven.org/maven2/org/fusesource/hawtbuf/hawtbuf/1.2/hawtbuf-1.2.jar) file from the `           <ActiveMQ>/lib          ` directory to the `           <ESB_HOME>/repository/components/lib          `directory.

-   For a list of prerequisites, see [Prerequisites to Start the ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites).

### Building the sample

The XML configuration for this sample is as follows:

```
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy name="StockQuoteProxy" transports="jms">
        <target>
            <inSequence>
                <property action="set" name="OUT_ONLY" value="true"/>
            </inSequence>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
            </endpoint>
            <outSequence>
                <send/>
            </outSequence>
        </target>
        <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        <parameter name="transport.jms.ContentType">
            <rules>
                <jmsProperty>contentType</jmsProperty>
                <default>application/xml</default>
            </rules>
        </parameter>
    </proxy>
</definitions>
```

This configuration file `         synapse_sample_250.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

    Now you have a running ESB instance and a back-end service deployed.
    In the next section, we will send a message to the back-end service
    through the ESB using a sample client.

### Executing the sample

**To execute the sample JMS client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory:

    ``` bash
    ant jmsclient -Djms_type=pox -Djms_dest=dynamicQueues/StockQuoteProxy -Djms_payload=MSFT
    ```

### Analyzing the output

When you run the client and analyze the ESB debug log, you will see the
following entry, which shows that the JMS listener received the request
message:

When you run the client, it sends a plain XML formatted place order
request to a JMS queue named `         StockQuoteProxy        ` . The
ESB polls on this queue for any incoming messages and picks up the
request. If you run the ESB in the DEBUG mode, you will see the
following entry on the console.

``` java
[JMSWorker-1] DEBUG ProxyServiceMessageReceiver -Proxy Service StockQuoteProxy received a new message...
```

Then, the ESB mediated the request and forwards it to the sample Axis2
server over HTTP. If you analyze the console running the sample Axis2
server, you will see the following message indicating that the server
has accepted an order:

``` java
Accepted order for : 16517 stocks of MSFT at $ 169.14622538721846
```

!!! Note
    It is possible to instruct a JMS proxy service to listen to an already
existing destination without creating a new one. To do this, use the
property elements on the proxy service definition to specify the
destination and connection factory.

``` java
<property name="transport.jms.Destination" value="dynamicTopics/something.TestTopic"/>
```