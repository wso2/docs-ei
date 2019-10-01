# Setting HTTP status codes
## Example use case
A REST service typically sends HTTP status codes with its response. When you configure an API that send messages to a SOAP back-end service, you can set the status code of the HTTP response within the configuration.

To achieve this, set the status code parameter within the out sequence of the API definition.

## Synapse configuration  

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">`
   ...
   <outSequence>
    
    **<property name="HTTP_SC" value="201" scope="axis2" />`**
    
    **` </outSequence> `**
    
    </resource>
 </api>
```  

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create a REST Api artifact with the above configuration.
5. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Send the following request to the Micro Integrator:
    
```bash
curl -v http://127.0.0.1:8280/stockquote/view/IBM
```

The response message will contain the following response code (201) as configured above.  

```bash
< HTTP/1.1 201 Created
< Content-Type: text/xml; charset=UTF-8
< Server: Synapse-HttpComponents-NIO
< Date: Thu, 21 Mar 2013 09:03:00 GMT
< Transfer-Encoding: chunked
```