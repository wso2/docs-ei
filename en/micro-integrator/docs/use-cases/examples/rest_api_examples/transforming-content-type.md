# Transforming the content type
## Example use case  
This section describes how you can transform the content type of a message using an API. In this scenario, the API exposes a REST back-end service that accepts and returns XML and JSON messages for HTTP methods as follows:
    
-   GET - response is in JSON format
-   POST - accepts XML request and returns response in JSON format
-   PUT - accepts JSON request and returns response in JSON format
-   DELETE - empty request body should be sent  
    
## Synapse configuration
    
Create an API using the following configuration:
    
```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="StarbucksService" context="/Starbucks_Service">
    <resource methods="POST" url-mapping="/orders/add">
        <inSequence>
            <property name="REST_URL_POSTFIX" scope="axis2" action="remove"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
    <resource methods="PUT" url-mapping="/orders/edit">
        <inSequence>
            <log level="full"/>
            <property name="REST_URL_POSTFIX" scope="axis2" action="remove"/>
            <property name="messageType" value="application/json" scope="axis2"/>
            <property name="ContentType" value="application/json" scope="axis2"/>
            <send>
                <endpoint>
                    <address uri="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/orders/" format="rest"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
    <resource methods="DELETE GET" uri-template="/orders/{id}">
        <inSequence>
            <send>
                <endpoint>
                    <address uri="http://localhost:8280/StarbucksService/services/Starbucks_Outlet_Service/"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
</api>
```
    
## Build and run
    
The context of the API is ‘/Starbucks_Service’. For every HTTP method, a url-mapping or uri-template is defined, and the URL to call the methods differ with the defined mapping or template.
    
Following is the CURL command to send a GET request to the API:
    
` curl -v -X GET http://localhost:8280/Starbucks_Service/orders/123                           `
    
The response from the back end to the ESB profile will be:
    
`         {"Order":{"additions":"Milk","drinkName":"Vanilla Flavored Coffee","locked":false,"orderId":123}}        `
    
The ESB profile transforms this response to XML and send it back as:
    
```
<Order>
    <additions>Milk</additions>
    <drinkName>Vanilla Flavored Coffee</drinkName>
    <locked>false</locked>
    <orderId>123</orderId>
</Order>
```
    
Following is the cURL command to send an HTTP POST request to the API:
    
`         curl -v -H "Content-Type: application/xml" -X POST -d @placeOrder.xml                              http://localhost:8280/Starbucks_Service/orders/add                           `
    
where `         placeOrder.xml        ` has the following content on the order:
    
```
<Order>
    <drinkName>Mocha Flavored Coffee</drinkName>
    <additions>Caramel</additions>
</Order>
```
    
This XML request will be sent to the back end, which will send back a JSON response to the ESB profile:
    
`         {"Order":{"additions":"Caramel","drinkName":"Mocha Flavored Coffee","locked":false,"orderId":"d088a289-1be3-453b-ab37-7609828d2197"}}        `
    
The ESB profile converts this response to XML and sends it back to the client:
    
```
<Order>
    <additions>Caramel</additions>
    <drinkName>Mocha Flavored Coffee</drinkName>
    <locked>false</locked>
    <orderId>d088a289-1be3-453b-ab37-7609828d2197</orderId>
</Order>
```
    
Following is the cURL command for sending an HTTP PUT request:
    
`         curl -v -H "Content-Type: application/xml" -X PUT -d @editOrder.xml                              http://localhost:8280/Starbucks_Service/orders/edit                           ` where `         editOrder.xml        ` has the following syntax:
    
```
<Order>
    <orderId>d088a289-1be3-453b-ab37-7609828d2197</orderId>
    <additions>Chocolate Chip Cookies</additions>
</Order>
```
    
The ESB profile will convert this request to JSON and send it to the back end. The response will be in JSON format:
    
`         {"Order":{"additions":"Chocolate Chip Cookies","drinkName":"Mocha Flavored Coffee","locked":false,"orderId":"d088a289-1be3-453b-ab37-7609828d2197"}}`
    
The ESB profile converts this response to XML and sends it back to the client:
    
```
<Order>
    <additions>Chocolate Chip Cookies</additions>
    <drinkName>Mocha Flavored Coffee</drinkName>
    <locked>false</locked>
    <orderId>d088a289-1be3-453b-ab37-7609828d2197</orderId>
</Order>
```

Following is the cURL command for sending an HTTP DELETE request:
    
`         curl -v -X DELETE                              http://localhost:8280/Starbucks_Service/orders/d088a289-1be3-453b-ab37-7609828d2197                           `
    
This request will be sent to the back end, and the order with the specified ID will be deleted.