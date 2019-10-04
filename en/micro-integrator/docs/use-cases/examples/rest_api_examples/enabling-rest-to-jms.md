# Enabling REST to JMS
    
This section describes how an API can receive an HTTP message sent from a REST client and place it on a JMS queue. The API receives the HTTP message and sends it to a JMS queue that resides in a message broker.
    
### Install ActiveMQ
    
Install ActiveMQ server.
    
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
    
` curl -v -d @placeorder.xml -H "Content-type: application/xml" http://127.0.0.1:8290/jmsstockquote/order/        `
    
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
    
If you go to the ActiveMQ web console and inspect the SimpleStockQuoteService queue, you will see a message queued there.