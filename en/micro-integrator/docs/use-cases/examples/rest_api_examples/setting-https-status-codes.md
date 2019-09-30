# Setting HTTP status codes
    
A REST service typically sends HTTP status codes with its response. When you configure an API that send messages to a SOAP back-end service, you can set the status code of the HTTP response within the ESB configuration. To achieve this, set the status code parameter within the out sequence of the API definition:  
    
`         <definitions>        `
    
    `         <api xmlns="                   http://ws.apache.org/ns/synapse                  " name="StockQuoteAPI" context="/stockquote">        `
    
    `         ...        `
    
    `                   <outSequence>                 `
    
    **`          <property name="HTTP_SC" value="201" scope="axis2" />         `**
    
    **`          </outSequence>         `**
    
    `         </resource>        `
    
         </api>
    
        </definitions>
    
Now you can try the REST call with the following command.  
    
`curl -v http://127.0.0.1:8280/stockquote/view/IBM`
    
The response message will contain the following response code (201) as configured above.  

```
< HTTP/1.1 201 Created
< Content-Type: text/xml; charset=UTF-8
< Server: Synapse-HttpComponents-NIO
< Date: Thu, 21 Mar 2013 09:03:00 GMT
< Transfer-Encoding: chunked
```