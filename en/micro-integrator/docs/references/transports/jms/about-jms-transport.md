# JMS Transport

Java Message Service (JMS) is a widely used API in Java-based Message
Oriented Middleware(MOM) applications. It facilitates loosely coupled,
reliable, and asynchronous communication between different components of
a distributed application.

JMS supports two asynchronous communication models for messaging as
follows:

-   Point-to-point model - In this model message communication happens
    from one JMS client to another JMS client through a dedicated queue.
-   Publish and subscribe model -  In this model message communication
    happens from one JMS client(publisher) to many JMS
    clients(subscribers) through a topic.

JMS supports two models for messaging as follows:

-   Queues : point-to-point
-   Topics : publish and subscribe

!!! tip

The ESB Profile of WSO2 EI supports the following messaging features
introduced with JMS 2.0:

-   Shared Topic Subscription
-   JMSX Delivery Count
-   JMS Message Delivery Delay


The Java Message Service (JMS) transport in WSO2 Enterprise
Integrator(WSO2 EI) allows you to easily send and receive messages to
queues and topics of any JMS service that implements the JMS
specification.

The JMS transport implementation comes from the WS-Commons Transports
project, and it makes use of JNDI to connect to various JMS brokers. As
a result, WSO2 EI can work with any JMS broker that offers JNDI support.
All the relevant classes are packed into the
`         axis2-transport-jms-<version>.jar        ` and the classes
**org.apache.axis2.transport.jms.JMSListener** and
**org.apache.axis2.transport.jms.JMSSender** act as the transport
receiver and sender respectively.

The JMS transport implementation requires an active JMS server instance
to be able to receive and send messages. We recommend using [WSO2
Message Broker](http://wso2.com/products/message-broker/) or Apache
ActiveMQ, but other implementations such as Apache Qpid and Tibco are
also supported. For information on how to configure the JMS transport
with the most common broker servers that can be integrated with WSO2 EI,
see [Configuring the JMS Transport](_Configuring_the_JMS_Transport_) .

-   [JMS connection factory
    parameters](#JMSTransport-ConnectionFactoryParamsJMSconnectionfactoryparameters)
-   [Service level JMS configuration
    parameters](#JMSTransport-ServicelevelJMSconfigurationparameters)
-   [JMS MapMessage support](#JMSTransport-JMSMapMessagesupport)
    -   [Producing a MapMessage](#JMSTransport-ProducingaMapMessage)
    -   [Consuming a MapMessage](#JMSTransport-ConsumingaMapMessage)

### JMS connection factory parameters

Configuration parameters for the JMS receiver and the sender are XML
fragments that represent JMS connection factories. Following is a
typical JMS configuration that uses [WSO2 Message
Broker](http://wso2.com/products/message-broker/) as the message broker:

``` java
    <parameter name="myTopicConnectionFactory">
          <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
          <parameter name="java.naming.provider.url">conf/jndi.properties</parameter>
          <parameter name="transport.jms.ConnectionFactoryJNDIName">TopicConnectionFactory</parameter>
          <parameter name="transport.jms.ConnectionFactoryType">topic</parameter>
    </parameter>
```

This is a bare minimal JMS connection factory configuration that
consists of four connection factory parameters. The following table
describes each JMS connection factory parameter in detail:

!!! info

Tip

In transport parameter tables, literals displayed in italic mode under
the **Possible Values** column should be considered as fixed literal
constant values. Those values can be directly used in transport
configurations.


<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              java.naming.factory.initial             </code></p></td>
<td><p>JNDI initial context factory class. The class must implement the <code>              java.naming.spi.InitialContextFactory             </code> interface.</p></td>
<td><p>Yes</p></td>
<td><p>A valid class name</p></td>
<td><p>org.apache.activemq.jndi.ActiveMQInitialContextFactory</p></td>
</tr>
<tr class="even">
<td><p><code>              java.naming.provider.url             </code></p></td>
<td><p>URL of the JNDI provider.</p></td>
<td><p>Yes</p></td>
<td><p>A valid URL</p></td>
<td><p>tcp://localhost:61616</p></td>
</tr>
<tr class="odd">
<td><p><code>              java.naming.security.principal             </code></p></td>
<td><p>JNDI Username.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              java.naming.security.credentials             </code></p></td>
<td><p>JNDI password.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.Transactionality             </code></p></td>
<td><div class="content-wrapper">
<p>Preferred mode of transactionality.</p>
!!! note
<p>Note</p>
<p>In WSO2 EI, JMS transactions only work with either the Callout mediator or the Call mediator in blocking mode.</p>

</div></td>
<td><p>No</p></td>
<td><p><em>none:</em> Disables transactions in the JMS transport</p>
<p><em>local:</em> E nables local JMS session transactions</p>
<p><em>jta:</em> Enables global JTA transactions</p></td>
<td><p>none</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.UserTxnJNDIName             </code></p></td>
<td><p>JNDI name to be used to require user transaction.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p>java:comp/UserTransaction</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.CacheUserTxn             </code></p></td>
<td><p>Whether caching for user transactions should be enabled or not.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p><em>true</em></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.SessionTransacted             </code></p></td>
<td><p>Whether the JMS session should be transacted or not.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p><em>true</em> if transactionality is 'local'</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.SessionAcknowledgement             </code></p></td>
<td><p>JMS session acknowledgment mode.</p></td>
<td><p>No</p></td>
<td><ul>
<li><em>AUTO_ACKNOWLEDGE:</em> The session automatically acknowledges the consumer receipt of messages when message processing has finished.</li>
<li><em>CLIENT_ACKNOWLEDGE:</em> The consumer acknowledges all messages delivered so far by the session. If the consumer falls behind in its processing, a large number of unacknowledged messages can build up.</li>
<li><em>DUPS_OK_ACKNOWLEDGE:</em> The session lazily acknowledges the delivery of messages to the consumer. Lazy means that the consumer can delay acknowledgement to the server until a convenient time. During a delay, the server might redeliver messages. This mode reduces session overhead but the consumer can receive duplicate messages should JMS fail,</li>
<li><em>SESSION_TRANSACTED:</em> The session is a related group of consumed or produced messages that are treated as a single unit of work.</li>
</ul>
<p>Also see <a href="http://wso2.com/library/articles/2013/01/jms-message-delivery-reliability-acknowledgement-patterns/">JMS Message Delivery Reliability and Acknowledgement Patterns</a> .</p></td>
<td><p><em>AUTO_ACKNOWLEDGE</em></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.ConnectionFactoryJNDIName             </code></p></td>
<td><p>The JNDI name of the connection factory.</p></td>
<td><p>Yes</p></td>
<td><em>QueueConnectionFactory, TopicConnectionFactory</em></td>
<td><p>ConnectionFactory</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.ConnectionFactoryType             </code></p></td>
<td><p>Type of the connection factory.</p></td>
<td><p>No</p></td>
<td><p><em>queue, topic</em></p></td>
<td><p>queue</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.JMSSpecVersion             </code></p></td>
<td><p>JMS API version.</p></td>
<td><p>No</p></td>
<td><p><em>1.1, 1.0.2b</em></p></td>
<td><p><em>1.1</em></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.UserName             </code></p></td>
<td><p>The JMS connection username.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.Password             </code></p></td>
<td><p>The JMS connection password.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.Destination             </code></p></td>
<td><p>The JNDI name of the destination.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p>Defaults to service name</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.DestinationType             </code></p></td>
<td><p>Type of the destination.</p></td>
<td><p>No</p></td>
<td><p><em>queue, topic</em></p></td>
<td><p><em>queue</em></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.DefaultReplyDestination             </code></p></td>
<td><p>JNDI name of the default reply destination.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.                            DefaultReplyDestinationType             </code></p></td>
<td><p>Type of the reply destination.</p></td>
<td><p>No</p></td>
<td><p><em>queue, topic</em></p></td>
<td><p>Defaults to the type of the destination</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.MessageSelector             </code></p></td>
<td><p>Message selector implementation.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.SubscriptionDurable             </code></p></td>
<td><p>Whether the connection factory is subscription durable or not.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p><em>false</em></p></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.DurableSubscriberClientID            </code></td>
<td>The <code>             ClientId            </code> parameter when using durable subscriptions</td>
<td><p>Required if the value specified as <code>              transport.jms.                            SubscriptionDurable             </code> is <code>              true             </code> .</p></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.DurableSubscriberName             </code></p></td>
<td><p>The name of the durable subscriber.</p></td>
<td><p>Required if the value<br />
specified as <code>              transport.jms.                            SubscriptionDurable             </code> is <code>              true             </code> .</p></td>
<td><p><br />
</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.PubSubNoLocal             </code></p></td>
<td><p>Whether the messages should be published by the same connection they were received.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p><em>false</em></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.CacheLevel             </code></p></td>
<td><div class="content-wrapper">
<p>The cache level, with which JMS objects should be cached at start up. You can configure this in the <code>               &lt;EI_HOME&gt;/conf/axis2/axis2.xml              </code> file, if WSO2 EI acts as a producer.</p>
<p>Example:</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;endpoint&gt;</span>
<span id="cb1-2"><a href="#cb1-2"></a>   &lt;address uri=<span class="st">&quot;jms:/example.MyQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=repository/conf/jndi.properties&amp;transport.jms.DestinationType=queue&amp;transport.jms.CacheLevel=producer&quot;</span>/&gt;</span>
<span id="cb1-3"><a href="#cb1-3"></a>&lt;/endpoint&gt;</span></code></pre></div>
</div>
</div>
<p>Else, you can configure as a proxy service parameter, if WSO2 EI acts as a consumer. Following are the possible values for this parameter and the description of each:</p>
<ul>
<li><strong>none</strong> - None of the JMS objects will be cached.</li>
<li><strong>connection</strong> -  JMS connection objects will be cached.</li>
<li><strong>session</strong> -  JMS connection and session objects will be cached.</li>
<li><strong>consumer</strong> - JMS connection, session and consumer objects will be cached.</li>
<li><strong>producer</strong> - JMS connection, session and producer objects will be cached.</li>
<li><strong>auto</strong> - An appropriate cache level will be used based on the transaction strategy.</li>
</ul>
</div></td>
<td><p>No</p></td>
<td><p><em>none</em> , <em>connection</em> , <em>session</em> , <em>consumer</em> , <em>producer</em> , <em>auto</em></p></td>
<td><p><em>auto</em></p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.ReceiveTimeout             </code></p></td>
<td><p>Time to wait for a JMS message during polling. Set this parameter value to a negative integer to wait indefinitely. Set to zero to prevent waiting.</p></td>
<td><p>No</p></td>
<td><p>Number of milliseconds to wait</p></td>
<td><p>1000 ms</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.ConcurrentConsumers             </code></p></td>
<td><p>Number of concurrent threads to be started to consume messages when polling.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer - For topics this must be always 1</p></td>
<td><p>1</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.MaxConcurrentConsumers             </code></p></td>
<td><p>Maximum number of concurrent threads to use during polling.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer - For topics this must be always 1</p></td>
<td><p>1</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.IdleTaskLimit             </code></p></td>
<td><p>The number of idle runs per thread before it dies out.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer</p></td>
<td><p>10</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.MaxMessagesPerTask             </code></p></td>
<td><p>The maximum number of successful message receipts per thread.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer - Use -1 to indicate infinity</p></td>
<td><p>-1</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.InitialReconnectDuration             </code></p></td>
<td><p>Initial reconnection attempts duration in milliseconds.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer</p></td>
<td><p>10000 ms</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.ReconnectProgressFactor             </code></p></td>
<td><p>Factor by which the reconnection duration will be increased.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer</p></td>
<td><p>2</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.MaxReconnectDuration             </code></p></td>
<td><p>Maximum reconnection duration in milliseconds.</p></td>
<td><p>No</p></td>
<td><p><br />
</p></td>
<td><p>3600000 ms (1 hr)</p></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.ReconnectInterval            </code></td>
<td>Reconnection interval in milliseconds.</td>
<td>No</td>
<td><br />
</td>
<td>&gt;3600000 ms (1 hr)</td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.MaxJMSConnections             </code></p></td>
<td><p>Maximum cached JMS connections in the producer level.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer value</p></td>
<td><p>10</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.                            MaxConsumeErrorRetriesBeforeDelay             </code></p></td>
<td><p>Number of retries on consume errors before sleep delay kicks in.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer value</p></td>
<td><p>20</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.ConsumeErrorDelay             </code></p></td>
<td><p>Sleep delay when a consume error is encountered (in milliseconds).</p></td>
<td><p>No</p></td>
<td><p>Any positive integer value</p></td>
<td><p>100 ms</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.ConsumeErrorProgression             </code></p></td>
<td><p>Factor by which the consume error retry sleep will be increased.</p></td>
<td><p>No</p></td>
<td><p>Any positive integer value</p></td>
<td><p>2.0</p></td>
</tr>
<tr class="even">
<td><code>             transport.jms.MaxConsumeErrorRetryCount            </code></td>
<td><p>The maximum number of times the consumer should retry upon receiving a consumer error. You need to introduce this parameter only if the Broker has issues in notifying the Exception Listeners about the exceptions occurred .</p></td>
<td>No</td>
<td>Any positive integer value</td>
<td>-1</td>
</tr>
</tbody>
</table>

JMS transport implementation has some parameters that should be
configured at service level. For example, parameters that should be
configured in the service XML files of individual services.

### Service level JMS configuration parameters

Following are some of the common parameters you can configure at the
service level.

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              transport.jms.ConnectionFactory             </code></p></td>
<td><p>Name of the JMS connection factory the service should use.</p></td>
<td><p>No</p></td>
<td><p>A name of an already defined connection factory</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.PublishEPR             </code></p></td>
<td><p>JMS EPR to be published in the WSDL.</p></td>
<td><p>No</p></td>
<td><p>A JMS EPR</p></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.ContentType            </code></td>
<td>Specifies how the transport listener should determine the content type of received messages.</td>
<td>No</td>
<td>A simple string value, in which case the transport listener assumes that the received messages always have the specified content type, or a set of rules. For more information, see <a href="http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration">http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration</a> .</td>
</tr>
<tr class="even">
<td><div class="content-wrapper">
<code>              transport.jms.MessagePropertyHyphens             </code>
</div></td>
<td>Specifies the action to be taken when there are JMS Message property names that contain hyphens.</td>
<td>No</td>
<td><p><code>              none             </code> - No action will be taken. This is the default value.</p>
<p><code>              replace             </code> - Transport headers with hyphens will be replaced before adding them as JMS message properties, and if WSO2 EI is the consumer, hyphens will be reintroduced on message retrieval.</p>
<p><code>              delete             </code> - Transport headers with hyphens will be deleted.</p></td>
</tr>
</tbody>
</table>

For information on how to tune the JMS transport for better performance,
see [Tuning the JMS
Transport](https://docs.wso2.com/display/EI650/Tuning+the+JMS+Transport)
.

### JMS MapMessage support

In the ESB Profile of WSO2 Enterprise Integrator (WSO2 EI), the JMS
transport supports producing and consuming JMS
[MapMessage](http://docs.oracle.com/javaee/1.4/api/javax/jms/MapMessage.html)
objects, which send a set of name/value pairs.

#### Producing a MapMessage

To send a MapMessage from a proxy service or API to a queue, construct
an XML payload using the [PayloadFactor
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
(or another method) in the following structure, and send it to a JMS
endpoint:

``` java
    <JMSMap xmlns="http://axis.apache.org/axis2/java/transports/jms/map-payload">
        <name1>value1</name1>
        <name2>value2</name2>
        <name3>value3</name3>
    </JMSMap>
```

The JMS sender will then produce the equivalent MapMessage object:

``` java
    MapMessage message = session.createMapMessage();           
    message.setString("name1", "value1");
    message.setString("name2", "value2");
    message.setString("name3", "value3");
```

#### Consuming a MapMessage

When a proxy service receives a JMS MapMessage via a JMS broker, it will
convert it to an XML message like this:

``` java
    <JMSMap xmlns="http://axis.apache.org/axis2/java/transports/jms/map-payload">
        <name1>value1</name1>
        <name2>value2</name2>
        <name3>value3</name3>
    </JMSMap>
```
