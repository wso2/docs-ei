# Siddhi Application Overview

A Siddhi application (.siddhi) file is the deployment artifact containing the Stream Processing logic for WSO2 Streaming Integrator.

The format of a Siddhi application is as follows:

``` sql
    @App:name("ReceiveAndCount")
    @App:description('Receive events via HTTP transport and view the output on the console')
    
    /* 
        Sample Siddhi App block comment
    */
    
    -- Sample Siddhi App line comment
    
    @Source(type = 'http',
            receiver.url='http://localhost:8006/productionStream',
            basic.auth.enabled='false',
            @map(type='json'))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type='log')
    define stream TotalCountStream (totalCount long);
    
    -- Count the incoming events
    @info(name='query1')
    from SweetProductionStream
    select count() as totalCount
    insert into TotalCountStream;
```

## Basic information about Siddhi applications

Following are some important things to note about Siddhi applications:

-   The file name of each Siddhi application must be equal to the name specified via the `@App:name()` annotation.  
    e.g., In the sample Siddhi application given above, the application name is `ReceiveAndCount`. Therefore, the Siddhi file name must be `ReceiveAndCount.Siddhi`.

-   It is optional to provide a description via the `@App:description()` annotation.

-   The definitions of the required streams, windows, tables, triggers and aggregations need to be included before the Siddhi queries.  
    e.g., In the above sample Siddhi file, the streams  (lines 14 and 17) are defined before the queries (lines 21-23).
    
-   Siddhi can infer the definition of the streams. It is not required to define all the streams. However, if annotations need to be added to a stream, that stream must be defined.
    
-   In the above sample, lines 4-6 nd 8 demonstrate how to include comments within Siddhi applications.

For more information about Siddhi applications, see [Siddhi Application at Siddhi Streaming SQL Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#siddhi-application).

## Common elements of a Siddhi application

This section explains the common types of definitions and queries that are included in  Siddhi application:

### Queries

Queries define the logical processing and selections that must be executed for streaming events. They consume from the pre-defined streams/ windows/ tables/ aggregations, process them in a streaming manner, and insert the output to another stream, window or table. For more information about Siddhi queries, see [Queries at Siddhi Streaming SQL Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#query).

### Streams

Streams are one of the core elements of a stream processing application. A stream is a logical series of events ordered in time with a uniquely identifiable name and set of defined attributes with specific data types defining its schema. In Siddhi, streams are defined by giving it a name and the set of attributes it contains. Lines 14 and 17 of the above sample are examples of defined streams. For more information on Siddhi streams, see [Streams at Siddhi Streaming SQL Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#stream).

### Tables

A table is a collection of events that can be used to store streaming data. The capability to store events in a table allows you to query for stored events later or process them again with a different stream. Thegeneric table concept holds here as well, however, Siddhi tables also support numerous table specific data manipulations such as defining primary keys, indexing, etc. For more information on Siddhi tables, see [Storage Integration](_Storage_Integration_) and [Tables at Siddhi Streaming SQL Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#table).

### Windows

Windows allow you to retain a collection of streaming events based on a time duration (time window), or a given number of events (length window). It allows you to process events that fall into the defined window or expire from it. For more information on Siddhi windows, see [Windows at Siddhi Streaming SQL Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#defined-window).

### Aggregations

Aggregation allows you to aggregate streaming events for different time granularities. The time granularities supported are seconds, minutes, hours, days, months and years. Aggregations such as sum, min, avg can be calculated for the desired duration(s) via Siddhi aggregation. For more information on Siddhi aggregations, see [Aggregations at Siddhi Streaming SQL Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#incremental-aggregation).

#### Persisted Aggregations.

  Note : "This capability is released as a product update on 11/06/2021. If you don't already have this update, you can [get the latest updates](https://updates.docs.wso2.com/en/latest/updates/overview/#!) now.

  With Persisted aggregation, the aggregation for higher granularities will be executing on top of the database at the end of each time granularity(Day - at the end of the day, Month - at the end of the month, Year - at the end of the year).
  This is the recommended approach if the aggregation group by elements have lots of unique combinations.

  * Enabling Persisted Aggregation

    The persisted aggregation can be enabled by adding the @persistedAggregation(enable="true") annotation on top of the aggregation definition.
    Furthermore, in order to execute the aggregation query, the cud function which is there in siddhi-store-rdbms is used. So in order to enable the "cud" operations, please add the following configuration on the deployment.yaml file.

    ```
       siddhi:
         extensions:
           -
             extension:
               name: cud
               namespace: rdbms
               properties:
                 perform.CUD.operations: true
    ```

    In order to use persisted aggregation, A datasource needs to configured through deployment.yaml file and it should be pointed out in @store annotation of the aggregation definition.

    Furthermore when using persisted aggregation with MySQL, please provide the aggregation processing timezone in JDBC URL since by default MySQL database will use server timezone for some time-related conversions which are there in an aggregation query.

    ```
        jdbc:mysql://localhost:3306/TEST_DB?useSSL=false&tz=useLegacyDatetimeCode=false&serverTimezone=UTC
    ```

    Also when using persisted aggregation with Oracle, add below configuration in the datasource configuration,

    ```
    connectionInitSql: alter session set NLS_DATE_FORMAT='YYYY-MM-DD HH24:MI:SS'
       
    eg:
       
     - name: APIM_ANALYTICS_DB
       description: "The datasource used for APIM statistics aggregated data."
       jndiConfig:
         name: jdbc/APIM_ANALYTICS_DB
         definition:
           type: RDBMS
           configuration:
             jdbcUrl: 'jdbc:oracle:thin:@localhost:1521:XE'
             username: 'root'
             password: '123'
             driverClassName: oracle.jdbc.OracleDriver
             maxPoolSize: 50
             idleTimeout: 60000
             connectionTestQuery: SELECT 1 FROM DUAL
             connectionInitSql: alter session set NLS_DATE_FORMAT='YYYY-MM-DD HH24:MI:SS'
             validationTimeout: 30000
             isAutoCommit: false
               
    ```
    For an example please refer to the following query which will be executed on the database to update the table for below sample Aggregation ,

    ```
    @persistedAggregation(enable="true")
    define aggregation ResponseStreamAggregation
    from processedResponseStream
    select api, version, apiPublisher, applicationName, protocol, consumerKey, userId, context, resourcePath, responseCode, 
        avg(serviceTime) as avg_service_time, avg(backendTime) as avg_backend_time, sum(responseTime) as total_response_count, time1, epoch, eventTime
    group by api, version, apiPublisher, applicationName, protocol, consumerKey, userId, context, resourcePath, responseCode, time1, epoch
    aggregate by eventTime every min;
    
    ```
  

The elements mentioned above work together in a Siddhi application to form an event flow. To understand how the elements os a Siddhi application are interconnected, you can view the design view of a Siddhi application. For more information, see [Stream Processor Studio Overview](https://docs.wso2.com/display/SP440/Stream+Processor+Studio+Overview#StreamProcessorStudioOverview-Open).

  
