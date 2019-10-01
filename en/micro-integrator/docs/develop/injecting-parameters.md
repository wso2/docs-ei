# Injecting parameters through environment variables 

We should package artifacts that need to be deployed in the micro integrator (synapse configs, APIs, proxies, 
inbound-endpoints, etc) when we are creating a docker image.

In the current implementation of the synapse library, we need to specify parameters that can be varied in different environments in the synapse configuration itself.
So in order to achieve dynamic nature, we need to inject these values to the docker container in dynamic manner.
And the solution is to injecting these values through environment variables.
In following topics it is described what are the parameters that are supported to be inject through environment 
variables and sample from each section.

## End Points

1. Address Endpoint
    1. URI
2. HTTP Endpoint
    1. URI
3. Loadbalance Endpoint
    1. Hostname
    2. Port
4. RecipientList Endpoint
    1. Hostname
    2. Port
5. Template Endpoint
   1. URI
6. WSDL Endpoint
   1. wsdlURI

### Sample

If you want to define the URL with environment properties, you can define it as shown below.

```
<?xml version="1.0" encoding="UTF-8"?>
<endpoint xmlns="http://ws.apache.org/ns/synapse" name="JSON_EP">
  <address uri="$SYSTEM:VAR"/>
</endpoint>
```


Here VAR is the url you need to have set as environment property.

This is useful when you need to need to deploy the endpoint in a container.

In a VM var can be export as below : 

export VAR=http://localhost:61616/...

## DataService Deployment - InlineDataSource

1. Driver
2. URL
3. Username
4. Password

### Sample
```
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




## DataService Deployment - Separate DataSource

1. Driver
2. URL
3. Username
4. Password

### Sample
```
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

## Tasks 
1. Pinned Servers

### sample
```
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

## Proxy Services 
1. Pinned Servers

## Inbound endpoints 

1. HTTP/S 
    1. inbound.http.port
2. HL7 
    1. inbound.hl7.Port
3. CFX-WS-RM 
    1. Inbound.cxf.rm.port
    2. inbound.cxf.rm.host
4. Websocket/Secure 
    1.  inbound.ws.port
5. File 
    1. transport.vfs.FileURI
    2. transport.vfs.MoveAfterFailure
    3. transport.vfs.MoveAfterProcess
    4. transport.vfs. ReplyFileURI
6. JMS 
    1. pinnedServers
    2. java.naming.provider.url
    3. transport.jms.UserName
    4. transport.jms.Password
7. Kafka 
    1. zookeeper.connect
8. MQTT 
    1. mqtt.server.host.name
    2. mqtt.server.port
    3. mqtt.subscription.username
    4. mqtt.subscription.password
9. RabbitMQ 
    1. rabbitmq.server.host.name
    2. rabbitmq.server.port
    3. rabbitmq.server.user.name
    4. rabbitmq.server.password
    
### sample
```
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

## Listener Proxy - All the Above Properties

### sample
```
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

## Message Store
1. JMS Message Store 
    1. store.jms.username
    2. store.jms.password
    3. store.jms.connection.factory

2. RabbitMQ Message Store
    1. store.rabbitmq.host.name
    2. store.rabbitmq.host.port
    3. store.rabbitmq.username
    4. store.rabbitmq.password

3. JDBC Message Store
    1. store.jdbc.drive
    2. store.jdbc.connection.url
    3. store.jdbc.username
    4. store.jdbc.password

4. Resequence Message Store
    1. store.jdbc.drive
    2. store.jdbc.connection.url
    3. store.jdbc.username
    4. store.jdbc.password

5. WSO2 MB Message Store - Jms Message Store Properties are supported

### sample
```
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
