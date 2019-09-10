# Using Recipient List Endpoints

## Sample 60: Routing a Message to a Static List of Recipients

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .


**Objective** : Demonstrate message routing to a set of static
endpoints.

**Prerequisites**

Start ESB with the following sample configuration:

``` html/xml
    <?xml version="1.0" encoding="UTF-8"?>
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <sequence name="main" onError="errorHandler">
            <in>
                <send>
                    <endpoint>
                        <!--List of Recipients (static)-->
                        <recipientlist>
                            <endpoint>
                                <address uri="http://localhost:9001/services/SimpleStockQuoteService"/>
                            </endpoint>
                            <endpoint>
                                <address uri="http://localhost:9002/services/SimpleStockQuoteService"/>
                            </endpoint>
                            <endpoint>
                                <address uri="http://localhost:9003/services/SimpleStockQuoteService"/>
                            </endpoint>
                        </recipientlist>
                    </endpoint>
                </send>
                <drop/>
            </in>
            <out>
                <log level="full"/>
                <drop/>
            </out>
        </sequence>
        <sequence name="errorHandler">
            <makefault response="true">
                <code xmlns:tns="http://www.w3.org/2003/05/soap-envelope" value="tns:Receiver"/>
                <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER."/>
            </makefault>
            <send/>
        </sequence>
    </definitions>
```

Deploy the SimpleStockQuoteService and start three instances of sample
Axis2 server as mentioned in sample 52 Sessionless Load Balancing
Between 3 Endpoints .

The above configuration routes a cloned copy of a message to each
recipient defined within the static recipient list. To test this, run
the StockQuote client to send an out-only message as follows:

``` java
    ant stockquote -Dmode=placeorder -Dtrpurl=http://localhost:8280/
```

This client sends a request to the SimpleStockQuoteService through the
ESB. ESB will create cloned copies of the message and route to the three
endpoints mentioned in the configuration. SimpleStockQuoteService prints
the details of the placed order. If you examine the console output of
each server, you can see that requests are processed by the three
servers as follows:

``` java
    Accepted order #1 for : 15738 stocks of IBM at $ 185.51155223506518
```

Now shutdown MyServer1 and resend the request. You will observe that
requests are still processed by MyServer2 and MyServer3.

## Routing a Message to a Dynamic List of Recipients

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .


-   [Introduction](#Sample61:RoutingaMessagetoaDynamicListofRecipients-Introduction)
-   [Prerequisites](#Sample61:RoutingaMessagetoaDynamicListofRecipients-Prerequisites)
-   [Building the
    sample](#Sample61:RoutingaMessagetoaDynamicListofRecipients-Buildingthesample)
-   [Executing the
    sample](#Sample61:RoutingaMessagetoaDynamicListofRecipients-Executingthesample)

### Introduction

This sample demonstrates message routing to a set of dynamic endpoints.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <sequence name="errorHandler">
          <makefault response="true">
             <code xmlns:tns="http://www.w3.org/2003/05/soap-envelope" value="tns:Receiver" />
             <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER." />
          </makefault>
          <send />
       </sequence>
       <sequence name="fault">
          <log level="full">
             <property name="MESSAGE" value="Executing default &quot;fault&quot; sequence" />
             <property name="ERROR_CODE" expression="get-property('ERROR_CODE')" />
             <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')" />
          </log>
          <drop />
       </sequence>
       <sequence name="main" onError="errorHandler">
          <in>
             <property name="EP_LIST" value="http://localhost:9001/services/SimpleStockQuoteService,http://localhost:9002/services/SimpleStockQuoteService,http://localhost:9003/services/SimpleStockQuoteService"/>  
             <property name="OUT_ONLY" value="true" />
             <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2" />
             <send>
                <endpoint>
                   <recipientlist>
                      <endpoints value="{get-property('EP_LIST')}" max-cache="20" />
                   </recipientlist>
                </endpoint>
             </send>
             <drop/>
          </in>
       </sequence>
    </definitions>
```

This configuration file `         synapse_sample_61.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start three instances of the sample Axis2 server on HTTP ports 9001,
    9002 and 9003 and give unique names to each server. For instructions
    on starting the Axis2 server, see [Starting the Axis2
    server](https://docs.wso2.com/display/ESB500/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory to
    send an out-only message:

    ``` bash
            ant stockquote -Dmode=placeorder -Dtrpurl=http://localhost:8280/
    ```

## Routing a Message to a Dynamic List of Recipients and Aggregating Responses

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .


-   [Introduction](#Sample62:RoutingaMessagetoaDynamicListofRecipientsandAggregatingResponses-Introduction)
-   [Prerequisites](#Sample62:RoutingaMessagetoaDynamicListofRecipientsandAggregatingResponses-Prerequisites)
-   [Building the
    sample](#Sample62:RoutingaMessagetoaDynamicListofRecipientsandAggregatingResponses-Buildingthesample)
-   [Executing the
    sample](#Sample62:RoutingaMessagetoaDynamicListofRecipientsandAggregatingResponses-Executingthesample)
-   [Analyzing the
    output](#Sample62:RoutingaMessagetoaDynamicListofRecipientsandAggregatingResponses-Analyzingtheoutput)

### Introduction

This sample demonstrates message routing to a set of dynamic endpoints
and aggregate responses.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <sequence name="errorHandler">
          <makefault response="true">
             <code xmlns:tns="http://www.w3.org/2003/05/soap-envelope" value="tns:Receiver" />
             <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER." />
          </makefault>
          <send />
       </sequence>
       <sequence name="fault">
          <log level="full">
             <property name="MESSAGE" value="Executing default &quot;fault&quot; sequence" />
             <property name="ERROR_CODE" expression="get-property('ERROR_CODE')" />
             <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')" />
          </log>
          <drop />
       </sequence>
       <sequence name="main" onError="errorHandler">
          <in>
             <property name="EP_LIST" value="http://localhost:9001/services/SimpleStockQuoteService,http://localhost:9002/services/SimpleStockQuoteService,http://localhost:9003/services/SimpleStockQuoteService"/>  
             <send>
                <endpoint>
                   <recipientlist>
                      <endpoints value="{get-property('EP_LIST')}" max-cache="20" />
                   </recipientlist>
                </endpoint>
             </send>
             <drop/>
          </in>
          <out>
            <!--Aggregate responses-->
            <aggregate>
               <onComplete xmlns:m0="http://services.samples"
                              expression="//m0:getQuoteResponse">
                 <log level="full"/>
                 <send/>
               </onComplete>
            </aggregate>
          </out>
       </sequence>
    </definitions>
```

This configuration file `         synapse_sample_62.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start three instances of the sample Axis2 server on HTTP ports 9001,
    9002 and 9003 and give unique names to each server. For instructions
    on starting the Axis2 server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI6xx/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory:

    ``` bash
            ant stockquote -Dtrpurl=http://localhost:8280/
    ```

### Analyzing the output

When you have a look at the `         synapse_sample_62.xml        `
configuration,  you will see that it routes a cloned copy of a message
to each recipient defined within the dynamic recipient list , and that
each recipient responds back with a stock quote. When all the responses
reach the ESB, the responses are aggregated to form the final response,
which will be sent back to the client.

If you sent the client request through a TCP based conversation
monitoring tool such as TCPMon, you will see the structure of the
aggregated response message.

