# JMS Synchronous Invocations : Dual Channel HTTP-to-JMS
A JMS synchronous invocation takes place when a JMS producer receives a response to a JMS request produced by it when invoked. The WSO2 Micro Integrator uses an internal **JMS correlation ID** to correlate the request and the response. See [JMSRequest/ReplyExample](http://www.eaipatterns.com/RequestReplyJmsExample.html) for more information. JMS synchronous invocations are further explained in the following use case.

![](attachments/119130318/119130319.png)

When the proxy service named `         SMSSenderProxy        ` receives an HTTP request, it publishes that request in a JMS queue named `         SMSStore        ` . Another proxy service named `         SMSForwardProxy        ` subscribes to messages published in this queue and forwards them to a back-end service named `         SimpleStockQuoteService        ` . When this back-end service returns an HTTP response, internal ESB logic is used to save that
message as a JMS message in a JMS queue named `         SMSReceiveNotification        `. The `         SMSSenderProxy        ` proxy service picks the response from the `         SMSReceiveNotification        ` queue and delivers it to the client as an HTTP message using internal ESB logic.

**Note** that the `         SMSSenderProxy        ` proxy service is able to pick up the message from the `         SMSReceiveNotification        ` queue because the `         transport.jms.ReplyDestination        ` parameter of the `         SMSSenderProxy        ` proxy service is set to the same `         SMSReceiveNotification        ` queue.

!!! info
    The correlation between request and response:
    
    Note that the message that is passed to the back-end service contains the JMS message ID. However, the back-end service is required to return the response using the JMS correlation ID. Therefore, the back-end service should be configured to copy the message ID from the request (the value of the **JMSMessageID** header) to the correlation ID of the response (using the **JMSCorrelationID** header).

## Synapse configurations

Create two proxy services with the JMS publisher configuration and JMS consumer configuration given below and then deploy the proxy service artifacts in the Micro Integrator.

### JMS publisher configuration

Shown below is the `         SMSSenderProxy        ` proxy service.

```
<proxy xmlns="http://ws.apache.org/ns/synapse"
           name="SMSSenderProxy"
           transports="https,http"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <property name="transport.jms.ContentTypeProperty"
                       value="Content-Type"
                       scope="axis2"/>
          </inSequence>
          <outSequence>
             <property name="TRANSPORT_HEADERS" scope="axis2" action="remove"/>
             <send/>
          </outSequence>
          <endpoint>
             <address uri="jms:/SMSStore?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=conf/jndi.properties&amp;transport.jms.DestinationType=queue&amp;transport.jms.ReplyDestination=SMSReceiveNotificationStore"/>
          </endpoint>
       </target>
       <description/>
</proxy>
```

Listed below are some of the properties that can be used with the **Property** mediator used in this proxy service:

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 76%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>TRANSPORT_HEADERS</code></pre></td>
<td><p>This property is used in the out sequence to make sure that transport headers (which are JMS headers in this example) are removed from the message when it is passed to the back-end client.</p>
<p>It is recommended to set this property because (according to the JMS specification) a property name can contain any character for which the <code>              Character.isJavaIdentifierPart             </code> Java method returns 'true'. Therefore when there are headers that contain special characters (e.g accept-encoding), some JMS brokers will give errors.</p></td>
</tr>
<tr class="even">
<td><pre><code>transport.jms.ContentTypeProperty</code></pre></td>
<td><p>The JMS transport uses this property in the above configuration to determine the content type of the response message. If this property is not set, the JMS transport treats the incoming message as plain text.</p>
<p><strong>Note:</strong> When this property is used, the content type is determined by the out transport. For example, if the proxy service/API is sending a request, the endpoint reference will determine the content type. Also, if the proxy service/API is sending the response back to the client, the configuration of the service/API will determine the content type.</p></td>
</tr>
<tr class="odd">
<td><pre><code>JMS_WAIT_REPLY</code></pre></td>
<td><div class="content-wrapper">
<p>This property can be used to specify how long the system should wait for the JMS queue (SMSRecieveNotification queue) to send the response back. You can add this property to the in sequence as shown below.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>&lt;property name=<span class="st">&quot;JMS_WAIT_REPLY&quot;</span> value=<span class="st">&quot;60000&quot;</span> scope=<span class="st">&quot;axis2&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
<tr class="even">
<td><p><code>              JMS_TIME_TO_LIVE             </code></p></td>
<td><div class="content-wrapper">
<p>This property can be set in the InSequence of the proxy service to specify the maximum time period for which a message can live without being consumed.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb5" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb5-1"><a href="#cb5-1"></a>&lt;property name=<span class="st">&quot;JMS_TIME_TO_LIVE&quot;</span> scope=<span class="st">&quot;axis2&quot;</span> value=<span class="st">&quot;20000&quot;</span>/&gt;</span></code></pre></div>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

The endpoint of this proxy service uses the properties listed below to connect the proxy service to the JMS queue in the Message Broker.

<table>
<thead>
<tr class="header">
<th>Property</th>
<th>Value for this use case</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>address uri</strong></p></td>
<td><p><code>              jms:/SMSStore             </code></p></td>
<td>The destination in which the request received by the proxy service is stored. Note that there are two ways to define the URL: </br>
  <ul>
    <li>
      Specify the JNDI name of the JMS queue and the connection factory parameters in the connection URL as shown in the above example.
    </li>
    <li>
      If you have already specified the endpoint's connection factory parameters (for the JMS sender configuration) in the axis2.xml file, the connection URL in the proxy service should be as shown below. In this example, the endpoint URL of the proxy service refers the relevant connection factory in the axis2.xml file:
      <code>
        jms:/SMSStore?transport.jms.ConnectionFactory=QueueConnectionFactory
      </code>
    </li>
  </ul>
</td>
</tr>
<tr class="even">
<td><p><strong>java.naming.factory.initial</strong></p></td>
<td><p><code>              org.wso2.andes.jndi.PropertiesFileInitialContextFactory             </code></p></td>
<td><div class="itemizedlist">
<p>The initial context factory to use.<br />
The value specified here should be the same as that specified in <code>               &lt;EI_HOME&gt;/conf/axis2/axis2.xml              </code> for the JMS transport receiver.</p>
</div></td>
</tr>
<tr class="odd">
<td><strong>java.naming.provider.url</strong></td>
<td><p><code>              conf/jndi.properties             </code></p></td>
<td>The location of the JNDI service provider.</td>
</tr>
<tr class="even">
<td><p><strong>transport.jms.DestinationType</strong></p></td>
<td><code>             queue            </code></td>
<td>The destination type of the JMS message that will be generated by the proxy service.</td>
</tr>
<tr class="odd">
<td><strong>transport.jms.ReplyDestination</strong></td>
<td><p><code>              SMSReceiveNotificationStore             </code></p></td>
<td>The destination in which the response generated by the back-end service is stored.</td>
</tr>
</tbody>
</table>

### JMS consumer configuration

Create a proxy service named `         SMSForwardProxy        ` with the configuration given below. This proxy service will consume messages from the `         SMSStore        ` queue of the Message Broker Profile, and forward the messages to the back-end service.

```
<proxy xmlns="http://ws.apache.org/ns/synapse"
           name="SMSForwardProxy"
           transports="jms"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
       </target>
       <parameter name="transport.jms.ContentType">
          <rules>
             <jmsProperty>contentType</jmsProperty>
             <default>text/xml</default>
          </rules>
       </parameter>
       <parameter name="transport.jms.ConnectionFactory">myQueueConnectionFactory</parameter>
       <parameter name="transport.jms.DestinationType">queue</parameter>
       <parameter name="transport.jms.Destination">SMSStore</parameter>
       <description/>
</proxy>
```

The `         transport.jms.ConnectionFactory        ` , `         transport.jms.DestinationType        ` parameter and the
`         transport.jms.Destination properties        ` parameter map the proxy service to the `         SMSStore        ` queue.

The `         SimpleStockQuoteService        ` sample shipped with WSO2 Micro Integrator is used as the back-end service in this example. To invoke this service, the address URI of this proxy service is defined as `         http://localhost:9000/services/SimpleStockQuoteServic        `.

## Run the Example

1. Configure the Micro Integrator with Apache ActiveMQ and set up the JMS Sender.
2. Start WSO2 Integration Studio and create a proxy service with the above configuration. You can copy the synapse configuration given above to the **Source View** of your proxy service.
3. To test this scenario you need an HTTP back-end service. Deploy the SimpleStockQuoteService and start the Axis2 server.
3. Send a message to the Micro Integrator by executing the following command from the `MI_HOME/samples/axis2Client` folder.

    ```
    ant stockquote -Daddurl=http://localhost:8280/services/SMSSenderProxy -Dsymbol=IBM
    ```

    !!! Info
        You can view the ActiveMQ queue by accessing the ActiveMQ management console using the URL `http://0.0.0.0:8161/admi`and using `admin` as both the username and password.

Analyze the output on the Axis2 server console as well as the output on the client console to understand how a JMS producer can receive a response to a JMS request produced by it.

You will see the following on the Axis2 server console:

``` java
Fri Dec 08 11:19:29 IST 2017 samples.services.SimpleStockQuoteService :: Generating quote for : IBM
```

You will see the following response on the client console:

``` java
Standard :: Stock price = $162.04696182786148
```
