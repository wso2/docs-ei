# Handling non-matching resources
## Example use case
    
In this scenario, we are defining a sequence to be invoked if the the Micro Integrator is unable to find a matching resource definition for a specific API invocation. This sequence generates a response indicating an error when no matching resource definition is found.
    
## Synapse configurations
    
Following is a sample REST Api configuration and mediation Sequence that we can used to implement this scenario.
    
```xml tab='REST Api'
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
    
```xml tab='Sequence'
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
## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create a REST Api artifact with the above configuration.
4. Create a mediation sequence with the above configuration.
5. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Send an invalid request to the back end as follows:
    
```bash
curl -X GET http://localhost:8280/jaxrs/customers-wrong/123
```
    
You will get the following response:
    
```bash
<tp:fault xmlns:tp="http://test.com">
<tp:code>404</tp:code>
<tp:type>Status report</tp:type>
<tp:message>Not Found</tp:message>
<tp:description>The requested resource (//customers-wrong/123) is not available.</tp:description>
</tp:fault>
```

Notice that we have specified the REST_URL_POSTFIX property with the value set to "remove". When invoking this API, even if the request contains a trailing slash after the context (e.g., `POST http://127.0.0.1:8287/orderdelayAPI/` instead of `POST  http://127.0.0.1:8287/orderdelayAPI`, the endpoint will be called correctly.