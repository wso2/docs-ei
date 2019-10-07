# Injecting Parameters as Environment Variables 

When deploying integration artifacts in different environments, it is necessary to change the synapse parameters used in the artifacts according to the environment. For example, the 'endpoint URL' will be different in each environment. If you define the synapse parameters in your artifacts as explained below, you can inject the required parameter values for each environment using system variables. Without this feature, you need to create and maintain separate artifacts for each environment.

This feature is useful for container deployments. We need to dynamically inject the parameter values to the docker container.

## Injecting Endpoint parameters

Configure the Endpoint parameters in your synapse configuration as shown below.

<table>
    <tr>
        <th>Endpoint Type</th>
        <th>Parameters</th>
    </tr>
    <tr>
        <td>Address Endpoint</td>
        <td><code>uri</code></td>
    </tr>
    <tr>
        <td>HTTP Endpoint</td>
        <td><code>uri</code></td>
    </tr>
    <tr>
        <td>Loadbalance Endpoint</td>
        <td>
            <code>hostname</code> and <code>port</code>
        </td>
    </tr>
    <tr>
        <td>RecipientList Endpoint</td>
        <td>
            <code>hostname</code> and <code>port</code>
        </td>
    </tr>
    <tr>
        <td>Template Endpoint</td>
        <td>
            <code>uri</code>
        </td>
    </tr>
    <tr>
        <td>WSDL Endpoint</td>
        <td>
            <code>wsdlURI</code>
        </td>
    </tr>
</table>

### Example

In the following example, the endpoint URL is configured for an environment variable.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<endpoint xmlns="http://ws.apache.org/ns/synapse" name="JSON_EP">
  <address uri="$SYSTEM:VAR"/>
</endpoint>
```

-   In a **VM** deployment, you can export the variables as shown below. Here VAR is the url you need to have set as environment property.

    ```bash
    export VAR=http://localhost:61616/...
    ```

## Injecting Data service parameters

-   `Driver`
-   `URL`
-   `Username`
-   `Password`

### Example

In the following example, the data service parameters are configured for an environment variable.

```xml tab='Inline Datasource'
<data name="DataServiceSample" serviceGroup="" serviceNamespace="">
    <description/>
    <config id="SourceSample">
        <property name="org.wso2.ws.dataservice.user">$SYSTEM:uname</property>
        <property name="org.wso2.ws.dataservice.password">$SYSTEM:pass</property>
        <property name="org.wso2.ws.dataservice.protocol">$SYSTEM:url1</property>
        <property name="org.wso2.ws.dataservice.driver">$SYSTEM:driver1</property>
    </config>
    <query>
    --------------------
    </query>
    <operation>
    --------------------
    </operation>
</data>
```

```xml tab='External Datasource'
<datasource>
    <name>MySQLConnection</name>
    <description>MySQL Connection</description>
    <definition type="RDBMS">
        <configuration>
            <driverClassName>$SYSTEM:driver1</driverClassName>
            <url>$SYSTEM:url1</url>
            <username>$SYSTEM:uname</username>
            <password>$SYSTEM:pass</password>
        </configuration>
    </definition>
</datasource>
```

-  In a **VM** deployment, you can export the variables as shown below. Here VAR is the url you need to have set as environment property.

    ```bash
    export uname=
    export pass=
    export url1=
    export driver1=
    ```

## Injecting Scheduled Task parameters

The <b>pinned servers</b> parameter can be set as an environment variable for a scheduled task or proxy service. See the examples given below.

### Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<task class="org.apache.synapse.startup.tasks.MessageInjector" group="synapse.simple.quartz" name="ProxytestInject" pinnedServers="$SYSTEM:pinned" xmlns="http://ws.apache.org/ns/synapse">
    <trigger count="5" interval="10"/>
    <property name="injectTo" value="proxy" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
    <property name="proxyName" value="testProxy" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
    <property name="soapAction" value="mediate" xmlns:task="http://www.wso2.org/products/wso2commons/tasks"/>
    <property name="message" xmlns:task="http://www.wso2.org/products/wso2commons/tasks">
        ----------
    </property>
</task>
```
-   In a **VM** deployment, you can export the variables as shown below. Here VAR is the url you need to have set as environment property.

    ```bash
    export pinned=
    ```

## Injecting Inbound Endpoint parameters

See the list of properties that can be defined as environment variables:

-   <a href="./../../../../references/synapse-properties/inbound-endpoints/listening-inbound-endpoints/http-inbound-endpoint-properties">HTTP/HTTPS Inbound Protocol</a>
-   <a href="./../../../../references/synapse-properties/inbound-endpoints/listening-inbound-endpoints/hl7-inbound-endpoint-properties">HL7 Inbound Protocol</a>
-   <a href="./../../../../references/synapse-properties/inbound-endpoints/listening-inbound-endpoints/cxf-ws-rm-inbound-endpoint-properties">CXF WS-RM Inbound Protocol</a>
-   <a href="./../../../../references/synapse-properties/inbound-endpoints/listening-inbound-endpoints/websocket-inbound-endpoint-properties">Websocket Inbound Protocol</a>

-   <a href="./../../../../references/synapse-properties/inbound-endpoints/polling-inbound-endpoints/file-inbound-endpoint-properties">File Inbound Protocol</a>
-   <a href="./../../../../references/synapse-properties/inbound-endpoints/polling-inbound-endpoints/jms-inbound-endpoint-properties">JMS Inbound Protocol</a>
-   <a href="./../../../../references/synapse-properties/inbound-endpoints/polling-inbound-endpoints/kafka-inbound-endpoint-properties">Kafka Inbound Protocol</a>

-   <a href="./../../../../references/synapse-properties/inbound-endpoints/event-based-inbound-endpoints/mqtt-inbound-endpoint-properties">MQTT Inbound Protocol</a>
-   <a href="./../../../../references/synapse-properties/inbound-endpoints/event-based-inbound-endpoints/rabbitmq-inbound-endpoint-properties">RabbitMQ Inbound Protocol</a>

<!--
<table>
    <tr>
        <th>Inbound Endpoint Type</th>
        <th>Parameters</th>
    </tr>
    <tr>
        <td>HTTP/S</td>
        <td>
            <code>inbound.http.port</code>
        </td>
    </tr>
    <tr>
        <td>HL7</td>
        <td>
            <code>inbound.hl7.Port</code>
        </td>
    </tr>
    <tr>
        <td>CFX-WS-RM</td>
        <td>
            <ul>
                <li>
                    <code>Inbound.cxf.rm.port</code>
                </li>
                <li>
                    <code>inbound.cxf.rm.host</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Websocket/Secure</td>
        <td>
            <code>inbound.ws.port</code>
        </td>
    </tr>
    <tr>
        <td>File</td>
        <td>
            <ul>
                <li>
                    <code>transport.vfs.FileURI</code>
                </li>
                <li>
                    <code>transport.vfs.MoveAfterFailure</code>
                </li>
                <li>
                    <code>transport.vfs.MoveAfterProcess</code>
                </li>
                <li>
                    <code>transport.vfs. ReplyFileURI</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>JMS</td>
        <td>
            <ul>
                <li>
                    <code>pinnedServers</code>
                </li>
                <li>
                    <code>java.naming.provider.url</code>
                </li>
                <li>
                    <code>transport.jms.UserName</code>
                </li>
                <li>
                    <code>transport.jms.Password</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Kafka</td>
        <td>
            <code>zookeeper.connect</code>
        </td>
    </tr>
    <tr>
        <td>MQTT</td>
        <td>
            <ul>
                <li>
                    <code>mqtt.server.host.name</code>
                </li>
                <li>
                    <code>mqtt.server.port</code>
                </li>
                <li>
                    <code>mqtt.subscription.username</code>
                </li>
                <li>
                    <code>mqtt.subscription.password</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>RabbitMQ</td>
        <td>
            <ul>
                <li>
                    <code>rabbitmq.server.host.name</code>
                </li>
                <li>
                    <code>rabbitmq.server.port</code>
                </li>
                <li>
                    <code>rabbitmq.server.user.name</code>
                </li>
                <li>
                    <code>rabbitmq.server.password</code>
                </li>
            </ul>
        </td>
    </tr>
</table>
-->

### Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<inboundEndpoint name="jms" onError="fault" protocol="jms" sequence="LogMsgSeq" suspend="false" xmlns="http://ws.apache.org/ns/synapse">
    <parameters>
        <parameter name="interval">15000</parameter>
        <parameter name="sequential">true</parameter>
        <parameter name="coordination">true</parameter>
        <parameter name="transport.jms.Destination">myq</parameter>
        <parameter name="transport.jms.CacheLevel">3</parameter>
        <parameter name="transport.jms.ConnectionFactoryJNDIName">$SYSTEM:jmsconfac</parameter>
        <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
        <parameter name="java.naming.provider.url">$SYSTEM:jmsurl</parameter>
        <parameter name="transport.jms.UserName">$SYSTEM:jmsuname</parameter>
        <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
        <parameter name="transport.jms.Password">$SYSTEM:jmspass</parameter>
        <parameter name="transport.jms.SessionTransacted">false</parameter>
        <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
        <parameter name="transport.jms.ContentType">application/xml</parameter>
        <parameter name="transport.jms.SharedSubscription">false</parameter>
        <parameter name="pinnedServers">$SYSTEM:pinned</parameter>
        <parameter name="transport.jms.ResetConnectionOnPollingSuspension">false</parameter>
    </parameters>
</inboundEndpoint>
```

-   In a **VM** deployment, you can export the variables as shown below. Here VAR is the url you need to have set as environment property.

    ```bash
    export jmsconfac=
    export jmsurl=
    export jmsuname=
    export jmspass=
    export pinned=
    ```

## Injecting proxy service parameters
    
The <b>pinned servers</b> parameter as well as all the service-level <b>transport parameters</b>:

-   [JMS parameters](../../references/synapse-properties/transport-parameters/jms-transport-parameters)
-   [FIX parameters](../../references/synapse-properties/transport-parameters/fix-transport-parameters)
-   [MailTo parameters](../../references/synapse-properties/transport-parameters/mailto-transport-parameters)
-   [MQTT parameters](../../references/synapse-properties/transport-parameters/mqtt-transport-parameters)
-   [RabbitMQ parameters](../../references/synapse-properties/transport-parameters/rabbitmq-transport-parameters)
-   [VFS parameters](../../references/synapse-properties/transport-parameters/vfs-transport-parameters)

### Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="JmsListner" pinnedServers="localhost" startOnLoad="true" transports="http https jms" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
            -------------
            <drop/>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </target>
    <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
    <parameter name="transport.jms.Destination">myq</parameter>
    <parameter name="transport.jms.ConnectionFactoryType">queue</parameter>
    <parameter name="transport.jms.ContentType">application/xml</parameter>
    <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>
    <parameter name="java.naming.provider.url">$SYSTEM:jmsurl</parameter>
    <parameter name="transport.jms.SessionTransacted">false</parameter>
    <parameter name="transport.jms.ConnectionFactoryJNDIName">$SYSTEM:jmsconfac</parameter>
    <parameter name="transport.jms.UserName">$SYSTEM:jmsuname</parameter>
    <parameter name="transport.jms.Password">$SYSTEM:jmspass</parameter>
</proxy>
```

-  In a **VM** deployment, you can export the variables as shown below. Here VAR is the url you need to have set as environment property.

    ```bash
    export jmsurl=
    export jmsconfac=
    export jmsuname=
    export jmspass=
    ```

## Injecting Message Store parameters

<table>
    <tr>
        <th>Message Store Type</th>
        <th>Parameters</th>
    </tr>
    <tr>
        <td>JMS Message Store</td>
        <td rowspan="2">
            <ul>
                <li>
                    <code>store.jms.username</code>
                </li>
                <li>
                    <code>store.jms.password</code>
                </li>
                <li>
                    <code>store.jms.connection.factory</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>WSO2 MB Message Store</td>
    </tr>
    <tr>
        <td>RabbitMQ Message Store</td>
        <td>
            <ul>
                <li>
                    <code>store.rabbitmq.host.name</code>
                </li>
                <li>
                    <code>store.rabbitmq.host.port</code>
                </li>
                <li>
                    <code>store.rabbitmq.username</code>
                </li>
                <li>
                    <code>store.rabbitmq.password</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>JDBC Message Store</td>
        <td rowspan="2">
            <ul>
                <li>
                    <code>store.jdbc.drive</code>
                </li>
                <li>
                    <code>store.jdbc.connection.url</code>
                </li>
                <li>
                    <code>store.jdbc.username</code>
                </li>
                <li>
                    <code>store.jdbc.password</code>
                </li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Resequence Message Store</td>
    </tr>
</table>

### Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<messageStore class="org.apache.synapse.message.store.impl.rabbitmq.RabbitMQStore" name="InboundStore" xmlns="http://ws.apache.org/ns/synapse">
    <parameter name="store.rabbitmq.host.name">$SYSTEM:rabbithost</parameter>
    <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
    <parameter name="store.rabbitmq.host.port">$SYSTEM:rabbitport</parameter>
    <parameter name="store.rabbitmq.route.key"/>
    <parameter name="store.rabbitmq.username">$SYSTEM:rabbitname</parameter>
    <parameter name="store.rabbitmq.virtual.host"/>
    <parameter name="rabbitmq.connection.ssl.enabled">false</parameter>
    <parameter name="store.rabbitmq.exchange.name">exchange3</parameter>
    <parameter name="store.rabbitmq.queue.name">queue3</parameter>
    <parameter name="store.rabbitmq.password">$SYSTEM:rabbitpass</parameter>
</messageStore>
```

-  In a **VM** deployment, you can export the variables as shown below.

    ```bash
    export rabbithost=
    export rabbitport=
    export rabbitname=
    export rabbitpass=
    ```
