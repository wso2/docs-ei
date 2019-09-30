# Inbound Endpoint JMS Protocol Sample
## Example use case

This sample demonstrates how one way message bridging from JMS to HTTP can be done using the inbound JMS endpoint.

## Synapse configuration

Following are the integration artifacts that we can used to implement this scenario.

```xml tab='Registry Resource'
 <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
    <parameter name="cachableDuration">15000</parameter>
 </registry>
```

```xml tab='Scheduled Task'
<taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager">
  <parameter name="cachableDuration">15000</parameter>
</taskManager>
```

```xml tab='Inbound Endpoint'
 <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="jms_inbound" sequence="request" onError="fault" protocol="jms" suspend="false">
    <parameters>
       <parameter name="interval">1000</parameter>
       <parameter name="transport.jms.Destination">ordersQueue</parameter>
       <parameter name="transport.jms.CacheLevel">1</parameter>
       <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
       <parameter name="sequential">true</parameter>
       <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
       <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
       <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
       <parameter name="transport.jms.SessionTransacted">false</parameter>
       <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
    </parameters>
 </inboundEndpoint>
```

```xml tab='Fault Sequence'
<sequence name="request" onError="fault">
  <call>
     <endpoint>
        <address format="soap12" uri="http://localhost:9000/services/SimpleStockQuoteService"/>
     </endpoint>
  </call>
  <drop/>
</sequence>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the following artifacts: Proxy service, registry resource, scheduled task, inbound endpoint, fault sequence.
4. Deploy the artifacts in your Micro Integrator.

Configure the ActiveMQ broker.

Set up the back-end service.

Invoke the proxy service:

1. Log on to the ActiveMQ console using the <http://localhost:8161/admin> url.
2. Browse the queue `ordersQueue` listening via the above endpoint.
3. Add a new message with the following content to the queue:

    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing">
        <soapenv:Body>
            <m0:getQuote xmlns:m0="http://services.samples"> 
                <m0:request>
                    <m0:symbol>IBM</m0:symbol>
                </m0:request>
            </m0:getQuote>
        </soapenv:Body>
    </soapenv:Envelope>
    ```

You will see that the JMS endpoint gets the message from the queue and sends it to the stock quote service.
