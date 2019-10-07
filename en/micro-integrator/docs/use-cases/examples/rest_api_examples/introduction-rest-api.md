# Introduction to REST APIs

## Example use case

You can configure REST endpoints in the Micro Integrator by directly specifying HTTP verbs, URL patterns, URI templates, HTTP media types, and other related headers. You can define REST APIs and the associated resources by combining REST APIs with mediation features provided by the underlying messaging framework.

## Synapse configuration

Following is a sample REST Api configuration that we can used to implement this scenario.

This is a REST api with two api resources. The GET calls are handled by the first resource (StockQuoteAPI). These REST calls will get converted into SOAP calls and sent to the back-end service. The response will be sent to the client in POX format.

```xml
<api name="StockQuoteAPI" context="/stockquote" xmlns="http://ws.apache.org/ns/synapse">
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
```
## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. [Create the rest api](../../../../develop/creating-artifacts/creating-an-api) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.

Invoke the sample Api:

1. Save the following sample request as "placeorder.xml" in your local file system. 

    ```bash
    <placeOrder xmlns="http://services.samples">
      <order>
         <price>50</price>
         <quantity>10</quantity>
         <symbol>IBM</symbol>
      </order>
    </placeOrder>
    ```

2.  The following command posts a simple XML request to the Micro Integrator. Execute the command that is used to invoke a SOAP service. The Micro Integrator returns the 202 response back to the client.

    ```bash
    curl -v -d @placeorder.xml -H "Content-type: application/xml" http://127.0.0.1:8290/stockquote/order/
    ```