# Switching from HTTP(S) to JMS

This example demonstrates how WSO2 Micro Integrator receives messages in HTTP and passes the messages through JMS.

This Micro Integrator configuration creates a Proxy Service Samples over HTTP and forwards received messages to the above EPR using JMS, and immediately responds with a 202. 

If the previous example on [JMS to HTTP](../switching_from_JMS_to_HTTP) is also configured, it will pick the message from queue and send it to the stockquote proxy.

## Synapse configuration

Following are the integration artifacts (proxy service) that we can used to implement this scenario.

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse" name="HTTPtoJMSStockQuoteProxy" transports="http">
    <target>
        <endpoint>
            <address uri="jms:/Queue1?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=tcp://localhost:61616&amp;transport.jms.DestinationType=queue"/>
        </endpoint>
        <inSequence>
            <property action="set" name="OUT_ONLY" value="true"/>
            <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL key="conf:HTTP_JMS/sample_proxy_1.wsdl" preservePolicy="true"/>
</proxy>
```
Example JMS connection URL for WSO2 MB
```
jms:/Queue1?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=conf/jndi.properties&amp;transport.jms.DestinationType=queue
```
## Build and Run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Add [sample_proxy_1.wsdl](https://github.com/wso2-docs/WSO2_EI/blob/master/samples-protocol-switching/sample_proxy_1.wsdl) as a [registry resource](../../../../develop/creating-artifacts/creating-registry-resources) (change the registry path of the proxy accordingly). 
4. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.
6. [Configure MI with the selected message broker](../../../../setup/brokers/configure-with-ActiveMQ) and start the Micro-Integrator.

Invoke the HTTPtoJMSStockQuoteProxy with the following payload (using SOAP UI or CURL)
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Header/>
   <soapenv:Body>
   	<m0:placeOrder xmlns:m0="http://services.samples">
	    <m0:order>
	        <m0:price>172.23182849731984</m0:price>
	        <m0:quantity>18398</m0:quantity>
	        <m0:symbol>IBM</m0:symbol>
	    </m0:order>
	</m0:placeOrder>
   </soapenv:Body>
</soapenv:Envelope>
```
Sample CURL
```
curl -X POST \
  http://localhost:8290/services/HTTPtoJMSStockQuoteProxy.HTTPtoJMSStockQuoteProxyHttpSoap11Endpoint \
  -H 'cache-control: no-cache' \
  -H 'content-type: text/xml' \
  -H 'soapaction: \"urn:placeOrder\"' \
  -d '<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Header/>
   <soapenv:Body>
   	<m0:placeOrder xmlns:m0="http://services.samples">
	    <m0:order>
	        <m0:price>172.23182849731984</m0:price>
	        <m0:quantity>18398</m0:quantity>
	        <m0:symbol>IBM</m0:symbol>
	    </m0:order>
	</m0:placeOrder>
   </soapenv:Body>
</soapenv:Envelope>'
```

Now message count in the queue should be increased or if the JMS listener is also setup, it should pick the message from queue and send to the stockquote proxy.