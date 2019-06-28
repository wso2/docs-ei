# Tuning the JMS Transport

The Java Message Service (JMS) transport of the WSO2 Enterprise
Integrator (WSO2 EI) allows you to easily send and receive messages to
queues and topics of any JMS service that implements the JMS
specification. The following sections describe how you can tune the JMS
transport of WSO2 EI for better performance.

### Increase the maximum number of JMS proxies

If you create several JMS proxy services in WSO2 EI, you will see a
message similar to the following:

`         WARN - JMSListener Polling tasks on destination : JMStoHTTPStockQuoteProxy18 of type queue for service JMStoHTTPStockQuoteProxy18 have not yet started after 3 seconds ..        `

This issue occurs when you do not have enough threads available to
consume messages from JMS queues. The maximum number of concurrent
consumers (that is, the number of JMS proxies) that can be deployed is
limited by the base transport worker pool that is used by the JMS
transport. You can configure the size of this worker pool using the
system properties `         lst_t_core        ` and
`         lst_t_max        ` . Note that increasing these values will
also increase the memory consumption, because the worker pool will
allocate more resources.

Similarly, you can configure the current number and the anticipated
number of outbound JMS proxies using the system properties
`         snd_t_core        ` and `         snd_t_max        ` .

To adjust the values of these properties, you can modify the server
startup script if you want to increase the available threads for NHTTP
and JMS transports (requires more memory), or create a
`         jms.properties        ` file if you want to increase the
available threads just for the JMS transport. Both approaches are
described below.

##### To increase the threads for NHTTP and JMS transports

1.  Open the integrator.sh or integrator.bat file in your
    `          <EI_HOME>/bin         ` directory for editing.
2.  Change the values of the properties as follows:
    > If you do not have the following properties in the
        `           integrator.sh          ` or
        `           integrator.bat          ` files, add them with the given
        values.

    -   `                           -Dlst_t_core=200                           `

       > The defined values is applied as a System Property.


    -   `            -Dlst_t_max=250           `
        `                       `
    -   `            -Dsnd_t_core=200           `
    -   `            -Dsnd_t_max=250           `

##### To increase the threads for just the JMS transport

1.  Create a file named `          jms.properties         ` with the
    following properties:
    -   `             lst_t_core=200            `

    -   `             lst_t_max=250            `

    -   `             snd_t_core=200            `

    -   `             snd_t_max=250            `

2.  Create a directory called `          conf         ` under your
    `          <EI_HOME>         ` directory and save the file in this
    directory.

### Configuring JMS Listener

You can increase the JMS listener performance by

1.  [Using concurrent
    consumers](#TuningtheJMSTransport-concurrentConsumers)
2.  [Enabling caching](_Tuning_the_JMS_Transport_)

#### Using concurrent consumers

Concurrent consumers is the minimum number of threads for message
consuming. If there are more messages to be consumed while the running
threads are busy, then additional threads are started until the total
number of threads reaches the value of the maximum number of concurrent
consumers (ie., `         MaxConcurrentConsumers        ` ). The maximum
number of concurrent consumers (or the number of JMS proxies) that can
be deployed is limited by the base transport worker pool that is used by
the JMS transport. The size of this worker pool can be configured via
the system property 'lst\_t\_core' and 'lst\_t\_max' as described above.
The number of concurrent producers are generally limited by the Synapse
core worker pool.

> Concurrent consumers are only applicable to JMS queues not for JMS
topics.

**To increase the JMS listener performance**

Add the following parameters to the JMS listener configuration of the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file:

``` xml
    <parameter name="transport.jms.ConcurrentConsumers" locked="false">50</parameter>
    <parameter name="transport.jms.MaxConcurrentConsumers" locked="false">50</parameter>
```

**Parameters**

-   `          transport.jms.ConcurrentConsumers         ` : the
    concurrent threads that need to be started to consume messages when
    polling.
-   `          transport.jms.MaxConcurrentConsumers         ` : the
    maximum number of concurrent threads to use during polling.

> If you set t he `         locked        ` property to
`         true        ` , the JMS proxy creates only one listener thread
at a given time. If you set it to `         false        ` , then it
creates multiple listener threads from a single proxy to consume
messages concurrently.


Enable caching

Add the following parameter to the JMS listener configuration of the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file to enable
caching:

``` xml
    <parameter name="transport.jms.CacheLevel">consumer</parameter>
```

The possible values for the cache level are `         none        ` ,
`         auto        ` , `         connection        ` ,
`         session        ` and `         consumer        `. Out of the
possible values, `         consumer        ` is the highest level that
provides maximum performance.

After adding concurrency consumers and cache level, your complete
configuration would be as follows:

**Sample JMS listener configuration with concurrent consumers and
caching**

``` xml
    <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
    ....
    <parameter name="myQueueConnectionFactory" locked="false">
    <parameter name="java.naming.factory.initial" locked="false">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
    <parameter name="java.naming.provider.url" locked="false">tcp://localhost:61616</parameter>
    <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">QueueConnectionFactory</parameter>
    <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
    <parameter name="transport.jms.ConcurrentConsumers" locked="false">50</parameter>
    <parameter name="transport.jms.MaxConcurrentConsumers" locked="false">50</parameter>
    <parameter name="transport.jms.CacheLevel">consumer</parameter>
    </parameter>
    ….
    </transportReceiver>
```

### Configuring JMS Sender

#### Enabling caching

Add the following parameter to the JMS sender configuration of the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file:

```Java
<parameter name="transport.jms.CacheLevel">producer</parameter>
```

The possible values for the cache level are `         none        ` ,
`         auto        ` , `         connection        ` ,
`         session        ` and `         producer        ` . Out of the
possible values, `         producer        ` is the highest level that
provides maximum performance.

> When using `producer` as the cache level, ensure to add
the JMS destination parameter to avoid the following error:

```         
INFO - AxisEngine [MessageContext: logID=2eabe85aeeb3bb62c26bb46d21b11b087ebf1e5e0b350839] JMSCC0029: A destination must be specified when sending from        `
`         this        ` `         producer.
```

Remove `         ClientApiNonBlocking        ` when sending messages via
JMS

By default, Axis2 spawns a new thread to handle each outgoing message.
To change this behavior, you need to remove the
`         ClientApiNonBlocking        ` property from the message.

> Removal of this property can be vital when queuing transports like JMS
are involved.

**To remove the `          ClientApiNonBlocking         ` property**

Add the following parameter to the configuration:

``` xml
    <property name="ClientApiNonBlocking" action="remove" scope="axis2"/>
```

### Troubleshooting JMS scenarios

The following sections will help you to resolve common problems
encountered in JMS integration scenarios with WSO2 Enterprise Integrator
(WSO2 EI).

#### Handling ClassNotFoundExceptions and NoClassDefFoundExceptions

Check if you have deployed all the required client libraries. The
missing class should be available in one of the jar files deployed in
\<EI\_HOME\>/lib directory.

WSO2 EI comes with geronimo-jms library, which contains the javax.jms
packages. Therefore, you do not have to deploy them again.

#### HTTP header conversion

When forwarding HTTP traffic to a JMS queue using WSO2 EI, you might get
an error similar to the one given below.

``` java
    ERROR JMSSender Error creating a JMS message from the axis message context
    javax.jms.MessageFormatException: MQJMS1058: Invalid message property name: Content-Type

```

This exception is specific to the JMS broker used, and is  thrown by the
JMS client libraries used to connect with the JMS broker.  

The incoming HTTP message contains a bunch of HTTP headers that have the
‘-‘ character. Some noticeable examples are **Content-length** and
**Transfer-encoding** headers. When WSO2 EI forwards a message over JMS,
it sets the headers of the incoming message to the outgoing JMS message
as JMS properties. But, according to the JMS specification, the ‘-‘
character is prohibited in JMS property names. Some JMS brokers like
ActiveMQ do not check this specifically, in which case there will not be
any issues. But some brokers do and they throw exceptions.

The solution is to simply remove the problematic HTTP headers from the
message before delivering it over JMS. You can use the property mediator
as follows to achieve this:

```java
<property action="remove" name="Content-Length" scope="transport">
<property action="remove" name="Accept-Encoding" scope="transport">
<property action="remove" name="User-Agent" scope="transport">
<property action="remove" name="Content-Type" scope="transport">
```

Alternatively, you can use the
`         transport.jms.MessagePropertyHyphens        ` parameter to
handle hyphenated properties, instead of handling them as described
above. For more information on this parameter, see [the
`          transport.jms.MessagePropertyHyphens         ` parameter
description and possible
values.](https://docs.wso2.com/display/EI650/JMS+Transport#JMSTransport-hyphen)

#### JMS property data type mismatch

When the WSO2 EI attempts to forward a message over JMS, there are
instances that the client libraries throw an exception saying the data
type of a particular message property is invalid.  

This problem occurs when the developer uses the property mediator to
manipulate property values set on the message. Certain implementations
of JMS have data type restrictions on properties. But the property
mediator always sets property values as strings.

The solution is to revise the mediation sequences and avoid manipulating
property values containing non-string values. If you want to set a
non-string property value, write a simple custom mediator. Instructions
are given in section [Creating Custom
Mediators](https://docs.wso2.com/display/EI650/Creating+Custom+Mediators)
.   For an example, to set a property named foo with integer value
12345, use the property mediator as follows and set the type attribute
to INTEGER.  If the type attribute of the property is not specifically
set, it will be assigned to String by default.

```java
<property name="foo" value="12345" type="INTEGER" scope="transport/">
```
#### Too-many-threads and out-of-memory issues

With some JMS brokers, WSO2 EI tends to spawn new worker threads
indefinitely until it runs out of memory and crashes. This problem is
caused by a bug in the underlying Axis2 engine. A simple workaround to
this problem is to engage the property mediator of the  mediation
sequence  as follows:

```java
<property action="remove" name="transportNonBlocking" scope="axis2">
```

This prevents WSO2 EI from creating new worker threads indefinitely. You
can use a jconsole like  JMX client to  monitor the active threads and
memory consumption of WSO2 EI.


#### JMSUtils cannot locate destination

If your topic or queue name contains the termination characters ":" or
"=", JMSUtils will not be able to find the topic/queue and will give you
the warning "JMSUtils cannot locate destination". (For more information,
see <http://docs.oracle.com/javase/7/docs/api/java/util/Properties.html#load%28java.io.Reader%29>.) For example, if the topic name is `         my::topic        ` , the
following configuration will not work, because the topic name will be
parsed as `my` instead of `my::topic`:


```java         
<address uri="jms:/my::topic                  ?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=repository/conf/jndi.properties&amp;transport.jms.DestinationType=topic"/>
```

To avoid this issue, you can create a key-value pair in the
`         jndi.properties        ` file that maps the topic/queue name
to a key that either escapes these characters with a backslash (\\) or
does not contain ":" or "=". For example:

```java
topic.my\:\:topic = my::topic        
```

or

`         topic.myTopic = my::topic        `

You can then use this key in the proxy service as follows:

```         
<address uri="jms:/                   my\:\:topic                  ?transport.jms.ConnectionFactoryJNDIName=TopicConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;java.naming.provider.url=repository/conf/jndi.properties&amp;transport.jms.DestinationType=topic"/>        
```

If you do not want to use the JNDI properties file, you can define the
key-value pair right in the proxy configuration:

```java
<address uri="jms:/                   my\:\:topic                  ?transport.jms.ConnectionFactoryJNDIName=TopicConnectionFactory&amp;java.naming.factory.initial=org.wso2.andes.jndi.PropertiesFileInitialContextFactory&amp;                              topic.my                    \:\:topic                            =my                            ::topic                  &amp;java.naming.provider.url=repository/conf/jndi.properties&amp;transport.jms.DestinationType=topic"/>
```
