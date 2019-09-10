# Sample 264: Sending Two-Way Messages Using JMS transport

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .

TEST  

**Objective** : Demonstrate sending request response scenario with JMS
transport.

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
   <proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy" transports="http">
       <target>
           <endpoint>
                   <address uri="jms:/SimpleStockQuoteService?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;
                 java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;
                 java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue"/>
           </endpoint>
           <inSequence>
               <property action="set" name="transport.jms.ContentTypeProperty" value="Content-Type" scope="axis2"/>
           </inSequence>
           <outSequence>
               <property action="remove" name="TRANSPORT_HEADERS" scope="axis2"/>
               <send/>
           </outSequence>
       </target>
       <publishWSDL uri="file:repository/conf/sample/resources/proxy/sample_proxy_1.wsdl"/>
   </proxy>
</definitions>
```

**Prerequisites:**

-   You need to set up ESB and axis2 server to use the JMS transport.
    See [Sample 251](_Sample_251_-_Switching_from_HTTP_S_to_JMS_) for
    more details.

This sample is similar to [Sample
251](_Sample_251_-_Switching_from_HTTP_S_to_JMS_) . Only difference is
we are expecting a response from the server. JMS transport uses
**transport.jms.ContentTypeProperty** to determine the content type of
the response message. If this property is not set JMS transport treats
the incoming message as plain text.

In the out path we remove the message context property
**TRANSPORT\_HEADERS** . If this property is not removed JMS headers
will be passed to the client.

Start Axis2 server with SimpleStockService deployed.

Invoke the stockquote client using the following command.

``` java
ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dsymbol=MSFT
```

The sample Axis2 server console will print a message indicating that it
has received the request:

``` java
Generating quote for : MSFT
```

In the client side it should print a message indicating it has received
the price.

``` java
Standard :: Stock price = $154.31851804993238
```
