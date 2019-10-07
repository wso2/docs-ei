# Consuming JMS Messages
This section describes how to configure WSO2 Micro Integrator to listen to a JMS Queue.

## Example 1: One-way messaging

In this example, the Micro Integrator listens to a JMS queue, consumes messages, and sends them to an HTTP back-end service.

### Synape configuration

Given below is the synapse configuration of the proxy service that mediates the above use case. Note that you need to update the JMS connection URL according to your broker as explained below.

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse" name="JMStoHTTPStockQuoteProxy" transports="jms">
    <target>
        <inSequence>
            <property action="set" name="OUT_ONLY" value="true"/>
        </inSequence>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <outSequence/>
    </target>
</proxy>
```

The Synapse artifacts used are explained below.

<table>
    <tr>
        <th>Artifact Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Proxy Service
        </td>
        <td>
            A proxy service is used to receive messages and to define the message flow. You can make the proxy service a JMS listener by setting its transport as <code>jms</code>. Once the JMS transport is enabled for a proxy service, the Micro Integrator listens on a JMS queue for the same name as the proxy service.</br>
            In the sample configuration above, the Micro Integrator listens to a JMS queue named <code>JMStoHTTPStockQuoteProxy</code>. To make the proxy service listen to a different JMS queue, define the <code>transport.jms.Destination</code> parameter with the name of the destination queue.
        </td>
    </tr>
    <tr>
        <td>Endpoint Mediator</td>
        <td>
           To send a message to the HTTP backend, you should define the connection URL as the endpoint address. 
        </td>
    </tr>
</table>

## Example 2: Two-way HTTP back-end call

In addition to one-way invocations, the proxy service can listen to the queue, pick up a message and do a two-way HTTP call as well. It allows the response to be delivered to a queue specified by the client. This is done by specifying a `ReplyDestination` element when placing a request message to a JMS queue.

### Synapse configuration

We can have a proxy service similar to the following to simulate a two-way invocation:

```xml
<proxy xmlns="http://ws.apache.org/ns/synapse"
           name="JMStoHTTPStockQuoteProxy1"
           transports="jms"
           startOnLoad="true"
           trace="disable">
       <description/>
       <target> 
          <inSequence>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence/>
       </target>
       <parameter name="transport.jms.ContentType">
          <rules>
             <jmsProperty>contentType</jmsProperty>
             <default>text/xml</default>
          </rules>
       </parameter>
</proxy>
```
The Synapse artifacts used are explained below.

<table>
    <tr>
        <th>Artifact Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Proxy Service
        </td>
        <td>
            A proxy service is used to receive messages and to define the message flow. You can make the proxy service a JMS listener by setting its transport as <code>jms</code>. Once the JMS transport is enabled for a proxy service, the Micro Integrator listens on a JMS queue for the same name as the proxy service.</br>
            In the sample configuration above, the Micro Integrator listens to a JMS queue named <code>JMStoHTTPStockQuoteProxy1</code>. To make the proxy service listen to a different JMS queue, define the <code>transport.jms.Destination</code> parameter with the name of the destination queue.
        </td>
    </tr>
    <tr>
        <td>Send Mediator</td>
        <td>
           To send a message to the HTTP backend, you should define the connection URL as the endpoint address. 
        </td>
    </tr>
</table>
    
## Example 3: Set content type of incoming JMS messages

By default, the Micro Integrator considers all messages consumed from a queue as SOAP messages. To consider messages consumed from a queue as a different format, define the **transport.jms.ContentType** parameter with the respective content type.

### Synapse configuration

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
        <outSequence/>
    </target>
    <parameter name="transport.jms.ContentType">
        <rules>
            <jmsProperty>contentType</jmsProperty>
            <default>application/xml</default>
        </rules>
    </parameter>
    <parameter name="transport.jms.Destination">MyJMSQueue</parameter>
</proxy>
```

The Synapse artifacts used are explained below.

<table>
    <tr>
        <th>Artifact Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
            Proxy Service
        </td>
        <td>
            You can specify a different content type within the <code>transport.jms.ContentType</code> parameter. In the sample configuration above, the content type defined is <code>application/xml</code>.
            If you want the proxy service to listen to a queue where the queue name is different from the proxy service name, you can specify the queue name using <code>transport.jms.Destination</code> parameter. In the sample configuration above, the Micro Integrator listens to a JMS queue named <b>MyJMSQueue</b>.
        </td>
    </tr>
    <tr>
        <td>Send Mediator</td>
        <td>
           To send a message to the HTTP backend, you should define the connection URL as the endpoint address. 
        </td>
    </tr>
</table>