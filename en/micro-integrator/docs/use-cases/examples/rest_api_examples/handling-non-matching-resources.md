# Handling non-matching resources
    
In this scenario, we are defining a sequence to be invoked if the the ESB profile is unable to find a matching resource definition for a specific API invocation. This sequence generates a response indicating an error when no matching resource definition is found.
    
### Setting up the back end
    
For the back-end service, we are using the StockQuote Service that is shipped with the ESB profile. Configure and start the back-end service as described in [Starting Sample Back-End Services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples). We will use [cURL](http://curl.haxx.se/) as the REST client to invoke the the ESB profile API.
    
### Configuring the API
    
Create an API using the following configuration:
    
```
<api xmlns="http://ws.apache.org/ns/synapse" name="jaxrs" context="/jaxrs">
   <resource methods="GET" uri-template="/customers/{id}">
      <inSequence>
         <send>
            <endpoint>
               <address uri="http://localhost:8280/jaxrs_basic/services/customers/customerservice"/>
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </resource>
</api> 
```
    
### Creating the sequence
    
Create a new sequence with the following configuration:
    
```
 <sequence xmlns="http://ws.apache.org/ns/synapse" name="_resource_mismatch_handler_">
   <payloadFactory>
      <format>
         <tp:fault xmlns:tp="http://test.com">
            <tp:code>404</tp:code>
            <tp:type>Status report</tp:type>
            <tp:message>Not Found</tp:message>
            <tp:description>The requested resource (/$1) is not available.</tp:description>
         </tp:fault>
      </format>
      <args>
         <arg xmlns:ns="http://org.apache.synapse/xsd" expression="$axis2:REST_URL_POSTFIX"/>
      </args>
   </payloadFactory>
   <property name="RESPONSE" value="true" scope="default"/>
   <property name="NO_ENTITY_BODY" action="remove" scope="axis2"/>
   <property name="HTTP_SC" value="404" scope="axis2"/>
   <header name="To" action="remove"/>
   <send/>
   <drop/>
</sequence>
```
### Executing the sample
    
Send an invalid request to the back end as follows:
    
```
curl -X GET http://localhost:8280/jaxrs/customers-wrong/123
```
    
You will get the following response:
    
```
<tp:fault xmlns:tp="http://test.com">
<tp:code>404</tp:code>
<tp:type>Status report</tp:type>
<tp:message>Not Found</tp:message>
<tp:description>The requested resource (//customers-wrong/123) is not available.</tp:description>
</tp:fault>
```

Notice that we have specified the REST\_URL\_POSTFIX property with the value set to "remove". When invoking this API, even if the request contains a trailing slash after the context (e.g., `POST http://127.0.0.1:8287/orderdelayAPI/` instead of `POST  http://127.0.0.1:8287/orderdelayAPI`, the endpoint will be called correctly.