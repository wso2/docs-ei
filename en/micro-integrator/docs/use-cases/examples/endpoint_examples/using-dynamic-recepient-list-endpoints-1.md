# Routing a Message to a Dynamic List of Recipients
## Example use case

This sample demonstrates message routing to a set of dynamic endpoints.

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

```xml tab='Main Sequence'
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
ant stockquote -Dmode=placeorder -Dtrpurl=http://localhost:8280/
```