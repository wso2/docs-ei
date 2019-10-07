# Store and Forward Using JMS Message Stores
See the examples given below.

## Example 1

In this example, the client sends requests to a **proxy service**, which stores the messages in a **JMS message store**. The **message forwarding processor** then picks the stored messages from the JMS message store and invokes the backend service.

![](attachments/119130313/119130315.png)  

### Synapse configurations

Shown below are the synapse artifacts that are used to define this use case.

```xml tab="Message Store"
<messageStore xmlns="http://ws.apache.org/ns/synapse" class="org.apache.synapse.message.store.impl.jms.JmsStore" name="JMSMS">
  <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
  <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
</messageStore>
```

```xml tab="Endpoint"
<endpoint name="SimpleStockQuoteService"> 
    <address uri="http://127.0.0.1:9000/services/SimpleStockQuoteService"/>
</endpoint>
```

```xml tab="Proxy Service"
<proxy name="Proxy1" transports="https http" startOnLoad="true" trace="disable">   
  <target>
    <inSequence>
      <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
      <property name="OUT_ONLY" value="true"/>
      <log level="full"/>
      <store messageStore="JMSMS"/>
    </inSequence>
  </target>
</proxy>
```

```xml tab="Message Processor"
<messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" name="Processor1" targetEndpoint="SimpleStockQuoteService" messageStore="JMSMS">
       <parameter name="max.delivery.attempts">4</parameter>
       <parameter name="interval">4000</parameter>
       <parameter name="is.active">true</parameter>
</messageProcessor>
```

See the descriptions of the above configurations:

<table>
  <tr>
    <th>Artifact</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Message Store</td>
    <td>
      Set the value of the the <code>java.naming.provider.url</code> property to point to the <code>jndi.properties</code>file. In this case, <code> store.jms.destination></code> is a mandatory parameter. If you are using the WSO2 Message Broker, you need to create a q ueue named 'JMSMS' using the Message Broker (i.e., the value you specify for the <code>store.jms.destination</code>.
    </td>
  </tr>
  <tr>
    <td>Endpoint</td>
    <td>
      Define an endpoint which is used to send the message to the back-end service.
    </td>
  </tr>
  <tr>
    <td>Proxy Service</td>
    <td>
      Create a proxy service which stores messages to the created Message Store. Note that you can use the FORCE_SC_ACCEPTED property in the message flow to send an Http 202 status to the client after the Micro Integrator accepts a message. If this property is not specified, the client that sends the request to the proxy service will timeout since it isbnot getting any response back from the proxy.
    </td>
  </tr>
  <tr>
    <td>Message Processor</td>
    <td>
      Create a message forwarding processor using the below configuration. Message forwarding processor consumes the messages stored in the message store.
    </td>
  </tr>
</table>

### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create integration artifacts with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........


Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.

Invoke the service:

```
ant stockquote -Daddurl=http://localhost:8280/services/Proxy1 -Dmode=placeorder
```

Note a message similar to the following example:  

``` java
SimpleStockQuoteService :: Accepted order for : 7482 stocks of IBM at $ 169.27205579038733
```

## Example 2

In the sample, when the message forwarding processor receives a response from the back-end service, it forwards it to a **replySequence** to process the response message.

![](attachments/119130313/119130314.png)

### Synapse configurations

Shown below are the synapse artifacts that are used to define this use case.

```xml tab="Message Store"
<messageStore xmlns="http://ws.apache.org/ns/synapse" class="org.apache.synapse.message.store.impl.jms.JmsStore" name="JMSMS">
  <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
  <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
</messageStore>
```

```xml tab="Endpoint"
<endpoint name="SimpleStockQuoteService">
  <address uri="http://127.0.0.1:9000/services/SimpleStockQuoteService"/>
</endpoint>
```

```xml tab="Proxy Service"
<proxy name="Proxy2" transports="https,http" statistics="disable" trace="disable" startOnLoad="true">
  <target>
    <inSequence>
      <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2" />
      <log level="full" />
      <store messageStore="JMSMS" />
    </inSequence>
  </target>
</proxy>
```

```xml tab="Sequence"
<sequence name="replySequence">
  <log level="full">
    <property name="REPLY" value="MESSAGE" />
  </log>
  <drop/>
</sequence>
```

```xml tab="Message Processor"
<messageProcessor name="Processor2" class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" targetEndpoint="SimpleStockQuoteService" messageStore="JMSMS" xmlns="http://ws.apache.org/ns/synapse">
  <parameter name="interval">1000</parameter>
  <parameter name="client.retry.interval">1000</parameter>
  <parameter name="max.delivery.attempts">4</parameter>
  <parameter name="message.processor.reply.sequence">replySequence</parameter>
  <parameter name="is.active">true</parameter>
  <parameter name="max.delivery.drop">Disabled</parameter>
  <parameter name="member.count">1</parameter>
</messageProcessor>
```

See the descriptions of the above configurations:

<table>
  <tr>
    <th>Artifact</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Message Store</td>
    <td>
      Set the value of the the <code>java.naming.provider.url</code> property to point to the <code>jndi.properties</code>file. In this case, <code> store.jms.destination></code> is a mandatory parameter. If you are using the WSO2 Message Broker, you need to create a q ueue named 'JMSMS' using the Message Broker (i.e., the value you specify for the <code>store.jms.destination</code>.
    </td>
  </tr>
  <tr>
    <td>Endpoint</td>
    <td>
      Define an endpoint which is used to send the message to the back-end service.
    </td>
  </tr>
  <tr>
    <td>Proxy Service</td>
    <td>
      Create a proxy service which stores messages to the created Message Store.
    </td>
  </tr>
  <tr>
    <td>Sequence</td>
    <td>
      Create a sequence to  handle  the response received from the back-end service.
    </td>
  </tr>
  <tr>
    <td>Message Processor</td>
    <td>
      Create a message forwarding processor using the below configuration. Message forwarding processor consumes the messages stored in the message store. Compared to [Example 1](#example-1), this has an additional parameter **message.processor.reply.sequence** to point to a sequence to handle the response message. 
    </td>
  </tr>
</table>

### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create a REST Api artifact with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........


Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.

Invoke the service:

```bash
ant stockquote -Daddurl=http://localhost:8280/services/Proxy2
```

Note a message similar to the following example printed in the Axis2 Server console.  

```bash
INFO - LogMediator To: /services/InOutProxy, WSAction: urn:getSimpleQuote, SOAPAction: urn:getSimpleQuote, MessageID: urn:uuid:dec12d9c-5289-476c-9d9a-b7bb7ebc7be4, Direction: request, REPLY = MESSAGE, Envelope:
     <?xml version='1.0' encoding='utf-8'?>
     <soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
     <soapenv:body><ns:getsimplequoteresponse xmlns:ns="http://services.samples">
     <ns:return xmlns:ax21="http://services.samples/xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ax21:GetQuoteResponse">
     <ax21:change>-2.3141856129298564</ax21:change><ax21:earnings>12.877155014054368</ax21:earnings>
     <ax21:high>172.73334579339183</ax21:high><ax21:last>165.31090559096748</ax21:last>
     <ax21:lasttradetimestamp>Thu Dec 29 07:48:42 IST 2011</ax21:lasttradetimestamp>
     <ax21:low>-164.80767926468306</ax21:low><ax21:marketcap>9451314.231029626</ax21:marketcap>
     <ax21:name>IBM Company</ax21:name><ax21:open>-161.41234152690964</ax21:open>
     
    <ax21:peratio>25.74977555860659</ax21:peratio><ax21:percentagechange>-1.2214036358135663</ax21:percentagechange>
    
    <ax21:prevclose>189.46935681818218</ax21:prevclose><ax21:symbol>IBM</ax21:symbol><ax21:volume>8611</ax21:volume>
     </ns:return></ns:getsimplequoteresponse></soapenv:body></soapenv:envelope>
```