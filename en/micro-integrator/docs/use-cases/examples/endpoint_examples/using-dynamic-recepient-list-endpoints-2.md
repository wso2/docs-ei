# Routing a Message to a Dynamic List of Recipients and Aggregating Responses

## Example use case

This sample demonstrates message routing to a set of dynamic endpoints and aggregate responses.

## Synapse configuration

The XML configuration for this sample is as follows:

```xml tab='Error Handling Sequence'
<sequence name="errorHandler">
  <makefault response="true">
     <code xmlns:tns="http://www.w3.org/2003/05/soap-envelope" value="tns:Receiver" />
     <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER." />
  </makefault>
  <send />
</sequence>
```

```xml tab='Fault Sequence'
<sequence name="fault">
  <log level="full">
     <property name="MESSAGE" value="Executing default &quot;fault&quot; sequence" />
     <property name="ERROR_CODE" expression="get-property('ERROR_CODE')" />
     <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')" />
  </log>
  <drop />
</sequence>
```

```xml tab='Sequence'
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
```
## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the integration artifacts shown above.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service.

Invoke the Micro Integrator:

```bash
ant stockquote -Dtrpurl=http://localhost:8280/
```

When you have a look at the above
configuration,  you will see that it routes a cloned copy of a message
to each recipient defined within the dynamic recipient list , and that
each recipient responds back with a stock quote. When all the responses
reach the ESB, the responses are aggregated to form the final response,
which will be sent back to the client.

If you sent the client request through a TCP based conversation
monitoring tool such as TCPMon, you will see the structure of the
aggregated response message.
