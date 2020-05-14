# Streaming Data from Oracle to Kafka

## Introduction

 This tutorial focuses on a scenario where multiple applications need to capture change data of a specific database. In this scenario, you may encounter the following issues.

 - Some applications that need to capture the change data in real time may not have streaming capabilities.
 - The database server may not have the capacity to handle all the queries by multiple applications that attempt to poll the database to capture the change data.

 In this scenario, it is more feasible to allow the applications with no streaming capabilities to subscribe for a topic in a messaging system where they can be updated about the insertions, updates and deletions to the database on time. For this, the change data needs to be published to the topic in a streaming manner. This can be done via the WSO2 Streaming Integrator.

 The WSO2 Streaming Integrator can listen to change data in a database via the `cdc` extension and expose that information in a streaming manner to a Kafka topic. Other applications in turn can access this information by subscribing to the Kafka topic.

 ![streaming-data-from-oracle-to-kafka](../images/streaming-data-from-oracle-to-kafka/publish-change-data-to-kafka-topic.png)


 !!!info "Key concepts"
     **Change Data Capture**: This is a technique or a process that makes it possible to capture changes in the data in real-time. This enables real-time ETL with databases because it allows users to receive real-time notifications when data in the database tables are changing.


## Business scenario


 !!! tip "Before you begin"
     To try out this tutorial, you need to complete the following prerequisites:<br/> <br/>
     - Install Oracle and start it. <br/> <br/>
     - Create a new connection in Oracle. <br/> <br/>
     - Create a new common user named `c##_admin`. <br/> <br/>
     - Create a table named `RAW_MATERIAL_STOCK` with the folowing columns. <br/> <br/>
         |**COLUMN_NAME**|**DATA_TYPE**|
         |---------------|-------------|
         |`REFNO`        |`VARCHAR2`   |
         |`NAME`         |`VARCHAR2`   |
         |`AMOUNT`       |`NUMBER`     | <br/> <br/>|
     - In the `RAW_MATERIAL_STOCK` table, enter four records as follows.

         |**REFNO**      |**NAME**|**AMOUNT**|
         |---------------|--------|----------|
         |AAA000000001   |flour   | 10000    |
         |AAA000000002   |sugar   | 10000    |
         |AAA000000003   |honey   | 10000    |


## Designing the Siddhi application

 !!! tip "Before you begin"
     To try out this tutorial, you need to complete the following prerequisites:<br/> <br/>
     - Install Oracle and start it. <br/> <br/>
     - Create a new connection in Oracle. <br/> <br/>
     - Create a new common user named `c##_admin`. <br/> <br/>
     - Create a new database named `PRODUCTION`. <br/> <br/>
     - In the `PRODUCTION` database, create a table named `RAW_MATERIAL_STOCK`. <br/> <br/>

 To address the business scenario described, let's create a Siddhi application as follows:

 1. Open the Streaming Integrator Studio and start creating a new Siddhi application. For more information, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).

 2. Name the new Siddhi application `ExposeOracleDataToKafkaApp` via the `@App` annotation. Optionally, yoiu can also enter a description for the Siddhi application as shown below.

     ```
     @App:name("ExposeOracleDataToKafkaApp")
     @App:description("Exposes data in an Oracle database by publishing it to a Kafka topic")
     ```

 3. First let's do the configurations to capture change data from the oracle database as follows:

     1. To specify the schema with which change data is captured as events, let's define an input stream.

         ```
         define stream RetrieveStockLevelsStream (refNo string, name string, amount int);define stream RetrieveStockLevelsStream (refNo string, name string, amount int);
         ```

     2. To allow the stream you defined to get the change data from a database as an input, connect a `cdc` source to it as shown below.

         ```
         @source(type='cdc',
             url='jdbc:oracle:thin://12.34.56.78:1521/PRODUCT.EXAMPLE.COM',
             username='xstrm',
             password='xs',
             table.name='ProductCatalogue.Product',
             operation='insert',
             database.server.name='PRODINS',
         connector.properties="database.out.server.name=DBZXOUT_PROD_INS,snapshot.mode=initial_schema_only",
             @map(type = 'keyvalue', fail.on.missing.attribute='false'))
         define stream RetrieveStockLevelsStream (refNo string, name string, amount int);
         ```


 4. To publish the change data to a Kafka topic, let's define an output stream and connect a Kafka sink to it.

     1. To specify the schema with which the change data events should be published, let's add an output stream definition as follows:

         ```
         define stream PublishChangeDataStream (refNo string, name string, amount int);
         ```

     2. To publish to a Kafka topic, let's connect a Kafka sink to the stream definition as follows.

         ```
         @sink(type='kafka' , bootstrap.servers='option_value',topic='option_value',is.binary.message='option_value')
         define stream PublishChangeDataStream (refNo string, name string, amount int);
         ```

 5. Now let's write a query to insert all the change data captured into the Kafka topic.

     1. Assign `PublishChangeData` as the name of the query via the `@info` annotation as shown below.

         `@info(name='SweetTotalQuery')`

     2. To get the CDC events received into the StockLevelsStream stream as the input events, add a `from` clause as follows:

         `from RetrieveStockLevelsStream`

     3. To publish all the CDC events that you have received into the Kafka topic, add the `select` clause as follows.

         `select *`

     4. To publish the CDC events to the Kafka topic, direct them to the `PublishChangeDataStream` to which you previously connected the relevant Kafka sink (in step 2), add the `insert into` clause as follows.

         `insert into PublishChangeDataStream;`

     The complete query is as follows:

         ```
         @info(name='SweetTotalQuery')
         from RetrieveStockLevelsStream
         select *
         insert into PublishChangeDataStream;
         ```

 6. Save the Siddhi application. It currently looks as follows.

     ```
     @App:name("ExposeOracleDataToKafkaApp")


     @source(type='cdc',
         url='jdbc:oracle:thin://12.34.56.78:1521/PRODUCT.EXAMPLE.COM',
         username='xstrm',
         password='xs',
         table.name='ProductCatalogue.Product',
         operation='insert',
         database.server.name='PRODINS',
     connector.properties="database.out.server.name=DBZXOUT_PROD_INS,snapshot.mode=initial_schema_only",
         @map(type = 'keyvalue', fail.on.missing.attribute='false'))
     define stream RetrieveStockLevelsStream (refNo string, name string, amount int);


     @sink(type='kafka' , bootstrap.servers='option_value',topic='option_value',is.binary.message='option_value')
     define stream PublishChangeDataStream (refNo string, name string, amount int);



     @info(name='SweetTotalQuery')
     from RetrieveStockLevelsStream
     select *
     insert into PublishChangeDataStream;
     ```

### Testing the application

 To test the `ExposeOracleDataToKafkaApp` Siddhi application you created,





### Enhancing change data before publishing


