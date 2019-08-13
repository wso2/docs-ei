# JDBC Message Store

The JDBC message store can be used to store and retrieve messages more efficiently in comparison with other message stores.

The JDBC message store implementation is a variation of the already existing synapse message store implementation and is designed in a manner similar to the message store of the ESB profile. The JDBC message store uses a JDBC connector to connect to external relational databases.

![](attachments/119131493/119131494.png){.image-center}

The advantages of using a JDBC message store instead of any other
message store are as follows:

-   Easy to connect – You only need to have a JDBC connector to connect
    to an external relational database.
-   Quick transactions – JDBC message stores are capable of handling a
    large number of transactions per second.
-   Ability to work with a high capacity for a long period of time –
    Since JDBC stores use databases as the medium to store data, it can
    store a large volume of data and is capable of handling data for a
    longer period of time.

## Sample configurations

The syntax of the JDBC message store can be different depending on
whether you connect to the database using a connection pool, or using a
datasource. Click on the relevant tab to view the syntax based on how
you want to connect to the database.

``` java tab="Connection Pool"
<messageStore class="org.apache.synapse.message.store.jdbc.JDBCMessageStore" name="MyStore" xmlns="http://ws.apache.org/ns/synapse">  
    <parameter name="store.jdbc.driver">Driver</parameter>
    <parameter name="store.jdbc.connection.url">ConnectionURL</parameter>
    <parameter name="store.jdbc.username">Username</parameter>
    <parameter name="store.jdbc.password">Password</parameter>
    <parameter name="store.jdbc.table">jdbcTable</parameter>  
</messageStore>
```

``` java tab="External Datasource"
<messageStore class="org.apache.synapse.message.store.jdbc.JDBCMessageStore" name="MyStore" xmlns="http://ws.apache.org/ns/synapse">
    <parameter name="store.jdbc.dsName">dsName</parameter>
    <parameter name="store.jdbc.table">jdbcTable</parameter>
</messageStore>
```

## Parameters

### Parameters (Required)

Following are descriptions of the fields that are displayed:

| Field          | Description                                                    |
|----------------|----------------------------------------------------------------|
| Name           | The name of the message store                                  |
| Database Table | The name of the database table.                                |
| Driver         | The class name of the database driver.                         |
| Url            | The JDBC URL of the database that the data will be written to. |
| User           | The user name used to connect to the database.                 |
| Password       | The password used to connect to the database.                  |

### Parameters (Connection pool)

Following are the parameters that should be specified and the
description for each:

| Parameter                                                    | Description                            | Required |
|--------------------------------------------------------------|----------------------------------------|----------|
| `                 store.jdbc.driver                `         | The class name of the database driver. | YES      |
| `                 store.jdbc.connection.url                ` | The database URL.                      | YES      |
| `                 store.jdbc.username                `       | The user name to access the database.  | YES      |
| `                 store.jdbc.password                `       | The password to access the database.   | NO       |
| `                 store.jdbc.table                `          | Table name of the database.            | YES      |

### Parameters (External Datasource)

Following are the parameters that should be specified and the description for each:

| Parameter                                            | Description                                | Required |
|:-----------------------------------------------------|--------------------------------------------|----------|
| `                 store.jdbc.dsName                ` | The name of the datasource to be looked up | YES      |
| `                 store.jdbc.table                `  | The table name of the database.            | YES      | 

### Paramaters (Carbon Datasource)

To make sure that the datasource appears in the **Datasource Name** list, you need to expose it as a JNDI datasource.

Following are descriptions of the fields that are displayed:

| Field           | Description                       |
|-----------------|-----------------------------------|
| Name            | The name for the message store    |
| Database Table  | The name of the database table.   |
| Datasource Name | The class name of the datasource. |

## Example

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
