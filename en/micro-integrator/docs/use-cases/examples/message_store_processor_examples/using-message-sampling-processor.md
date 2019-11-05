# Using the Message Sampling Processor
## Synapse configuration

Shown below are the synapse artifacts that are used to define this use case.

```xml tab='Send Sequence'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="send_seq">
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
```

```xml tab='Proxy Service'
<proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="https http" startOnLoad="true" trace="disable">
          <description />
    <target>
       <inSequence>
            <log level="full"/>
            <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
            <property name="OUT_ONLY" value="true"/>
            <store messageStore="MyStore"/>
        </inSequence>
        <outSequence>
             <send />
        </outSequence>
    </target>
</proxy>
```

```xml tab='Message Store'
<messageStore xmlns="http://ws.apache.org/ns/synapse" name="MyStore"/>
```

```xml tab='Message Processor'
<messageProcessor xmlns="http://ws.apache.org/ns/synapse"
     class="org.apache.synapse.message.processor.impl.sampler.SamplingProcessor"
     name="SamplingProcessor" messageStore="MyStore">
    <parameter name="interval">20000</parameter>
    <parameter name="sequence">send_seq</parameter>
</messageProcessor> 
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences), [message store](../../../../develop/creating-artifacts/creating-a-message-store), and [message processor](../../../../develop/creating-artifacts/creating-a-message-processor) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.

Set up the back-end service.

1. Download the [stockquote_service.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar).
2. Open a terminal, navigate to the location of the downloaded service, and run it using the following command:

    ```bash
    java -jar stockquote_service.jar
    ```

Invoke the service:

```bash
POST http://localhost:9090/services/StockQuoteProxy HTTP/1.1
Accept-Encoding: gzip,deflate
Content-Type: text/xml;charset=UTF-8
SOAPAction: "urn:getQuote"
Content-Length: 492
Host: localhost:9090
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
   <soapenv:Header/>
   <soapenv:Body>
      <ser:getQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
         <ser:request>
            <xsd:symbol>IBM</xsd:symbol>
         </ser:request>
      </ser:getQuote>
   </soapenv:Body>
</soapenv:Envelope>
```

When you send the request, the message will be dispatched to the proxy service. In the Proxy Service, store mediator will store the getQuote request message in the "MyStore" Message Store. Message Processor will consume the messages, and forward to the "send_seq" sequence in configured rate. You will observe that the service invocation rate is not changing when we increase the rate of proxy service invocation.
