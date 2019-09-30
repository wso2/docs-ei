# Storage Integration

##Introduction

This section explains how to integrate data stores into Streaming Integration flows. The Streaming integrator can consume
 data from stores in a streaming manner as well as publish data into them. You can perform CRUD operations and Change Data Capture (CDC).
 
## Configuring stores

You can store data in-memory or in a physical data store. In a production environment, it is recommended to store data in a physical store because data stored in-memory can be lost if a system failure occurs.
 
![Defining a Data Store](../../images/storage-integration/defining-stores.png)

If you want to share a database across multiple Siddhi applications, you must define a data store in the `<SI_HOME>/conf/server/deployment.yaml` 
file. If you want a single connection pool, you can define a store as a ref in the `ref` section of the `<SI_HOME>/conf/server/deployment.yaml` file. If you want to define a unique data store for your Siddhi application, you can define it inline.

To understand all three methods consider an example where a factory manager wants to store the purchase records of raw 
material supplies to be stored in order to able to check the availability of materials at any given time.

### Storing data in existing data sources

To understand how to store data in existing data sources, follow the procedure below:

!!!info
    You need to create a database and a table, and then connect it to the Streaming Integrator via a data source. For this example, you can create a database named `FactoryMaterialDB` as follows:

    1. Download and install [MySQL Server](https://dev.mysql.com/downloads/).

    2. Download the [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).

    3. Unzip the downloaded MySQL driver zipped archive, and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) into the <SI_HOME>/lib directory.

    4. Enter the following command in a terminal/command window, where username is the username you want to use to access the databases.

        `mysql -u username -p`

    5. When prompted, specify the password you are using to access the databases with the username you specified.

    6. Add the following configuration under the `Data Sources Configuration` section of the `<SI_HOME>/conf/server/deployment.yaml` file.

        !!!info
            You need to change the values for the username and password parameters to the username and password that you are using to access the MySQL database.
            ```
            - name: FactoryMaterialDB
              description: Datasource used for Factory Supply Records
              jndiConfig:
                name: jdbc/FactoryMaterialDB
                useJndiReference: true
              definition:
                type: RDBMS
                configuration:
                  jdbcUrl: 'jdbc:mysql://localhost:3306/FactoryMaterialDB'
                  username: root
                  password: root
                  driverClassName: com.mysql.jdbc.Driver
                  maxPoolSize: 50
                  idleTimeout: 60000
                  connectionTestQuery: SELECT 1
                  validationTimeout: 30000
                  isAutoCommit: false
            ```

    7. To create a database table named `FactoryMaterialDB`, issue the following commands from the terminal.
        `mysql> create database FactoryMaterialDB;`<br/>
        `mysql> use FactoryMaterialDB;`<br/>
        `mysql> source <SI_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;`<br/>
        `mysql> grant all on FactoryMaterialDB.* TO username@localhost identified by "password";`

1. Start creating a new Siddhi application. You can name it `ShipmentHistoryApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
   `@App:name('ShipmentHistory');`
   
2. First, define an input stream to capture the data that you need to store as follows.

    ```
    @source(type = 'http', @map(type = 'json'))
    define stream RawMaterialStream(transRef long, material string, supplier string, amount double);

    ```
    
    Here, each event representing a material shipment transaction is received via the `http` transport and in JSON format. This is specified via the source annotation. For more information about consuming data via sources, see the [Consuming Data guide](consuming-messages.md).
    
3. Now let's define the table in which you are storing the data as follows.
    `define table ShipmentDetails(transRef long, material string, supplier string, amount double);`
    
4. To save the data in the required database, connect the table you defined to the datasource that you previously created.
    ```
    @Store(type='rdbms', datasource='FactoryMaterialDB')@PrimaryKey("transRef")
    define table ShipmentDetails(transRef long, material string, supplier string, amount double);
    ```
    
5. To store the information arriving at the `RawMaterialStream` stream to the `ShipmentDetails` table, write a Siddhi query as follows:
    1. To specify that events to be stored are taken from the `RawMaterialStream` input stream, add the `from` clause as follows.
        `from RawMaterialStream`
        
    2. To derive the exact attribute names with which the records are stored in the table, add the `select` statement as follows.
        `select transRef, material, supplier`
    
    3. If a record from the stream already exists in a the `ShipmentDetails` table, it should be updated with the latest 
    information from the stream. If it does not already exist, it must be inserted as a new record. To specify this, add an `update and insert` clause as follows.
        `update or insert into ShipmentDetails on ShipmentDetails.transRef == transRef;`
        
       Each record is identified by the transaction reference which is the primary key. Therefore, the condition `on ShipmentDetails.transRef == transRef` means that if a record with the same value for the `transRef` attribute exists in the table, the record from the stream updates it. If not, the record from the stream is inserted as a new record.

### Referring to externally defined stores

To understand how to refer to am externally defined store, follow the procedure below:

!!!info "Before you begin:"
    You need to create a database and a table, and then define an external store referring to it as follows.

    1. Download and install [MySQL Server](https://dev.mysql.com/downloads/).

    2. Download the [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).

    3. Unzip the downloaded MySQL driver zipped archive, and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) into the `<SI_HOME>/lib` directory.

    4. Enter the following command in a terminal/command window, where username is the username you want to use to access the databases.

        `mysql -u username -p`

    5. When prompted, specify the password you are using to access the databases with the username you specified.

    6. In the `<SI_HOME>/conf/server/deployment.yaml` file, add a subsection for refs and connect to the database you created as shown below.

        ```
        siddhi:
          refs:
            -
               ref:
                 name: 'FactoryMaterial'
                 type: 'rdbms'
                 properties:
                   jdbc.url: ''jdbc:mysql://localhost:3306/FactoryMaterialDB''
                   username: '<YOUR_USERNAME>'
                   password: '<YOUR_PASSWORD>'
                   field.length='currentTime:100'
                   jdbc.driver.name: 'com.mysql.jdbc.Driver'
        ```

    7. To create a database table named `FactoryMaterialDB`, issue the following commands from the terminal.
        `mysql> create database FactoryMaterialDB;`<br/>
        `mysql> use FactoryMaterialDB;`<br/>
        `mysql> source <SI_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;`<br/>
        `mysql> grant all on FactoryMaterialDB.* TO username@localhost identified by "password";`
            
            
1. Start creating a new Siddhi application. You can name it `ShipmentHistoryApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
   `@App:name('ShipmentHistory');`
   
2. First, define an input stream to capture the data that you need to store as follows.

    ```
    @source(type = 'http', @map(type = 'json'))
    define stream RawMaterialStream(transRef long, material string, supplier string, amount double);

    ```
    
    Here, each event representing a material shipment transaction is received via the `http` transport and in JSON format. This is specified via the source annotation. For more information about consuming data via sources, see the [Consuming Data guide](consuming-messages.md).
    
3. Now let's define the table in which you are storing the data as follows.
    `define table ShipmentDetails(transRef long, material string, supplier string, amount double);`
    
4. To save the data in the required database, connect the table you defined to the ref that you previously created.
   `
   @Store(ref='FactoryMaterial')
   @PrimaryKey('transRef')
   define table ShipmentDetails(transRef long, material string, supplier string, amount double);
   `
   Here, you are referring to the `FactoryMaterial` external store you configured and specifying the `transRef` attribute as the primary key.
    
5. To store the information arriving at the `RawMaterialStream` stream to the `ShipmentDetails` table, write a Siddhi query as follows:
    1. To specify that events to be stored are taken from the `RawMaterialStream` input stream, add the `from` clause as follows.
        `from RawMaterialStream`
        
    2. To derive the exact attribute names with which the records are stored in the table, add the `select` statement as follows.
        `select transRef, material, supplier`
    
    3. If a record from the stream already exists in a the `ShipmentDetails` table, it should be updated with the latest 
    information from the stream. If it does not already exist, it must be inserted as a new record. To specify this, add an `update and insert` clause as follows.
        `update or insert into ShipmentDetails on ShipmentDetails.transRef == transRef;`
        
       Each record is identified by the transaction reference which is the primary key. Therefore, the condition `on ShipmentDetails.transRef == transRef` means that if a record with the same value for the `transRef` attribute exists in the table, the record from the stream updates it. If not, the record from the stream is inserted as a new record.



###Configuring data stores inline
To understand how to define a store inline, follow the procedure below:

!!!info "Before you begin"
    You need to create a database and a table, and then define an external store referring to it as follows.

     1. Download and install [MySQL Server](https://dev.mysql.com/downloads/).

     2. Download the [MySQL JDBC driver](https://dev.mysql.com/downloads/connector/j/).

     3. Unzip the downloaded MySQL driver zipped archive, and copy the MySQL JDBC driver JAR (mysql-connector-java-x.x.xx-bin.jar) into the <SI_HOME>/lib directory.    

     4. Enter the following command in a terminal/command window, where username is the username you want to use to access the databases.

        `mysql -u username -p`

     5. When prompted, specify the password you are using to access the databases with the username you specified.

     6. To create a database table named `FactoryMaterialDB`, issue the following commands from the terminal.
        `mysql> create database FactoryMaterialDB;`<br/>
        `mysql> use FactoryMaterialDB;`<br/>
        `mysql> source <SI_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;`<br/>
        `mysql> grant all on FactoryMaterialDB.* TO username@localhost identified by "password";`
        
1. Start creating a new Siddhi application. You can name it `ShipmentHistoryApp` For instructions, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
   `@App:name('ShipmentHistory');`
   
2. First, define an input stream to capture the data that you need to store as follows.

    ```
    @source(type = 'http', @map(type = 'json'))
    define stream RawMaterialStream(transRef long, material string, supplier string, amount double);

    ```
    
    Here, each event representing a material shipment transaction is received via the `http` transport and in JSON format. This is specified via the source annotation. For more information about consuming data via sources, see the [Consuming Data guide](consuming-messages.md).
    
3. Now let's define the table in which you are storing the data as follows.
    `define table ShipmentDetails(transRef long, material string, supplier string, amount double);`
    
4. To save the data in the required database, connect an inline store definition to the table definition via the `@store` annotation as follows
    `
    @primaryKey('transRef')
    @index('supplier')
    @store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/FactoryMaterialDB, username="<YOUR_USERNAME>", password="<YOUR_PASSWORD>" , jdbc.driver.name="com.mysql.jdbc.Driver")
    define table ShipmentDetails(transRef long, material string, supplier string, amount double);
    `
   Here, the `jdbc.url` parameter connects the `ShipmentDetails` table to the `FactoryMaterialDB` database that you previously created. The value for the `transRef` parameter is unique for each purchase event. Therefore, it is specified as the primary key for the table. The records are indexed by the supplier.
    
5. To store the information arriving at the `RawMaterialStream` stream to the `ShipmentDetails` table, write a Siddhi query as follows:
    1. To specify that events to be stored are taken from the `RawMaterialStream` input stream, add the `from` clause as follows.
        `from RawMaterialStream`
        
    2. To derive the exact attribute names with which the records are stored in the table, add the `select` statement as follows.
        `select transRef, material, supplier`
    
    3. If a record from the stream already exists in a the `ShipmentDetails` table, it should be updated with the latest 
    information from the stream. If it does not already exist, it must be inserted as a new record. To specify this, add an `update and insert` clause as follows.
        `update or insert into ShipmentDetails on ShipmentDetails.transRef == transRef;`
        
       Each record is identified by the transaction reference which is the primary key. Therefore, the condition `on ShipmentDetails.transRef == transRef` means that if a record with the same value for the `transRef` attribute exists in the table, the record from the stream updates it. If not, the record from the stream is inserted as a new record.



## Performing CRUD operations

###Performing CRUD operations via streams

If you tried out the example in the previous sections of this guide, you have already performed an update or insert operation 
via streams when you updated or inserted a record relating to the purchase records into the `ShipmentDetails` table. In 
this section, you can try more CRUD operations via streams as follows:

- If you want to insert all the records from the `RawMaterialStream` stream to the `ShipmentDetails` table, you can write the Siddhi query as follows.
    ```
    from RawMaterialStream
    select *
    insert into ShipmentDetails
    ```
- If you want to search for all the records by a specific supplier, you can write the Siddhi query as follows.
    ```
    from RawMaterialStream on supplier == 'Lakspur landing' in ShipmentDetails
    select *
    insert into SearchResultsStream
    ```
    
    Here, you search for records by entering the search condition in the `from` clause and directing the filtered results into an output stream named `SearchResultsStream`.

- If you want to delete all the records for a specific material, you can write the Siddhi query as follows:
    ```
    from DeleteMaterialStream on material == 'honey' in ShipmentDetails
    select *
    delete ShipmentDeails on ShipmentDetails.material == matarial;
    ```
    

###Performing CRUD operations via REST API

This section explains how to use REST API to perform the same CRUD operations that you previously performed via streams.

!!!info "Before you begin:"
    The Siddhi store query endpoint can be configured as follows:

    1. In the siddhi.stores.query.api: section of the `<SI_HOME>/conf/server/deployment.yaml` file, configure the following properties. The following is a sample configuration with default values.
        ```
        siddhi.stores.query.api:
          transportProperties:
            -
              name: "server.bootstrap.socket.timeout"
              value: 60
            -
              name: "client.bootstrap.socket.timeout"
              value: 60
            -
              name: "latency.metrics.enabled"
              value: true
           listenerConfigurations:
            -
              id: "default"
              host: "0.0.0.0"
              port: 7070
            -
              id: "msf4j-https"
              host: "0.0.0.0"
              port: 7443
              scheme: https
              keyStoreFile: "${carbon.home}/resources/security/wso2carbon.jks"
              keyStorePassword: wso2carbon
              certPass: wso2carbon
        ```    
        - **transport properties**
           - `server.bootstrap.socket.timeout`: The number of seconds after which the connection socket of the bootstrap server times out.
           - `client.bootstrap.socket.timeout`: The number of seconds after which the connection socket of the bootstrap server times out.
           - `latency.metrics.enabled`: If this is set to `true`, the latency metrics are enabled and logged for the HTTP transport.
        - **listenerConfigurations**: Multiple listeners can be configured as shown in the above sample.
           - `id`: A unique ID for the listener.
           - `host`: The host of the listener.
           - `port`: The port of the listener.
           - `scheme `: This specifies whether the transport scheme is HTTP or HTTPS.
           - `keyStoreFile`: If the transport scheme is HTTPS, this parameter specifies the path to the key store file.
           - `keyStorePassword`: If the transport scheme is HTTPS, this parameter specifies the key store password.          
    2. In the `siddhi.stores.query.api:` section of the `<SI_HOME>/conf/server/deployment.yaml` file, the following properties are configured by default:
        ```        
        siddhi.stores.query.api:
          transportProperties:
            -
              name: "server.bootstrap.socket.timeout"
              value: 60
            -
              name: "client.bootstrap.socket.timeout"
              value: 60
            -
              name: "latency.metrics.enabled"
              value: true
           listenerConfigurations:
            -
              id: "default"
              host: "0.0.0.0"
              port: 7370
        ```
        The same parameter descriptions provided for the `<SI_HOME>/conf/server/deployment.yaml ` file apply to this configuration.
        


## Change Data Capture

## Manupulating data in multiple tables



