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

``` xml
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

``` xml
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
parameters in the esb.toml file:

### `         synapse.threads.core        ` and `         synapse.threads.max        `

Iterate and Clone mediators use a thread pool to create new threads when
processing messages and sending messages parallelly. You can configure
the size of the thread pool by the
`         synapse.threads.core        ` parameter. The number of threads
specified via this parameter should be increased as required to balance
an increased load. Increasing the value specified for this parameter
results in higher performance of the Iterate and Clone mediators. You
can specify the maximum number of synapse threads in the pool by the
`         synapse.threads.max        ` parameter.

### `         synapse.threads.keepalive        `

The keep-alive time for idle threads in milliseconds. Once this time has
elapsed for an idle thread, it will be destroyed. This parameter is
applicable only if the Iterate or the Clone mediator is used to handle a
high load.

### `         synapse.threads.qlen        `

You can use this parameter to specify the length of the queue that is
used to hold the runnable tasks to be executed by the pool. You can
specify a finite value as the queue length by giving any positive
number. If this parameter is set to (-1) it means that the task queue
length is infinite.  
If the queue length is finite there can be situations where requests are
rejected when the task queue is full, and all the cores are occupied. If
the queue length is infinite, and if some thread locking happens, the
server can go out of memory. Therefore, you need to decide on an optimal
value based on the actual load.
