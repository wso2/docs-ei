# Using the Message Forwarding Processor
## Synapse configuration
Shown below are the synapse artifacts that are used to define this use case.

```xml tab='Endpoint'
<endpoint xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteServiceEp">
    <address uri="http://localhost:9000/services/SimpleStockQuoteService">
        <suspendOnFailure>
            <errorCodes>-1</errorCodes>
            <progressionFactor>1.0</progressionFactor>
        </suspendOnFailure>
    </address>
</endpoint>
```

```xml tab='Proxy Service'
<proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="https http" startOnLoad="true" trace="disable">
          <description />
    <target>
       <inSequence>
        <log level="full"/>
        <property name="FORCE_SC_ACCEPTED" scope="axis2" value="true"/>
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
    class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
    messageStore="MyStore" name="ScheduledProcessor" targetEndpoint="StockQuoteServiceEp">
    <parameter name="interval">10000</parameter>
    <parameter name="throttle">false</parameter>
    <parameter name="target.endpoint">StockQuoteServiceEp</parameter>
</messageProcessor>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [scheduled task](../../../../develop/creating-artifacts/creating-scheduled-task), [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences), [endpoint](../../../../develop/creating-artifacts/creating-endpoints), [message store](../../../../develop/creating-artifacts/creating-a-message-store) and [message processor](../../../../develop/creating-artifacts/creating-a-message-processor) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

[Configure the ActiveMQ broker](../../../../setup/brokers/configure-with-ActiveMQ).

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

Now Start the SimpleStockQuoteService. When you Start the service you will see message getting delivered to the service. Even though service is down when we invoke it from the client. Here in the Proxy Service store mediator will store the getQuote request message in the "MyStore" Message Store. Message Processor will send the message to the endpoint configured as a message context property. Message processor will remove the message from the store only if message delivered successfully.
