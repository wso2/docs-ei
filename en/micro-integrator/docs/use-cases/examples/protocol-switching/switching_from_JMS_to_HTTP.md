# Switching from JMS to HTTP(S)

This example demonstrates how the Micro Integrator receives a messages over the JMS transport and forwards it over an HTTP/S transport. In this sample, the client sends a request message to the proxy service exposed in JMS. The Micro Integrator forwards this message to the HTTP endpoint and returns the reply back to the client through a JMS temporary queue.

## Synapse configuration

Following are the integration artifacts (proxy service) that we can used to implement this scenario.

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse" name="JMStoHTTPStockQuoteProxy" transports="jms">
      <target>
          <inSequence>
              <property action="set" name="OUT_ONLY" value="true"/>
              <send>
                  <endpoint>
                      <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                  </endpoint>
              </send>
          </inSequence>
      </target>
      <parameter name="transport.jms.ContentType">
          <rules>
              <jmsProperty>contentType</jmsProperty>
              <default>text/xml</default>
          </rules>
      </parameter>
      <parameter name="transport.jms.Destination">Queue1</parameter>
      <parameter name="transport.jms.ConnectionFactory">myQueueListener</parameter>
  </proxy>
```

## Build and Run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.
5. Start the selected message broker and create a queue with name <strong>Queue1</strong>. 
6. [Configure MI with the selected message broker](../../../../setup/brokers/configure-with-ActiveMQ) and start the Micro-Integrator.

Set up the back-end service.

* Download the [stockquote_service.jar](
https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar)

* Run the mock service using the following command
```
$ java -jar stockquote_service.jar
```

Publish the following XML message to the Queue1.
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