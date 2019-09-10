# Introduction to REST APIs

```
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <!-- You can add any flat sequences, endpoints, etc.. to this synapse.xml file if you do *not* want to keep the artifacts in several files -->
  <api name="StockQuoteAPI" context="/stockquote">
     <resource uri-template="/view/{symbol}" methods="GET">
        <inSequence>
           <payloadFactory>
             <format>
                <m0:getQuote xmlns:m0="http://services.samples">
                   <m0:request>
                     <m0:symbol>$1</m0:symbol>
                   </m0:request>
               </m0:getQuote>
             </format> 
              <args>
                <arg expression="get-property('uri.var.symbol')"/>
              </args>
           </payloadFactory>
           <header name="Action" value="urn:getQuote"/>
           <send>
             <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
             </endpoint>
           </send>
        </inSequence>
        <outSequence>
           <send/>
        </outSequence>
     </resource>
     <resource methods="POST" url-mapping="/order/*">
        <inSequence>
           <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
           <property name="OUT_ONLY" value="true"/>
           <header name="Action" value="urn:placeOrder"/>
           <send>
              <endpoint>
                  <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
              </endpoint>
           </send>
        </inSequence>      
     </resource>
  </api>
</definitions>
```

The GET calls are handled by the first resource in the StockQuoteAPI. These REST calls will get converted into SOAP calls and sent to the Axis2 server. The response will be sent to the client in POX format.

The following command posts a simple XML to the ESB. Save the following sample request as "placeorder.xml" in your local file system and execute the command that is used to invoke a SOAP service. The ESB returns the 202 response back to the client.

```
curl -v -d @placeorder.xml -H "Content-type: application/xml" http://127.0.0.1:8280/stockquote/order/
```

```
<placeOrder xmlns="http://services.samples">
  <order>
     <price>50</price>
     <quantity>10</quantity>
     <symbol>IBM</symbol>
  </order>
</placeOrder>
```