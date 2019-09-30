# Using a JDBC Message Store

In this sample, the client sends requests to a proxy service. The proxy service stores the messages in a JDBC message store. The back-end service is invoked by a message forwarding processor, which picks the messages stored in the JDBC message store.

### Synapse configuration

Sample configuration that uses a MySQL database named sampleDB and the database table **jdbc_message_store**

``` java tab="Proxy Service"
<proxy xmlns="http://ws.apache.org/ns/synapse"
              name="MessageStoreProxy"
              transports="https http"
              startOnLoad="true"
              trace="disable">
          <description/>
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

``` java tab="Message Store"
<messageStore xmlns="http://ws.apache.org/ns/synapse"
                     class="org.apache.synapse.message.store.impl.jdbc.JDBCMessageStore"
                     name="SampleStore">
  <parameter name="store.jdbc.password"/>
  <parameter name="store.jdbc.username">root</parameter>
  <parameter name="store.jdbc.driver">com.mysql.jdbc.Driver</parameter>
  <parameter name="store.jdbc.table">jdbc_message_store</parameter>
  <parameter name="store.jdbc.connection.url">jdbc:mysql://localhost:3306/sampleDB</parameter>
</messageStore>
```

``` java tab="Message Processor"
<messageProcessor xmlns="http://ws.apache.org/ns/synapse"
                         class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
                         name="ScheduledProcessor"
                         messageStore="SampleStore">
  <parameter name="max.delivery.attempts">5</parameter>
  <parameter name="interval">10</parameter>
  <parameter name="is.active">true</parameter>
</messageProcessor>
```

### Run the Example

1.  Setup the database. Use one of the following DB scripts depending on which database type you want to use. 

    ``` java tab="MySQL"
    CREATE TABLE jdbc_message_store(
                indexId BIGINT( 20 ) NOT NULL AUTO_INCREMENT ,
                msg_id VARCHAR( 200 ) NOT NULL ,
                message BLOB NOT NULL ,
                PRIMARY KEY ( indexId )
                )
    ```

    ``` java tab="H2"
    CREATE TABLE jdbc_message_store(
                    indexId BIGINT( 20 ) NOT NULL AUTO_INCREMENT ,
                    msg_id VARCHAR( 200 ) NOT NULL ,
                    message BLOB NOT NULL ,
                    PRIMARY KEY ( indexId )
                    )
    ```

2.  Add the relevant database driver into the `           MI_HOME/lib          ` folder.

3.  Create the synapse artifacts given above.

4.  Deploy the **SimpleStockQuoteService** client by navigating to
    MI_HOME/samples/axis2Server/src/SimpleStockQuoteService, and
    running the **ant** command in the command prompt or shell script.
    This will build the sample and deploy the service for you.

5.  The ESB profile of WSO2 EI comes with a default Axis2 server, which
    you can use as the back-end service for this sample. To start the
    Axis2 server, navigate to
    `           <EI_HOME>/samples/axis2server          ` , and run
    `           axis2Server.sh          ` on Linux or
    `           axis2Server.bat          ` on Windows.

6.  Go to <http://localhost:9000/services/SimpleStockQuoteService?wsdl>
    and verify that the service is running.
    
7. To invoke the proxy service, navigate to `MI_HOME/repository/samples/axis2client         ` , and execute the following command:

    ``` java
    ant stockquote -Daddurl=http://localhost:8280/services/MessageStoreProxy
    ```
