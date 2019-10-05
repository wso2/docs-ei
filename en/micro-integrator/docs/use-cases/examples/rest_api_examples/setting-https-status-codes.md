# Setting HTTP status codes
## Example use case
A REST service typically sends HTTP status codes with its response. When you configure an API that send messages to a SOAP back-end service, you can set the status code of the HTTP response within the configuration.

To achieve this, set the status code parameter within the out sequence of the API definition.

## Synapse configuration  

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">`
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
            <property name="HTTP_SC" value="201" scope="axis2" />
            <send/>
        </outSequence>  
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

Send the following request to the Micro Integrator:
    
```bash
curl -v http://127.0.0.1:8290/stockquote/view/IBM
```

The response message will contain the following response code (201) as configured above.  

```bash
< HTTP/1.1 201 Created
< Content-Type: text/xml; charset=UTF-8
< Server: Synapse-HttpComponents-NIO
< Date: Thu, 21 Mar 2013 09:03:00 GMT
< Transfer-Encoding: chunked
```