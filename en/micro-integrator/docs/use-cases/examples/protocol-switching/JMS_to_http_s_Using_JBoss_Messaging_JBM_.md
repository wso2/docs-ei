# Transport Switching - JMS to http/s Using JBoss Messaging(JBM)

Introduction to switching transports with proxy services.
The JMS provider will be JBoss Messaging(JBM) (
http://www.jboss.org/jbossmessaging/ ).

```
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy name="StockQuoteProxy" transports="jms">
        <target>
            <inSequence>
                <property action="set" name="OUT_ONLY" value="true"/>
            </inSequence>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
            </endpoint>
            <outSequence>
                <send/>
            </outSequence>
        </target>
        <publishWSDL uri="file:repository/conf/sample/resources/proxy/sample_proxy_1.wsdl"/>
        <parameter name="transport.jms.ContentType">
            <rules>
                <jmsProperty>contentType</jmsProperty>
                <default>application/xml</default>
            </rules>
        </parameter>
    </proxy>
</definitions>
```

**Prerequisites:**

-   Start the Axis2 server and deploy the SimpleStockQuoteService (Refer
    steps above)
-   Download , install and start JBM server (
    [http://www.jboss.org/jbossmessaging](http://www.jboss.org/jbossmessaging/)
    ), and configure Synapse to listen on JBM (refer notes below)

Configure the required queues in JBM. Add the following entry to JBM jms
configuration inside file-config/stand-alone/non-clustered/jbm-jms.xml.

```
<queue name="StockQuoteProxy">
    <entry name="StockQuoteProxy"/>
</queue>
```

Once you started the JBM server with the above changes you'll be able to
see the following on STDOUT.

``` java
10:18:02,673 INFO [org.jboss.messaging.core.server.impl.MessagingServerImpl] JBoss Messaging Server version 2.0.0.BETA3 (maggot, 104) started
```

You also need to copy the jbm-core-client.jar, jbm-jms-client.jar,
jnp-client.jar(these jars are inside $JBOSS\_MESSAGING/client folder)
and jbm-transports.jar, netty.jar(these jars are from
$JBOSS\_MESSAGING/lib folder) jars from JBM into the
$ESB\_HOME/repository/components/lib directory. This was tested with JBM
2.0.0.BETA3

You need to add the following configuration for Axis2 JMS transport
listener in axis2.xml found at repository/conf/axis2.xml.

```
<transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
    <parameter name="default" locked="false">
        <parameter name="java.naming.factory.initial">org.jnp.interfaces.NamingContextFactory</parameter>
        <parameter name="java.naming.provider.url">jnp://localhost:1099</parameter>
        <parameter name="java.naming.factory.url.pkgs">org.jboss.naming:org.jnp.interfaces</parameter>
        <parameter name="transport.jms.ConnectionFactoryJNDIName">ConnectionFactory</parameter>
    </parameter>
</transportReceiver>
```
Once you start ESB configuration 263 and request for the WSDL of the
proxy service ( <http://localhost:8280/services/StockQuoteProxy?wsdl> )
you will notice that its exposed only on the JMS transport. This is
because the configuration specified this requirement in the proxy
service definition.

Before running the JMS client you need to open the build.xml ant script
and uncomment the following block under the jmsclient target.

```
<!--<sysproperty key="java.naming.provider.url" value="${java.naming.provider.url}"/> <sysproperty key="java.naming.factory.initial" value="${java.naming.factory.initial}"/> <sysproperty key="java.naming.factory.url.pkg" value="${java.naming.factory.url.pkg}"/>-->
```

Now lets send a stock quote request on JMS, using the dumb stock quote
client as follows:

``` java
ant jmsclient -Djms_type=pox -Djms_dest=StockQuoteProxy -Djms_payload=MSFT -Djava.naming.provider.url=jnp://localhost:1099 -Djava.naming.factory.initial=org.jnp.interfaces.NamingContextFactory 
```

-Djava.naming.factory.url.pkgs=org.jboss.naming:org.jnp.interfaces

On the ESB debug(you'll need to enable debug log in ESB) log you will
notice that the JMS listener received the request message as:

``` java
[JMSWorker-1] DEBUG ProxyServiceMessageReceiver -Proxy Service StockQuoteProxy received a new message...
```

Now if you examine the console running the sample Axis2 server, you will
see a message indicating that the server has accepted an order as
follows:

``` java
Accepted order for : 16517 stocks of MSFT at $ 169.14622538721846
```

In this sample, the client sends the request message to the proxy
service exposed over JMS in Synsape. Synapse forwards this message to
the HTTP EPR of the simple stock quote service hosted on the sample
Axis2 server. Note that the operation is out-only and no response is
sent back to the client. The transport.jms.ContentType property is
necessary to allow the JMS transport to determine the content type of
incoming messages. With the given configuration it will first try to
read the content type from the 'contentType' message property and fall
back to 'application/xml' (i.e. POX) if this property is not set. Note
that the JMS client used in this example doesn't send any content type
information.

!!! Note
    It is possible to instruct a JMS proxy service to listen to an already
existing destination without creating a new one. To do this, use the
parameter elements on the proxy service definition to specify the
destination and connection factory etc. For example:
    <parameter name="transport.jms.Destination">dynamicTopics/something.TestTopic</parameter> 
