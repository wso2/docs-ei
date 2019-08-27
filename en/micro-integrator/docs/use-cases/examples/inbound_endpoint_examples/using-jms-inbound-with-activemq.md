# Using the JMS Inbound Protocol with Active MQ

This section describes how to configure the JMS inbound protocol with
ActiveMQ.

### Configuring with ActiveMQ

Follow the instructions below to set up and configure Apache ActiveMQ as
the JMS server:

1.  Download and set up Apache ActiveMQ. For more information, see the
    [ActiveMQ getting started
    guide](http://activemq.apache.org/getting-started.html) .
2.  Set up WSO2 EI. For information on getting the WSO2 EI set up, see
    [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .

    !!! Info
        ActiveMQ should be up and running before starting WSO2 EI.

3.  Copy the following client libraries from the
    `<AMQ_HOME>/lib` directory to the
    `<EI_HOME>/lib` directory.  

    **ActiveMQ 5.8.0 and above**

    -   `activemq-broker-5.8.0.jar`
    -   `activemq-client-5.8.0.jar`
    -   `geronimo-jms_1.1_spec-1.1.1.jar`
    -   `geronimo-j2ee-management_1.1_spec-1.0.1.jar`
    -   `hawtbuf-1.9.jar`

    **Earlier version of ActiveMQ**

    -   `activemq-core-5.5.1.jar`
    -   `geronimo-j2ee-management_1.0_spec-1.0.jar`
    -   `geronimo-jms_1.1_spec-1.1.1.jar`

4.  Next,  configure the inbound listener in the ESB.

### Configuring the JMS inbound listener

Following is a sample JMS inbound listener configuration:

```
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

!!! Info
    For details on the JMS configuration parameters used in the sample configuration above, see [JMS Connection Factory Parameters](https://docs.wso2.com/display/EI650/JMS+Transport#JMSTransport-ConnectionFactoryParams).

!!! Info
    The sample configuration above does not address the problem of transient failures of the ActiveMQ message broker. For example, if we consider a scenario where the ActiveMQ broker goes down for some reason and comes back up after a while, the ESB profile of WSO2 EI will not reconnect to ActiveMQ but instead it will throw errors when requests are sent to the ESB profile until it is restarted. In order to tackle this issue you need to specify the following as the `java.naming.provider.url` parameter value *.* failover:tcp://localhost:61616 
    Setting this as the value for the `java.naming.provider.url` parameter will make sure that re-connection takes place when ActiveMQ is up and running. The failover prefix is associated with the failover transport of ActiveMQ. For more information, see [Failover Transport](http://activemq.apache.org/failover-transport-reference.html).


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

!!! Note
    In ActiveMQ 5.9.0, although the broker properly identifies the re-delivery times, the broker does not delay the messages as expected. Thus, it will not enforce even the global level delay.

The final Inbound Endpoint configuration with the re-delivery delay and number of times you defined will be as follows:

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

!!! Note
    There is no grantee that the re-scheduling will occur as soon as the defined time period in the sleep(..) operation ends. Also, it considers the delay of any re-deliveries. For example, if the re-delivery delay is x seconds and the sleep time is y seconds, then the overall time for the message to get re-delivered is x+y seconds.

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