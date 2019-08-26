# Change Data Capturing for MySQL

## Introduction

The Streaming Integrator (SI) allows you to capture changes to a database table, in a streaming manner.

This tutorial takes you through the different modes and  options you could use to perform Change Data Capturing (CDC) using the SI. In this tutorial, you are using a MySQL datasource.  

!!!Tip
    To use a different database other than MySQL, refer [dependencies for CDC](https://github.com/siddhi-io/siddhi-io-cdc#dependencies) and add the corresponding driver jar. In addition to that, modify the JDBC URL accordingly, in `url` parameter in all Siddhi applications given in this tutorial.

#### Listening mode and Polling mode 

There are two modes in which you could perform CDC using the SI: **Listening mode** and **Polling mode**.  

- Polling mode: In the polling mode, the datasource is periodically polled for capturing the changes. The polling period can be configured.
 
- Listening mode: In listening mode, the SI keeps listening to the Change Log of the database and notifies if a change takes place. Here, unlike the polling mode, you are notified about the change immediately.

#### Type of events captured

You can capture following type of changes done to a database table:
- Insert operations  
- Update operations
- Delete operations (available for Listening mode only) 

## Tutorial Outline
- [Listening mode](#listening-mode)
    - [Preparing server and database for this tutorial](#Preparing-server-and-database-for-this-tutorial)
    - [Capturing inserts](#Capturing-inserts)
    - [Capturing updates](#Capturing-updates)
    - [Capturing deletes](#Capturing-deletes)   
    - [Preserving State of the application through a system failure](#Preserving-State-of-the-application-through-a-system-failure)
- [Polling mode](#polling-mode)
    - [Preparing server and database for this tutorial](#Preparing-server-and-database-for-this-tutorial-1)
    - [Capturing inserts](#Capturing-inserts-1)
    - [Capturing updates](#Capturing-updates-1)
    - [Preserving State of the application through a system failure](#Preserving-State-of-the-application-through-a-system-failure-1)

## Listening mode

### Preparing server and database for this tutorial

!!!Prerequisites
    Before you begin:<br/>
    - You need to have access to a MySQL instance.<br/>
    - Enable binary logging in the MySQL server. For detailed instructions, see [Enabling the Binlog tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog). 
    - Add the MySQL JDBC driver into the `<SI_HOME>/lib` directory as follows:
      1. Download the MySQL JDBC driver from [the MySQL site](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).
      2. Unzip the archive.
      3. Copy the `mysql-connector-java-5.1.45-bin.jar` to the `<SI_HOME>/lib` directory.
      4. Start the SI server.

    
1. Let's create a new database in the MySQL server which you are to use throughout this tutorial. To do this, execute the following query.
    ```
    CREATE SCHEMA production;
    ```  
2. Create a new user by executing the following SQL query.
    ```
    GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
    ```
3. Switch to the `production` database and create a new table, by executing the following queries:
    ```
    use production;
    ```
    ```
    CREATE TABLE SweetProductionTable (name VARCHAR(20),amount double(10,2));
    ```

### Capturing inserts

Now you can write a simple Siddhi application to monitor the `SweetProductionTable` for insert operations. 

1. Open a text file and copy-paste following application into it.

    ``` 
    @App:name('CDCListenForInserts')
    
    @App:description('Capture MySQL Inserts using CDC listening mode.')
    
    @source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2si', password = 'wso2', table.name = 'SweetProductionTable', operation = 'insert', 
        @map(type = 'keyvalue'))
    define stream InsertSweetProductionStream (name string, amount double);
    
    @sink(type = 'log')
    define stream LogStream (name string, amount double);
    
    @info(name = 'query')
    from InsertSweetProductionStream
    select *
    insert into LogStream;
    ``` 
    Here the `url` parameter has the value `jdbc:mysql://localhost:3306/production`. Change it to point to your MySQL server.

2. Save this file as `CDCListenForInserts.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.
    
    !!!info
        This Siddhi application captures all the inserts made to the `SweetProductionTable` database table and logs them.

3. Now let's perform an insert operation on the MySQL table by executing the following MySQL query on the database:
    ```
    insert into SweetProductionTable values('chocolate',100.0);
    ```
    The following log appears in the SI console:
    ``` 
    INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : logStream : Event{timestamp=1563200225948, data=[chocolate, 100.0], isExpired=false}
    ```

### Capturing updates

Now you can write a Siddhi application to monitor the `SweetProductionTable` for update operations.

1. Open a text file and copy-paste following application into it.
    ``` 
    @App:name('CDCListenForUpdates')
    
    @App:description('Capture MySQL Updates using CDC listening mode.')
    
    @source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2si', password = 'wso2', table.name = 'SweetProductionTable', operation = 'update', 
        @map(type = 'keyvalue'))
    define stream UpdateSweetProductionStream (before_name string, name string, before_amount double, amount double);
    
    @sink(type = 'log')
    define stream LogStream (before_name string, name string, before_amount double, amount double);
    
    @info(name = 'query')
    from UpdateSweetProductionStream
    select *
    insert into LogStream;
    ``` 
2. Save this file as `CDCListenForUpdates.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application captures all the updates to the `SweetProductionTable` database table and logs them.


3. Now let's perform an update operation on the MySQL table. For this, execute following MySQL query on the database:
    ```
    update SweetProductionTable SET name = 'Almond cookie' where name = 'chocolate';
    ```
   As a result, you can see the following log in the SI console.
    ```
    INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : updateSweetProductionStream : Event{timestamp=1563201040953, data=[chocolate, Almond cookie, 100.0, 100.0], isExpired=false}
    ```
    
    !!!info
        Here, the `before_name1` attribute indicates the value of the `name` attribute before the update was made (`chocolate` in this case), and the `name` attribute has the current name after the update (i.e., `almond cookie`).

### Capturing deletes

Now you can write a Siddhi application to monitor the `SweetProductionTable` for delete operations.

1. Open a text file and copy-paste following application into it.
    ``` 
    @App:name('CDCListenForDeletes')
    
    @App:description('Capture MySQL Deletes using CDC listening mode.')
    
    @source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2si', password = 'wso2', table.name = 'SweetProductionTable', operation = 'delete', 
        @map(type = 'keyvalue'))
    define stream DeleteSweetProductionStream (before_name string, before_amount double);
    
    @sink(type = 'log')
    define stream LogStream (before_name string, before_amount double);
    
    @info(name = 'query')
    from DeleteSweetProductionStream
    select *
    insert into LogStream;
    ```
2. Save this file as `CDCListenForDeletes.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application captures all the delete operations carried out for the `SweetProductionTable` database table and logs them.

3. Now let's perform a delete operation for the MySQL table. To do this, execute following MySQL query on the database:
    ```
    delete from SweetProductionTable where name = 'Almond cookie';
    ```
    The following log appears in the SI console:
    ```
    INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : DeleteSweetProductionStream : Event{timestamp=1563367367098, data=[Almond cookie, 100.0], isExpired=false}
    ```
    
    !!!info
        Here, the `before_name` attribute indicates the name of the sweet in the deleted record (i.e., `Almond cookie` in this case). Similarly, the `before_amount` indicates the amount in the deleted record.

### Preserving State of the application through a system failure

Let's try out a scenario in which you are going to deploy a Siddhi application to count the total number of productions.

!!!info
    In this scenario, the current count should be "remembered" by the SI server through system failures, so that when the system is restored, the count is not reset to zero. 
    To achieve this, you can use the state persistence capability in the Streaming Integrator.

1. Enable state persistence feature in SI server as follows. Open the `<SI_HOME>/conf/server/deployment.yaml` file on a text editor and locate the `state.persistence` section.  

    ``` 
      # Periodic Persistence Configuration
    state.persistence:
      enabled: true
      intervalInMin: 1
      revisionsToKeep: 2
      persistenceStore: org.wso2.carbon.streaming.integrator.core.persistence.FileSystemPersistenceStore
      config:
        location: siddhi-app-persistence
    ```   
    Set `enabled` parameter to `true` and save the file. 

2. Enable state persistence debug logs as follows. Open the `<SI_HOME>/conf/server/log4j2.xml` file on a text editor and locate following line in it.
    ```
     <Logger name="com.zaxxer.hikari" level="error"/>
    ``` 
    Add following `<Logger>` element below that.
    ```
    <Logger name="org.wso2.carbon.streaming.integrator.core.persistence" level="debug"/>
    ```
    Save the file.

3. Restart the Streaming Integrator server for above change to be effective.

4. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name('CountProductions')
    
    @App:description('Siddhi application to count the total number of orders.')
    
    @source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2si', password = 'wso2', table.name = 'SweetProductionTable', operation = 'insert',
            @map(type = 'keyvalue'))
    define stream InsertSweetProductionStream (name string, amount double);
    
    @sink(type = 'log')
    define stream LogStream (totalProductions double);
    
    @info(name = 'query')
    from InsertSweetProductionStream
    select sum(amount) as totalProductions
    insert into LogStream;
    ```

5. Save this file as `CountProductions.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory. When the 
   Siddhi application is successfully deployed, the following `INFO` log appears in the Streaming Integrator console.
    ```
    INFO {org.wso2.carbon.stream.processor.core.internal.StreamProcessorService} - Siddhi App CountProductions deployed successfully
    ``` 
6. Now let's perform a few insert operations on the MySQL table. Execute following MySQL queries on the database:
    ```
    insert into SweetProductionTable values('Almond cookie',100.0);
    ```   
    ```
    insert into SweetProductionTable values('Baked alaska',20.0);
    ```  
    Now you will see following logs on the SI console.
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions : LogStream : Event{timestamp=1564151034866, data=[100.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions : LogStream : Event{timestamp=1564151037870, data=[120.0], isExpired=false}
    ```  
    These logs print the sweet production count. Notice that the current count of sweet productions is being printed as `120` in the second log. This is because we have so far produced `120` sweets: `100` Almond cookies and `20` Baked alaskas.
    
7. Now wait for following log to appear on the SI console
    ```
    DEBUG {org.wso2.carbon.streaming.integrator.core.persistence.FileSystemPersistenceStore} - Periodic persistence of CountProductions persisted successfully
    ```
    This log indicates that the current state of the Siddhi application is successfully persisted. Siddhi application state is persisted every minute, hence you will notice this log appearing every minute.
    
    Next, you are going to insert two sweet productions into the `SweetProductionTable` and shutdown the SI server before the state persistence happens (in other words, before above log appears). 
    
    !!!Tip
        It is better to start inserting records immediately after the state persistence log appears, so that you have plenty of time to push messages and shutdown the server, until next log appears.
        
8. Now insert following sweets into the `SweetProductionTable` by executing following queries on the database :
    ```
    insert into SweetProductionTable values('Croissant',100.0);
    ```
    ```    
    insert into SweetProductionTable values('Croutons',100.0);
    ```         
9. Shutdown SI server. Here we deliberately create a scenario where the server crashes before the SI server could persist the latest production count. 
    
    !!!Info
        As the SI server crashed before the state is persisted, the SI server could not persist the latest count (which should include the last two productions `100` Croissants and `100` Croutons). The good news is, `CDC source` will replay the last two messages, hence recovering successfully from the server crash.
 
10. Restart the SI server and wait for about one minute.

11. The following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions : LogStream : Event{timestamp=1564151078607, data=[220.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions : LogStream : Event{timestamp=1564151078612, data=[320.0], isExpired=false}
    ``` 
Notice that the `CDC source` has replayed the last two messages. As a result, the sweet productions count has being correctly restored.

## Polling mode

### Preparing server and database for this tutorial

!!!Prerequisites
    Before you begin, you need to have access to a MySQL instance.

  
1. Let's create a new database in the MySQL server which you are to use throughout this tutorial. To do this, issue the following command.
    ```
    CREATE SCHEMA production_pol;
    ```  
2. Switch to the production database and create a new table by executing following queries.
    ```
    use production_pol;
    ```
    ```
    CREATE TABLE SweetProductionTable (last_update TIMESTAMP, name VARCHAR(20),amount double(10,2));
    ```  
    
    !!!Note
        If you have already carried out the next two preparation steps (i.e., steps 3 and 4) when you tried out the [Listening Mode section](#listening-mode), you can skip them.
    

3. Create a new user by executing the following SQL query.
    ```
    GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
    ```
4. Add the MySQL JDBC driver into `<SI_HOME>/lib` as follows:
    1. Download the MySQL JDBC driver from [the MySQL site](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).
    2. Unzip the archive.
    3. Copy the `mysql-connector-java-5.1.45-bin.jar` to the `<SI_HOME>/lib` directory.
  
### Capturing inserts
 
Now you can write a simple Siddhi application to monitor the `SweetProductionTable` table for insert operations. 
 
1. Open a text file and copy-paste the following application into it.
 
    ``` 
    @App:name("CDCPolling")
    
    @App:description("Capture MySQL changes, using CDC source - polling mode.") 
    
    @source(type = 'cdc',
        url = 'jdbc:mysql://localhost:3306/production_pol?useSSL=false',
        mode = 'polling',
        jdbc.driver.name = 'com.mysql.jdbc.Driver',
        polling.column = 'last_update',
        polling.interval = '10',
        username = 'wso2si',
        password = 'wso2',
        table.name = 'SweetProductionTable',
        @map(type = 'keyvalue' ))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type = 'log')
    define stream LogStream (name string, amount double);
    
    @info(name = 'query')
    from SweetProductionStream
    select *
    insert into LogStream;
    ```
    Here the `url` parameter currently specifies the URL `jdbc:mysql://localhost:3306/production_pol`. Change it to point to your MySQL server.

2. Save this file as `CDCPolling.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

    !!!info
        This Siddhi application polls the database periodically, captures the changes made to the `SweetProductionTable`
         database table during the polled interval and logs them. The polling interval is specified via the `polling.interval`
         parameter in the Siddhi application when defining the CDC source. In this example, the polling interval is 10 seconds.


3. Now let's perform an insert operation on the MySQL table. To do this, execute following MySQL query on the database.
    ```
    insert into SweetProductionTable(name,amount) values('chocolate',100.0);
    ```
    The following log appears in the SI console:
    ``` 
    INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithPollingMode : LogStream : Event{timestamp=1563378804914, data=[chocolate, 100.0], isExpired=false}
    ``` 
### Capturing Updates

For capturing updates, you can use the same Siddhi application `CDCPolling.siddhi` which you deployed in the [Capturing inserts](#capturing-inserts_1) section.

Let's perform an update operation on the MySQL table. To do this, execute following MySQL query on the database:
```
update SweetProductionTable SET name = 'Almond cookie' where name = 'chocolate';
```

The following log appears in the SI console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithPollingMode : logStream : Event{timestamp=1563436388530, data=[Almond cookie, 100.0], isExpired=false}  
```

### Preserving State of the application through a system failure

Let's try out a scenario in which you are going to deploy a Siddhi app to count the total number of productions.

!!!info
    In this scenario, the current count should be "remembered" by the SI server through system failures, so that when the system is restored, the count is not reset to zero. 
    To achieve this, you can use the state persistence capability in the Streaming Integrator.

1. Enable state persistence feature in SI server as follows. Open the `<SI_HOME>/conf/server/deployment.yaml` file on a text editor and locate the `state.persistence` section.  

    ``` 
      # Periodic Persistence Configuration
    state.persistence:
      enabled: true
      intervalInMin: 1
      revisionsToKeep: 2
      persistenceStore: org.wso2.carbon.streaming.integrator.core.persistence.FileSystemPersistenceStore
      config:
        location: siddhi-app-persistence
    ```   
    Set `enabled` parameter to `true` and save the file. 

2. Enable state persistence debug logs as follows. Open the `<SI_HOME>/conf/server/log4j2.xml` file on a text editor and locate following line in it.
    ```
     <Logger name="com.zaxxer.hikari" level="error"/>
    ``` 
    Add following `<Logger>` element below that.
    ```
    <Logger name="org.wso2.carbon.streaming.integrator.core.persistence" level="debug"/>
    ```
    Save the file.

3. Restart the Streaming Integrator server for above change to be effective.

4. Open a text file and copy-paste following Siddhi application to it.
    ```
    @App:name("CountProductions_pol")
    
    @App:description("Siddhi application to count the total number of orders.")
    
    @source(type = 'cdc',
        url = 'jdbc:mysql://localhost:3306/production_pol?useSSL=false',
        mode = 'polling',
        jdbc.driver.name = 'com.mysql.jdbc.Driver',
        polling.column = 'last_update',
        polling.interval = '10',
        username = 'wso2si',
        password = 'wso2',
        table.name = 'SweetProductionTable',
        @map(type = 'keyvalue' ))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type = 'log')
    define stream LogStream (totalProductions double);
    
    @info(name = 'query')
    from SweetProductionStream
    select sum(amount) as totalProductions
    insert into LogStream;
    ```

5. Save this file as `CountProductions_pol.siddhi` in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory. When the 
   Siddhi application is successfully deployed, the following `INFO` log appears in the Streaming Integrator console.
    ```
    INFO {org.wso2.carbon.stream.processor.core.internal.StreamProcessorService} - Siddhi App CountProductions_pol deployed successfully
    ``` 
6. Now let's perform a few insert operations on the MySQL table. Execute following MySQL queries on the database:
    ```
    insert into SweetProductionTable(name,amount) values('Almond cookie',100.0);
    ```   
    ```
    insert into SweetProductionTable(name,amount) values('Baked alaska',20.0);
    ```  
    Now you will see following logs on the SI console.
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions_pol : LogStream : Event{timestamp=1564385971323, data=[100.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions_pol : LogStream : Event{timestamp=1564386011344, data=[120.0], isExpired=false}
    ```  
    These logs print the sweet production count. Notice that the current count of sweet productions is being printed as `120` in the second log. This is because we have so far produced `120` sweets: `100` Almond cookies and `20` Baked alaskas.
    
7. Now wait for following log to appear on the SI console
    ```
    DEBUG {org.wso2.carbon.streaming.integrator.core.persistence.FileSystemPersistenceStore} - Periodic persistence of CountProductions persisted successfully
    ```
    This log indicates that the current state of the Siddhi application is successfully persisted. Siddhi application state is persisted every minute, hence you will notice this log appearing every minute.
    
    Next, you are going to insert two sweet productions into the `SweetProductionTable` and shutdown the SI server before state persistence happens (in other words, before above log appears). 
    
    !!!Tip
        It is better to start pushing messages immediately after the state persistence log appears, so that you have plenty of time to push messages and shutdown the server, until next log appears.
        
8. Now insert following sweets into the `SweetProductionTable` by executing following queries on the database :
    ```
    insert into SweetProductionTable(name,amount) values('Croissant',100.0);
    ```
    ```    
    insert into SweetProductionTable(name,amount) values('Croutons',100.0);
    ```         
9. Shutdown SI server. Here we deliberately create a scenario where the server crashes before the SI server could persist the latest production count. 
    
    !!!Info
        As the SI server crashed before the state is persisted, the SI server could not persist the latest count (which should include the last two productions `100` Croissants and `100` Croutons). The good news is, `CDC source` will replay the last two messages, hence recovering successfully from the server crash.
 
10. Restart the SI server and wait for about one minute.

11. The following log appears in the SI console:
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions_pol : LogStream : Event{timestamp=1564386179998, data=[220.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions_pol : LogStream : Event{timestamp=1564386180004, data=[320.0], isExpired=false}
    ``` 
Notice that the `CDC source` has replayed the last two messages. As a result, the sweet productions count has correctly restored.
