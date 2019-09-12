# Split-Aggregate Pattern

The split-aggregate pattern sends an incoming request from the client to
several target endpoints simultaneously. Then it combines all the
responses from each back-end to a single response, and sends the
response back to the client.  
This pattern can be implemented in WSO2 EI using the [Iterate
mediator](https://docs.wso2.com/display/EI650/Iterate+Mediator) , [Clone
mediator](https://docs.wso2.com/display/EI650/Clone+Mediator) and
[Aggregate
mediator](https://docs.wso2.com/display/EI650/Aggregate+Mediator) . The
Iterate mediator splits the message into a number of different messages
that are derived from the parent message using Xpath, and sends the
messages to target endpoints using the same sequence. The Clone mediator
sends the same message to target endpoints using different target
sequences. The Aggregate mediator collects all the response messages and
creates one response message.

## Split-aggregate use case

The following use case demonstrates the split-aggregate pattern:

The Iterate mediator splits the original request message to a number of
different messages using the Xpath expression
`         //m0:getQuote/m0:request        ` . Then the messages are sent
to the `         SimpleStockQuoteService        ` soap back-end. All the
responses are aggregated and sent back to the client at the OutSequence.

Following is the proxy service used in this scenario:

``` 
    <?xml version="1.0" encoding="UTF-8"?>
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="SplitAggregateProxy" startOnLoad="true">
       <target>
          <inSequence>
             <iterate xmlns:m0="http://services.samples" preservePayload="true"
                      attachPath="//m0:getQuote" expression="//m0:getQuote/m0:request">
                <target>
                   <sequence>
                      <send>
                         <endpoint>
                            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                         </endpoint>
                      </send>
                   </sequence>
                </target>
             </iterate>
          </inSequence>
          <outSequence>
             <aggregate>
                <completeCondition>
                   <messageCount/>
                </completeCondition>
                <onComplete xmlns:m0="http://services.samples" expression="//m0:getQuoteResponse">
                   <send/>
                </onComplete>
             </aggregate>
          </outSequence>
       </target>
    </proxy>
```

The sample request sent to the proxy service is given below. This
request is split into 4 messages and sent to the
`         SimpleStockQuoteService        ` . Then, the Aggregate
mediator aggregates the responses and sends back the aggregated
response.

```
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
       <soapenv:Header/>
       <soapenv:Body>
          <ser:getQuote>
              <ser:request>
                <xsd:symbol>ABC</xsd:symbol>
             </ser:request> 
             <ser:request>
                <xsd:symbol>WSO2</xsd:symbol>
             </ser:request>
              <ser:request>
                <xsd:symbol>AAA</xsd:symbol>
             </ser:request>  
              <ser:request>
                <xsd:symbol>BBB</xsd:symbol>
             </ser:request>
          </ser:getQuote>    
       </soapenv:Body>
    </soapenv:Envelope>
```

## Tuning the performance

The Iterate, Clone and Aggregate mediators demonstrate high performance
due to default threading and memory configuration. The performance of
these mediators can be further increased by tuning the following
parameters in the ei.toml file:

```toml
[[mediation]]
synapse.core_threads = 20
synapse.max_threads = 100
synapse.threads_queue_length = 10
```

See the [descriptions of these parameters](../../../references/config-catalog/#mediation-process).
