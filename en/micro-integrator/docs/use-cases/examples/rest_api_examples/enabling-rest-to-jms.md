# Enabling REST to JMS
    
This section describes how an API can receive an HTTP message sent from a REST client and place it on a JMS queue. The API receives the HTTP message and sends it to a JMS queue that resides in a message broker.
    
### Create a queue in the message broker
    
Create a queue by the name of SimpleStockQuoteService in the MB profile.
    
### Configuring the API
    
Following is a sample API configuration in the ESB profile that we can used to implement this scenario:
    
```
<api xmlns="http://ws.apache.org/ns/synapse" name="RestToJmsAPI" context="/jmsstockquote">
   <resource methods="POST" url-mapping="/order/*">
      <inSequence>
         <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
         <property name="OUT_ONLY" value="true"/>
         <send>
            <endpoint>
               <address uri="jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=conf/jndi.properties&amp;transport.jms.DestinationType=queue"/>
            </endpoint>
         </send>
      </inSequence>
   </resource>
</api>
```
    
In this API configuration, we have set the OUT_ONLY property to true, as it is a one-way invocation. The message is sent to the JMS queue using the JMS Transport sender. (Note the prefix “jms” in the address endpoint URI.) Since we have set the FORCE_SC_ACCEPTED property value to true ,the ESB profile returns a 202 response back to the client.
    
### Executing the sample
    
Following is a sample cURL command to invoke this API:
    
` curl -v -d @placeorder.xml -H "Content-type: application/xml" http://127.0.0.1:8280/jmsstockquote/order/        `
    
Save the following sample place order request as "placeorder.xml" in your local file system and execute the above command.
    
```
<placeOrder xmlns="http://services.samples">
  <order>
     <price>50</price>
     <quantity>10</quantity>
     <symbol>IBM</symbol>
  </order>
</placeOrder>
```
    
If you go to the management console of the MB profile and inspect the SimpleStockQuoteService queue, you will see a message queued there.
    
## Exposing a back-end REST service using a different API
    
In this scenario, we are exposing a back-end REST service using a REST API in the ESB profile. That means we are exposing a different API to the client for a REST back-end service.
    
### Setting up the back end
    
For the REST back-end service, we are going to use the [JAX-RS Basics](https://docs.wso2.com/display/EI650/JAX-RS+Basics) sample service available in the ESB profile. Configure and deploy the JAX-RS Basics sample by following the instructions for b uilding and running the sample on the [JAX-RS Basics](https://docs.wso2.com/display/EI650/JAX-RS+Basics) page.
    
### Configuring the API
    
Following is a sample configuration we can use to configure this scenario:
    
```
<api xmlns="http://ws.apache.org/ns/synapse" name="ClientServiceAPI" context="/clientservice">
   <resource methods="OPTIONS DELETE GET" uri-template="/clients/{id}">
      <inSequence>

         <property name="REST_URL_POSTFIX" expression="fn:concat('/customers/',get-property('uri.var.id'))" scope="axis2"/>
         <send>
            <endpoint>
               <address uri="http://localhost:9763/jaxrs_basic/services/customers/customerservice/"/>
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </resource>
   <resource methods="POST PUT" uri-template="/clients">
      <inSequence>
         <property name="REST_URL_POSTFIX" value="/customers" scope="axis2"/>
         <switch source="$ctx:REST_METHOD">
            <case regex="POST">
               <payloadFactory>
                  <format>
                     <Customer xmlns="">
                        <name>$1</name>
                     </Customer>
                  </format>
                  <args>
                     <arg expression="//addClient/name"/>
                  </args>
               </payloadFactory>
            </case>
            <case regex="PUT">
               <payloadFactory>
                  <format>
                     <Customer xmlns="">
                        <id>$1</id>
                        <name>$2</name>
                     </Customer>
                  </format>
                  <args>
                     <arg expression="//updateClient/id"/>
                     <arg expression="//updateClient/name"/>
                  </args>
               </payloadFactory>
            </case>
         </switch>
         <send>
            <endpoint>
               <address uri="http://localhost:9763/jaxrs_basic/services/customers/customerservice/"/>
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </resource>
</api>
```
    
The service URL for the back-end REST service is:
    
` http://localhost:8280/jaxrs_basic/services/customers/customerservice/        `
    
The service URL for the REST API (ClientServiceAPI) is:
    
`         http://localhost:8280/clientservice/        `
    
As you can see, we have exposed the back-end REST service in a different context using this API. In addition to altering the context, the API is accepting the payload in a different format.
    
For the HTTP POST method, the back-end REST service accepts the payload in the following format:
    
``` html/xml
<Customer>
   <name>Jack</name>
</Customer>
```
    
The ClientServiceAPI accepts the payload in the following format:
    
``` html/xml
<addClient>
   <name>Jack</name>
</addClient>
```
    
For the HTTP PUT method, the back-end REST service accepts the payload in the following format:
    
``` html/xml
<Customer>
   <id>123</id>
   <name>John</name>
</Customer>
```
    
The ClientServiceAPI accepts the payload in the following format:
    
``` html/xml
<updateClient>
   <id>123</id>
   <name>John</name>
</updateClient>
```
    
Conversion of the payload format is done using the \<payloadFactory\> mediator.
    
We have now exposed a different REST API for the REST back-end services using a REST API in the ESB profile. In the ClientServiceAPI, there are two resources. The first resource is for the GET, OPTIONS, and DELETE HTTP methods. URL postfix is needed for the REST back-end service and is calculated using the following expression:
    
`fn:concat('/customers/',get-property('uri.var.id'))        `
    
The calculated URL postfix is appended to the back-end service URL, as we have set that value to the REST\_URL\_POSTFIX property. Therefore, the final request URL will be something similar to the following:
    
`http://localhost:8280/jaxrs_basic/services/customers/customerservice/customers/123                           `
    
### Executing the sample
    
Following is the cURL command to send a GET request to the API:
    
`curl -v http://localhost:8280/clientservice/clients/123`
    
You will see the following line printed as the response:
    
`<?xml version="1.0" encoding="UTF-8" standalone="yes"?><Customer><id>123</id><name>John</name></Customer>        `
    
The second resource is for the POST and PUT HTTP methods. The incoming REST request is filtered using the HTTP\_METHOD, and the appropriate payload required for the back-end REST service is constructed using the \<payloadFactory\> mediator.
    
Following is the cURL command to send a POST request to the API:
    
`curl -v -d @addClient.xml -H "Content-type: text/xml" http://localhost:8280/clientservice/clients`
    
Content of the addClient.xml file should be in the following format:
    
```
<addClient>
   <name>WSO2</name>
</addClient>
```
    
When you execute the above command in the application server console, the following line will be printed:
    
`----invoking addCustomer, Customer name is: WSO2`
    
Following is the cURL command to send a PUT request to the API:
    
`curl -v -d @updateClient.xml -X PUT -H "Content-type: text/xml" http://localhost:8280/clientservice/clients`
    
The content of the updateClient.xml file should be in the following format:
    
```
<updateClient>
    <id>123</id>
    <name>EI</name>        
</updateClient>
```

When you execute the above command in the application server console, the following line will be printed:
    
`----invoking updateCustomer, Customer name is: EI`