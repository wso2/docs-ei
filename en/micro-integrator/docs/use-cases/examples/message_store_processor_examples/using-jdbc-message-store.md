# Using a JDBC Message Store
## Example use case
In this sample, the client sends requests to a proxy service. The proxy service stores the messages in a JDBC message store. The back-end service is invoked by a message forwarding processor, which picks the messages stored in the JDBC message store.

## Synapse configuration

Sample configuration that uses a MySQL database named sampleDB and the database table **jdbc_message_store**

```xml tab="Proxy Service"
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="MessageStoreProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
          <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
          <property name="OUT_ONLY" value="true"/>
          <property name="target.endpoint" value="StockQuoteServiceEp"/>
          <store messageStore="SampleStore"/>
      </inSequence>
    </target>
    <publishWSDL uri="http://localhost:9000/services/SimpleStockQuoteService?wsdl"/>
</proxy>
```

```xml tab="Message Store"
<?xml version="1.0" encoding="UTF-8"?>
<messageStore class="org.apache.synapse.message.store.impl.jdbc.JDBCMessageStore" name="SampleStore" xmlns="http://ws.apache.org/ns/synapse">
    <parameter name="store.jdbc.driver">com.mysql.jdbc.Driver</parameter>
    <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
    <parameter name="store.jdbc.username">root</parameter>
    <parameter name="store.jdbc.connection.url">jdbc:mysql://localhost:3306/sampleDB</parameter>
    <parameter name="store.jdbc.password">********</parameter>
    <parameter name="store.jdbc.table">jdbc_message_store</parameter>
</messageStore>
```

```xml tab="Message Processor"
<?xml version="1.0" encoding="UTF-8"?>
<messageProcessor class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" messageStore="SampleStore" name="ScheduledProcessor" targetEndpoint="StockQuoteServiceEp" xmlns="http://ws.apache.org/ns/synapse">
    <parameter name="client.retry.interval">1000</parameter>
    <parameter name="max.delivery.attempts">5</parameter>
    <parameter name="member.count">1</parameter>
    <parameter name="store.connection.retry.interval">1000</parameter>
    <parameter name="max.store.connection.attempts">-1</parameter>
    <parameter name="max.delivery.drop">Disabled</parameter>
    <parameter name="interval">10000</parameter>
    <parameter name="is.active">true</parameter>
</messageProcessor>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create a REST Api artifact with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Setup the database. Use one of the following DB scripts depending on which database type you want to use. 

```java tab="MySQL"
CREATE TABLE jdbc_message_store(
            indexId BIGINT( 20 ) NOT NULL AUTO_INCREMENT ,
            msg_id VARCHAR( 200 ) NOT NULL ,
            message BLOB NOT NULL ,
            PRIMARY KEY ( indexId )
            )
```

```java tab="H2"
CREATE TABLE jdbc_message_store(
                indexId BIGINT( 20 ) NOT NULL AUTO_INCREMENT ,
                msg_id VARCHAR( 200 ) NOT NULL ,
                message BLOB NOT NULL ,
                PRIMARY KEY ( indexId )
                )
```

Invoke the sample Api:

```java
ant stockquote -Daddurl=http://localhost:8290/services/MessageStoreProxy
```
