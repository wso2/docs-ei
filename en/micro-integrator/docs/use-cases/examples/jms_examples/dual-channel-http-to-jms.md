# JMS Synchronous Invocations : Dual Channel HTTP-to-JMS

A JMS synchronous invocation takes place when a JMS producer receives a
response to a JMS request produced by it when invoked. The ESB Profile
of WSO2 Enterprise Integrator (WSO2 EI) uses an internal **JMS
correlation ID** to correlate the request and the response. See [JMS
Request/Reply
Example](http://www.eaipatterns.com/RequestReplyJmsExample.html) for
more information. JMS synchronous invocations are further explained in
the following use case.

![](attachments/119130318/119130319.png)

When the proxy service named `         SMSSenderProxy        ` receives
an HTTP request, it publishes that request in a JMS queue named
`         SMSStore        ` . Another proxy service named
`         SMSForwardProxy        ` subscribes to messages published in
this queue and forwards them to a back-end service named
`         SimpleStockQuoteService        ` . When this back-end service
returns an HTTP response, internal ESB logic is used to save that
message as a JMS message in a JMS queue named
`         SMSReceiveNotification        ` . The
`         SMSSenderProxy        ` proxy service picks the response
from the `         SMSReceiveNotification        ` queue and delivers it
to the client as an HTTP message using internal ESB logic.

**Note** that the `         SMSSenderProxy        ` proxy service is
able to pick up the message from the
`         SMSReceiveNotification        ` queue because the
`         transport.jms.ReplyDestination        ` parameter of the
`         SMSSenderProxy        ` proxy service is set to the same
`         SMSReceiveNotification        ` queue.

!!! info

The correlation between request and response:

Note that the message that is passed to the back-end service contains
the JMS message ID. However, the back-end service is required to return
the response using the JMS correlation ID. Therefore, the back-end
service should be configured to copy the message ID from the request
(the value of the **JMSMessageID** header) to the correlation ID of the
response (using the **JMSCorrelationID** header).


The following subsections explain how to execute this use case.

-   [Creating the JMS publisher and
    consumer](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-CreatingtheJMSpublisherandconsumer)
    -   [JMS publisher
        configuration](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-JMSpublisherconfiguration)
    -   [JMS consumer
        configuration](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-JMSconsumerconfiguration)
-   [Testing the use
    case](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-Testingtheusecase)
    -   [Setting up the ESB profile to communicate with the
        broker](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-SettinguptheESBprofiletocommunicatewiththebroker)
    -   [Starting the ESB and the
        Broker](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-StartingtheESBandtheBroker)
    -   [Starting theSimpleStockQuote
        service](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-StartingtheSimpleStockQuoteservice)
    -   [Invoking the JMS
        publisher](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-InvokingtheJMSpublisher)
    -   [Analyzing the
        output](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-Analyzingtheoutput)

### Creating the JMS publisher and consumer

Create two proxy services with the [JMS publisher
configuration](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-JMSpublisherconfiguration)
and [JMS consumer
configuration](#JMSSynchronousInvocations:DualChannelHTTP-to-JMS-JMSconsumerconfiguration)
given below and then deploy the proxy service artifacts in the ESB of
WSO2 EI. See [creating a proxy
service](https://docs.wso2.com/display/EI650/Creating+a+Proxy+Service)
for instructions.

#### JMS publisher configuration

Create a proxy service named `         SMSSenderProxy        ` with the
configuration given below.

**Sample proxy service**

``` xml
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

Listed below are some of the properties that can be used with the
**Property** mediator used in this proxy service:

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

The endpoint of this proxy service uses the properties listed below to
connect the proxy service to the JMS queue in the Message Broker
profile.

!!! tip

Note that there are two ways to define the endpoint URL:

-   Specify the JNDI name of the JMS queue and the [connection factory
    parameters](JMS-Transport_119130301.html#JMSTransport-ConnectionFactoryParams)
    in the connection URL as shown in the above example.

-   If you have already specified the endpoint's connection factory
    parameters (for the JMS sender configuration) in the
    `           axis2.xml          ` file (stored in the
    `           <EI_HOME>/conf/axis2/          ` directory), the
    connection URL in the proxy service should be as shown below. In
    this example, the endpoint URL of the proxy service refers the
    relevant connection factory in the axis2.xml file:

    ``` java
        jms:/SMSStore?transport.jms.ConnectionFactory=QueueConnectionFactory
    ```
    
    !!! note
    
    Be sure to replace the ' `         &        ` ' character in the
    endpoint URL with ' `         &amp;        ` ' to avoid the following
    exception:
    
``` java
    com.ctc.wstx.exc.WstxUnexpectedCharException: Unexpected character '=' (code 61); expected a semi-colon after the reference for entity 'java.naming.factory.initial' at [row,col {unknown-source}
```
    

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
<td>The destination in which the request received by the proxy service is stored.</td>
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

Since this is a two-way invocation, the
[OUT\_ONLY](https://docs.wso2.com/display/EI650/Generic+Properties#GenericProperties-OutOnly)
property is not set in the In sequence.

#### JMS consumer configuration

Create a proxy service named `         SMSForwardProxy        ` with the
configuration given below. This proxy service will consume messages from
the `         SMSStore        ` queue of the Message Broker Profile, and
forward the messages to the back-end service.

``` xml
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

The `         transport.jms.ConnectionFactory        ` ,
`         transport.jms.DestinationType        ` parameter and the
`         transport.jms.Destination properties        ` parameter map
the proxy service to the `         SMSStore        ` queue.

The `         SimpleStockQuoteService        ` sample shipped with WSO2
EI is used as the back-end service in this example. To invoke this
service, the address URI of this proxy service is defined as
`         http://localhost:9000/services/SimpleStockQuoteServic        `
e.

### Testing the use case

Follow the steps given below to try out the above use case.

#### Setting up the ESB profile to communicate with the broker

Follow the steps below to enable the JMS transport of the ESB Profile to
communicate with the Message Broker profile:

1.  Edit the `           <EI_HOME>/conf/axis2/axis2.xml          ` file,
    find the commented `           <transport receiver>          `
    block and uncomment it as follows:

    ``` html/xml
             <!--Uncomment this and configure as appropriate for JMS transport support with WSO2 MB 2.x.x -->
               <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                   <parameter name="myTopicConnectionFactory" locked="false">
                      <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                       <parameter name="java.naming.provider.url" locked="false">conf/jndi.properties</parameter>
                       <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">TopicConnectionFactory</parameter>
                       <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                   </parameter>
             
                   <parameter name="myQueueConnectionFactory" locked="false">
                       <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                       <parameter name="java.naming.provider.url" locked="false">conf/jndi.properties</parameter>
                       <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                      <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                   </parameter>
             
                   <parameter name="default" locked="false">
                       <parameter name="java.naming.factory.initial" locked="false">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
                       <parameter name="java.naming.provider.url" locked="false">conf/jndi.properties</parameter>
                       <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
                       <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                   </parameter>
               </transportReceiver>
    ```

2.  Uncomment the following `           <transport sender>          `
    block for JMS in the same file:

    ``` html/xml
            <!-- uncomment this and configure to use connection pools for sending messages-->
            <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender"/>
    ```

        !!! info
    
        For more information on the JMS configuration parameters used in the
        code segments above, see [JMS Connection Factory
        Parameters](JMS-Transport_119130301.html#JMSTransport-JMSConnectionFactoryParameters)
        .
    

3.  Open the `           <EI_HOME>/conf/jndi.properties          ` file
    and update the connection factories and queues as follows:

    ``` java
        # register some connection factories
        # connectionfactory.[jndiname] = [ConnectionURL]
        connectionfactory.QueueConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
    
        # register some queues in JNDI using the form
        # queue.[jndiName] = [physicalName]
        queue.SMSStore=SMSStore
        queue.SMSReceiveNotificationStore=SMSReceiveNotificationStore
    ```

#### Starting the ESB and the Broker

1.  Start the Message Broker Profile. For information on how to start
    the Message Broker Profile, see [Starting the
    Message Broker Profile](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-EI_Broker)
    .
2.  Start the ESB Profile. For information on how to start the ESB
    Profile, see [Starting the
    ESB Profile](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-ESB)
    .

#### Starting theSimpleStockQuote service

Follow the steps below to start the Axis2 server:

1.  Open a command prompt (or a shell in Linux) and go to the
    `          <EI_HOME>/samples/axis2Server         ` directory.
2.  Execute one of the following commands  
    -   **On Windows** :
        `            axis2server.bat                       `
    -   **On Linux/Solaris** : `            ./axis2server.sh           `
3.  Deploy the sample back-end service. Follow the steps below to build
    and deploy the `          SimpleStockQuoteService         ` :
    1.  Open a command prompt (or a shell in Linux) and go to the
        `            <EI_HOME>/samples/axis2Server/src/SimpleStockQuoteService           `
        directory.
    2.  Run `             ant            ` .

#### Invoking the JMS publisher

Execute the following command from the
`         <EI_HOME>/samples/axis2Client        ` directory to invoke the
`         SMSSenderProxy        ` proxy service that you defined as the
JMS publisher:

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/SMSSenderProxy -Dsymbol=IBM
```

#### Analyzing the output

Analyze the output on the Axis2 server console as well as the output on
the client console to understand how a JMS producer can receive a
response to a JMS request produced by it.

You will see the following on the Axis2 server console:

``` java
    Fri Dec 08 11:19:29 IST 2017 samples.services.SimpleStockQuoteService :: Generating quote for : IBM
```

You will see the following response on the client console:

``` java
    Standard :: Stock price = $162.04696182786148
```
