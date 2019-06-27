# JMS Inbound Protocol

The JMS inbound protocol is a multi-tenant capable alternative to the
JMS transport. The JMS inbound protocol implementation requires an
active JMS server instance to be able to receive messages, and you need
to place the client JARs for your JMS server in the ESB profile
classpath.

This section provides information on using the Broker Profile of WSO2 EI
or Apache ActiveMQ as the JMS server, but other implementations such as
Apache Qpid and Tibco are also supported.

-   To configure the JMS inbound protocol with the Broker profile, see
    [Configuring the JMS Inbound Protocol with the Broker
    Profile](_Configuring_the_JMS_Inbound_Protocol_with_the_Broker_Profile_)
    .
-   To configure the JMS inbound protocol with ActiveMQ, see
    [Configuring the JMS Inbound Protocol with
    ActiveMQ](_Configuring_the_JMS_Inbound_Protocol_with_ActiveMQ_) .

Configuration parameters for a JMS inbound endpoint are XML fragments
that represent JMS connection factories.

Following is a sample JMS inbound endpoint configuration:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" 
                     name="jms" sequence="request" 
                     onError="fault" 
                     protocol="jms" 
                     suspend="false">
          <parameters>
             <parameter name="interval">1000</parameter>
             <parameter name="coordination">true</parameter> 
             <parameter name="transport.jms.Destination">ordersQueue</parameter>
             <parameter name="transport.jms.CacheLevel">3</parameter>
             <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
             <parameter name="sequential">true</parameter>
             <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
             <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>
             <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
             <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
          </parameters>
       </inboundEndpoint>
```

The parameters `         interval        ` and
`         coordination        ` are common to all polling inbound
endpoints. For descriptions of these parameters, see [Common polling
inbound endpoint
parameters](Polling-Inbound-Endpoints_119130486.html#PollingInboundEndpoints-CommonPolling)
.

### JMS inbound endpoint parameters

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
<td><p>The JNDI initial context factory class. The class must implement the <code>              java.naming.spi.InitialContextFactory             </code> interface.</p></td>
<td><p>Yes</p></td>
<td><p>A valid class name</p></td>
<td><p>-</p></td>
</tr>
<tr class="even">
<td><p><code>              java.naming.provider.url             </code></p></td>
<td><p>The URL of the JNDI provider.</p></td>
<td><p>Yes</p></td>
<td><p>A valid URL</p></td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.ConnectionFactoryJNDIName             </code></p></td>
<td><p>The JNDI name of the connection factory.</p></td>
<td><p>Yes</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
</tr>
<tr class="even">
<td><pre><code>sequential</code></pre></td>
<td>Whether the messages need to be polled and injected sequentially or not.</td>
<td>Yes</td>
<td><pre><code>true, false</code></pre></td>
<td><pre><code>true</code></pre></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.ConnectionFactoryType             </code></p></td>
<td><p>The type of the connection factory.</p></td>
<td><p>No</p></td>
<td><p><code>              queue             </code> , <em></em> <code>              topic             </code></p></td>
<td><p><code>              queue             </code></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.Destination             </code></p></td>
<td><p>The JNDI name of the destination.</p></td>
<td><p>No</p></td>
<td><p>-</p></td>
<td><p>The defaults value is the service name.</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.SessionAcknowledgement             </code></p></td>
<td><p>The JMS session acknowledgment mode.</p></td>
<td><p>No</p></td>
<td><p><code>              AUTO_ACKNOWLEDGE             </code> , <code>              CLIENT_ACKNOWLEDGE             </code> , <code>              DUPS_OK_ACKNOWLEDGE             </code> , <code>              SESSION_TRANSACTED             </code></p></td>
<td><p><code>              AUTO_ACKNOWLEDGE             </code></p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.CacheLevel             </code></p></td>
<td><p>The JMS resource cache level.</p></td>
<td><p>No</p></td>
<td><div class="content-wrapper">
<p><code>               0-               none               , 1-               connection               , 2-               session, 3-consumer              </code></p>
!!! info
<p>To subscribe to topics, set the value of <code>               transport.jms.CacheLevel              </code> to <code>               3              </code> .</p>

</div></td>
<td><p>0</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.UserName             </code></p></td>
<td><p>The JMS connection username.</p></td>
<td><p>No</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.Password             </code></p></td>
<td><p>The JMS connection password.</p></td>
<td><p>No</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.JMSSpecVersion             </code></p></td>
<td><p>The JMS API version.</p></td>
<td><p>No</p></td>
<td><p><code>              1.0.2b             </code> , <code>              1.1             </code> , <code>              2.0             </code></p></td>
<td><p><code>              1.1             </code></p></td>
</tr>
<tr class="even">
<td><code>             transport.jms.SubscriptionDurable            </code></td>
<td>Whether the connection factory is subscription durable or not.</td>
<td>No</td>
<td><code>             true            </code> , <code>             false            </code></td>
<td><code>             false            </code></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.DurableSubscriberClientID            </code></td>
<td>The <code>             ClientId            </code> parameter when using durable subscriptions.</td>
<td>Required if the value specified as <code>             transport.jms.SubscriptionDurable            </code> is <code>             true            </code> .</td>
<td>-</td>
<td>-</td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.DurableSubscriberName             </code></p></td>
<td><p>The name of the durable subscriber.</p></td>
<td><p>Required if the value specified as <code>              transport.jms.SubscriptionDurable             </code> is <code>              true             </code> .</p></td>
<td><p>-</p></td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.MessageSelector             </code></p></td>
<td><p>Message selector implementation.</p></td>
<td><p>No</p></td>
<td><p>-</p></td>
<td><p><br />
</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.ReceiveTimeout             </code></p></td>
<td>The time to wait for a JMS message during polling.<br />
Set this parameter value to a negative integer to wait indefinitely. Set it to zero to prevent waiting.</td>
<td>No</td>
<td>The number of milliseconds to wait.</td>
<td><code>             1            </code></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.ContentType            </code></td>
<td>How the inbound listener should determine the content type of received messages. Priority is always given to the JMS message type.</td>
<td>No</td>
<td>A simple string value, in which case the transport listener assumes that the received messages always have the specified content type, or a set of rules. For more information, see <a href="http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration">http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration</a> .</td>
<td>-</td>
</tr>
<tr class="even">
<td><code>             transport.jms.ContentTypeProperty            </code></td>
<td>Get the content type from message property.</td>
<td>No</td>
<td><code>             contentType            </code></td>
<td>-</td>
</tr>
<tr class="odd">
<td><code>             transport.jms.ReplyDestination            </code></td>
<td>The destination that the response generated by the back-end service is stored.</td>
<td>No</td>
<td>-</td>
<td>ReplyTo from the message</td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.PubSubNoLocal             </code></p></td>
<td><p>Whether messages should be published via the same connection that they were received.</p></td>
<td><p>No</p></td>
<td><p><code>              true, false             </code></p></td>
<td><p>false</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.jms.SharedSubscription             </code></p></td>
<td><p>If set to true, messages will be forwarded to only one of the consumers and consumers will share the messages that are published to the topic.</p></td>
<td><p>No</p></td>
<td><p><code>              true, false                           </code></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><code>              pinnedServers             </code></p></td>
<td><p>List of synapse server names separated by commas or spaces where this inbound endpoint should be deployed. If there is no pinned server list, the inbound endpoint configuration will be deployed in all server instances.</p></td>
<td><p>No</p></td>
<td><p>List of valid synapse server names</p></td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.ConcurrentConsumers            </code></td>
<td>Number of concurrent threads to be started to consume messages when polling.</td>
<td>No</td>
<td>Any positive integer.<br />
For topics this must always be 1.</td>
<td>1</td>
</tr>
<tr class="even">
<td><p><code>              transport.jms.retry.duration             </code></p></td>
<td><p>The retry interval to reconnect to the JMS server.</p></td>
<td><p>No</p></td>
<td>Retry interval in miliseconds.</td>
<td><p>-</p></td>
</tr>
<tr class="odd">
<td><code>             transport.jms.RetriesBeforeSuspension            </code></td>
<td>The number of consecutive mediation failures after which polling should be suspended.</td>
<td>No</td>
<td>Specify any positive numerical value based on your requirement. Polling will be suspended when the mediation failure count reaches the specified value.</td>
<td>-</td>
</tr>
<tr class="even">
<td><code>             transport.jms.PollingSuspensionPeriod            </code></td>
<td>The period of time that polling is to be suspended when the <code>             transport.jms.RetriesBeforeSuspension            </code> parameter is set.</td>
<td>No</td>
<td>Specify a required time period in milliseconds.</td>
<td>60000</td>
</tr>
</tbody>
</table>

### Samples

For a sample that demonstrates how one way message bridging from JMS to
HTTP can be done using the JMS inbound endpoint, see [Sample 901:
Inbound Endpoint JMS Protocol
Sample](https://docs.wso2.com/display/ESB500/Sample+901%3A+Inbound+Endpoint+JMS+Protocol+Sample)
.

## Configuring the JMS Inbound Protocol with ActiveMQ

This section describes how to configure the JMS inbound protocol with
ActiveMQ.

-   [Configuring with
    ActiveMQ](#ConfiguringtheJMSInboundProtocolwithActiveMQ-ConfiguringwithActiveMQ)
-   [Configuring the JMS inbound
    listener](#ConfiguringtheJMSInboundProtocolwithActiveMQ-ConfiguringtheJMSinboundlistener)
-   [Setting up delayed
    delivery](#ConfiguringtheJMSInboundProtocolwithActiveMQ-Settingupdelayeddelivery)
    -   [Setting up delayed delivery for
        topics](#ConfiguringtheJMSInboundProtocolwithActiveMQ-topicSettingupdelayeddeliveryfortopics)
    -   [Setting up delayed delivery for
        subscribers](#ConfiguringtheJMSInboundProtocolwithActiveMQ-Settingupdelayeddeliveryforsubscribers)
    -   [Enforcing a delay per
        message](#ConfiguringtheJMSInboundProtocolwithActiveMQ-Enforcingadelaypermessage)

### Configuring with ActiveMQ

Follow the instructions below to set up and configure Apache ActiveMQ as
the JMS server:

1.  Download and set up Apache ActiveMQ. For more information, see the
    [ActiveMQ getting started
    guide](http://activemq.apache.org/getting-started.html) .
2.  Set up WSO2 EI. For information on getting the WSO2 EI set up, see
    [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .

        !!! info
    
        Note
    
        ActiveMQ should be up and running before starting WSO2 EI.
    

3.  Copy the following client libraries from the
    `          <AMQ_HOME>/lib         ` directory to the
    `          <EI_HOME>/lib         ` directory.  

    **ActiveMQ 5.8.0 and above**

    -   `            activemq-broker-5.8.0.jar           `
    -   `            activemq-client-5.8.0.jar                       `
    -   `            geronimo-jms_1.1_spec-1.1.1.jar           `
    -   `            geronimo-j2ee-management_1.1_spec-1.0.1.jar           `
    -   `            hawtbuf-1.9.jar           `

    **Earlier version of ActiveMQ**

    -   `             activemq-core-5.5.1.jar            `

    -   `             geronimo-j2ee-management_1.0_spec-1.0.jar            `

    -   `             geronimo-jms_1.1_spec-1.1.1.jar            `

4.  Next,  configure the inbound listener in the ESB.

### Configuring the JMS inbound listener

Following is a sample JMS inbound listener configuration:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="jms" sequence="request" onError="fault" protocol="jms" suspend="false">
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

!!! info

Note

For details on the JMS configuration parameters used in the sample
configuration above, see [JMS Connection Factory
Parameters](https://docs.wso2.com/display/EI650/JMS+Transport#JMSTransport-ConnectionFactoryParams)
!!! info

The sample configuration above does not address the problem of transient
failures of the ActiveMQ message broker. For example, if we consider a
scenario where the ActiveMQ broker goes down for some reason and comes
back up after a while, the ESB profile of WSO2 EI will not reconnect to
ActiveMQ but instead it will throw errors when requests are sent to the
ESB profile until it is restarted.  
In order to tackle this issue you need to specify the following as the
`         java.naming.provider.url        ` parameter value *.*

failover:tcp://localhost:61616

Setting this as the value for the
`         java.naming.provider.url        ` parameter will make sure
that re-connection takes place when ActiveMQ is up and running. The
failover prefix is associated with the failover transport of ActiveMQ.
For more information, see [Failover
Transport](http://activemq.apache.org/failover-transport-reference.html)
.


Now you have an instances of ActiveMQ and the inbound endpoint of the
ESB profile configured, up and running.

### Setting up delayed delivery

This explains how you can set up delayed delivery in the broker.

#### Setting up delayed delivery for topics

When a topic has one or more subscriptions and when you need to apply a
global delay to all the subscriptions of it, you can set it up in the
broker by [defining global
policies](http://activemq.apache.org/message-redelivery-and-dlq-handling.html)
.

#### Setting up delayed delivery for subscribers

The client who connects can override the [global policies you set up for
topics](#ConfiguringtheJMSInboundProtocolwithActiveMQ-topic) . When
configuring the `         java.naming.provider.url        ` parameter in
the JMS Inbound Endpoint, you can pass the re-delivery policy property
as a query String. For a detailed description on the policy properties,
go to [Apache ActiveMQ
Documentation](http://activemq.apache.org/message-redelivery-and-dlq-handling.html)
.

For example, when configuring a Script mediator, if you need to rollback
the message four times upon an error with a one second delay, set the
`         java.naming.provider.url        ` property as follows.

**Script Mediator configuration**

``` powershell
    tcp://localhost:61616?jms.redeliveryPolicy.redeliveryDelay=1000&jms.redeliveryPolicy.maximumRedeliveries=4
```

The `         redeliveryDelay        ` property specifies the
re-delivery delay in milliseconds and the
`         maximumRedeliveries        ` property defines the number of
times the retry should occur. For more information on these properties,
go to [Apache ActiveMQ
Documentation](http://activemq.apache.org/redelivery-policy.html) .

!!! note

In ActiveMQ 5.9.0, although the broker properly identifies the
re-delivery times, the broker does not delay the messages as expected.
Thus, it will not enforce even the global level delay.


The final Inbound Endpoint configuration with the re-delivery delay and
number of times you defined will be as follows:

**Inbound Endpoint configuration**

``` xml
    <inboundEndpoint name="MARSInboundEP" onError="MARSEPErrorSeq"
            protocol="jms" sequence="ProcessOrderSeq" suspend="false">
            <parameters>
                    .......
                   <parameter name="java.naming.provider.url">tcp://localhost:61616?jms.redeliveryPolicy.redeliveryDelay=1000&amp;jms.redeliveryPolicy.maximumRedeliveries=4</parameter>
                     ......
            </parameters>
    </inboundEndpoint>
```

#### Enforcing a delay per message

There are not an out of the box feature available in ActiveMQ to enforce
delay per message. The re-delivery policies are defined by the Initial
Context Factory when it establishes the connection and it does not
happen on a per message basis.  
  
However, before rolling back, you can enforce a delay using the Script
mediator in the error sequence  as follows.

**Script Mediator configuration**

``` xml
    <script language="js">java.lang.Thread.sleep(10000);</script> 
```

Then, the sample sequence is as follows.

**Sample Sequence configuration**

``` xml
    <sequence name="MARSEPErrorSeq" xmlns="http://ws.apache.org/ns/synapse">
        <log level="full">
            <property expression="get-property('TxBID')" name="TxBID" xmlns:ns="http://org.apache.synapse/xsd"/>
            <property expression="get-property('MARSGUID')" name="MARSGUID" xmlns:ns="http://org.apache.synapse/xsd"/>
            <property expression="get-property('AppName')" name="AppName" xmlns:ns="http://org.apache.synapse/xsd"/>
            <property name="Method" value="MARSEPErrorSeq"/>
        </log>
        <property name="SET_ROLLBACK_ONLY" scope="default" type="STRING" value="true"/>
        <script language="js"><![CDATA[java.lang.Thread.sleep(10000);]]></script>
    </sequence>
```

The Script mediator holds the end of the sequence for the defined number
of times before it reaches the end. The `         rollback()        `
operation occurs once the sequence ends. Hence, this holds onto the
rollback operation for a specified period of time. Further, this
re-delivers the message only after the specified duration.

Therefore, in the above example, the
`         java.lang.Thread.sleep(10000)        ` property schedules the
message back after 10 seconds.

!!! note

There is no grantee that the re-scheduling will occur as soon as the
defined time period in the sleep(..) operation ends. Also, it considers
the delay of any re-deliveries. For example, if the re-delivery delay is
x seconds and the sleep time is y seconds, then the overall time for the
message to get re-delivered is x+y seconds.


Also you can use the filter mediator and switch mediator to do the
filtering as shown in the example below, when you need to define
re-delivery intervals based on the message error condition dynamically.

**Switch Mediator configuration**

``` xml
    <switch source="">
       <case regex="string">
          <script language="js"><![CDATA[java.lang.Thread.sleep(10000);]]></script>
       </case>
       <default>
          <script language="js"><![CDATA[java.lang.Thread.sleep(2000);]]></script>
       </default>
    </switch>
```

## Configuring the JMS Inbound Protocol with the Broker Profile

This section describes how to configure the JMS inbound protocol with
the Broker profile of WSO2 Enterprise Integrator (WSO2 EI).

1.  If you have not already done so, see [Installing the
    Product](https://docs.wso2.com/display/EI650/Installing+the+Product)
    for details on installing WSO2 EI.
2.  Open a command line terminal and start the broker profile that is
    shipped with your WSO2 EI distribution by running one of the
    following startup scripts from the
    `          <EI_HOME>/wso2/broker/bin         ` directory:
    -   On Linux/Mac OS:  sh wso2server.sh
    -   On Windows:  wso2server.bat --run

3.  Configure the JMS inbound listener. Following is a sample JMS
    inbound listener configuration:

    ``` html/xml
        <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                         name="jms_inbound"
                         sequence="request"
                         onError="fault"
                         protocol="jms"
                         suspend="false">
           <parameters>
              <parameter name="interval">1000</parameter>
              <parameter name="sequential">true</parameter>
              <parameter name="coordination">true</parameter>
              <parameter name="java.naming.factory.initial">org.wso2.andes.jndi.PropertiesFileInitialContextFactory</parameter>
              <parameter name="java.naming.provider.url">conf/jndi.properties</parameter>
              <parameter name="transport.jms.ConnectionFactoryJNDIName">QueueConnectionFactory</parameter>
              <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
              <parameter name="transport.jms.Destination">JMSMS</parameter>
              <parameter name="transport.jms.SessionTransacted">false</parameter>
              <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
              <parameter name="transport.jms.CacheLevel">1</parameter>
              <parameter name="transport.jms.SubscriptionDurable">false</parameter>
              <parameter name="transport.jms.SharedSubscription">false</parameter>
           </parameters>
        </inboundEndpoint>
    ```

        !!! info
    
        For more information on the JMS configuration parameters used in the
        code segments above, see [JMS Connection Factory
        Parameters](https://docs.wso2.com/display/EI650/JMS+Transport#JMSTransport-ConnectionFactoryParams)
        .
    

4.  Open `          <EI_HOME>/conf/jndi.properties         ` file and
    make a reference to the running EI-Broker runtime as specified
    below:  
    1.  Use **carbon** as the virtual host.

    2.  Define a queue named `            JMSMS           ` .
    3.  Comment out the topic, since it is not required in this
        scenario. However, in order to avoid getting the
        `             javax.naming.NameNotFoundException:TopicConnectionFactory            `
        exception during server startup, make a reference to the Broker
        profile from the
        `             TopicConnectionFactory            ` as well.

        For example:

        ``` java
                # register some connection factories
                # connectionfactory.[jndiname] = [ConnectionURL]
                connectionfactory.QueueConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
                connectionfactory.TopicConnectionFactory = amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5675'
                # register some queues in JNDI using the form
                # queue.[jndiName] = [physicalName]
                queue.JMSMS=JMSMS
                queue.StockQuotesQueue = StockQuotesQueue
        ```

5.  Ensure that Broker profile is running, and then open a command
    prompt (or a shell in Linux) and go to the
    `          <EI_HOME>/bin         ` directory.
6.  Start the Integration runtime server by executing  
    `          sh integrator.sh -Dqpid.dest_syntax=BURL         ` (on
    Linux/OS X) **or**  
    `          integrator.bat -Dqpid.dest_syntax=BURL         ` (on
    Windows).

Now you have an instance of Broker profile and a WSO2 EI inbound
endpoint configured, up and running.
