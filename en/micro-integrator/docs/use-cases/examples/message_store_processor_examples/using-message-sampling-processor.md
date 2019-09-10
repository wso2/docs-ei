# Sample 701: Introduction to Message Sampling Processor

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


**Objective:** Introduction to Message Sampling Processor.

``` html/xml
                <!-- Introduction to  Message Sampling Processor -->
    
                <definitions xmlns="http://ws.apache.org/ns/synapse">
                    <sequence name="send_seq">
                        <send>
                            <endpoint>
                                <address uri="http://localhost:9000/services/SimpleStockQuoteService">
                                    <suspendOnFailure>
                                    <errorCodes>-1</errorCodes>
                                    <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                            </endpoint>
                        </send>
                    </sequence>
                    <sequence name="main">
                        <in>
                            <log level="full"/>
                            <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
                            <property name="OUT_ONLY" value="true"/>
                            <store messageStore="MyStore"/>
                        </in>
                        <out/>
                        <description>The main sequence for the message mediation</description>
                    </sequence>
                    <messageStore name="MyStore"/>
                    <messageProcessor
                         class="org.apache.synapse.message.processor.impl.sampler.SamplingProcessor"
                         name="SamplingProcessor" messageStore="MyStore">
                        <parameter name="interval">20000</parameter>
                        <parameter name="sequence">send_seq</parameter>
                    </messageProcessor>
                </definitions>
            
```

**Prerequisites** :

-   Create the above configuration and deploy it in the ESB profile.
-   Start the SimpleStockQuoteService if its not already started

To Execute the Client few times:

``` java
    ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService -Dtrpurl=http://localhost:8280/ -Dmode=placeorder
```

When you execute the client, the message will be dispatched to the main
sequence. In the Main sequence, store mediator will store the placeOrder
request message in the "MyStore" Message Store. Message Processor will
consume the messages, and forward to the "send\_seq" sequence in
configured rate. You will observe that the service invocation rate is
not changing when we increase the rate of client executions.
