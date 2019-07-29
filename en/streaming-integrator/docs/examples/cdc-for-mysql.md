# Change Data Capturing for MySQL

## Introduction

The Streaming Integrator (SI) allows you to capture changes to a data base table, in a streaming manner.

This tutorial takes you through the different modes and  options you could use to perform Change Data Capturing (CDC) using the SI. In this tutorial, you will be using a MySQL datasource.  

#### Listening mode and Polling mode 

There are two modes you could perform CDC using the SI: **Listening mode** and **Polling mode**.  

- Polling mode: In polling mode, the datasource is periodically polled for capturing the changes. The polling period can be configured.
 
- Listening mode:  On listening mode, the SI will keep listening to the Change Log of the database and notify in case a change has taken place. Here, you are immediately notified about the change, compared to polling mode.

#### Type of events captured

You could capture following type of changes done to a database table:
- Insert operations  
- Update operations
- Delete operations (available for Listening mode only) 

## Tutorial Outline
- [Listening mode](#listening-mode)
    - [Preparing server and database for this tutorial](#Preparing-server-and-database-for-this-tutorial)
    - [Capturing inserts](#Capturing-inserts)
    - [Capturing updates](#Capturing-updates)
    - [Capturing deletes](#Capturing-deletes)   
- [Polling mode](#polling-mode)
    - [Preparing server and database for this tutorial](#Preparing-server-and-database-for-this-tutorial-1)
    - [Capturing inserts](#Capturing-inserts-1)
    - [Capturing updates](#Capturing-updates-1) 

## Listening mode

### Preparing server and database for this tutorial

1. It is assumed that you have access to a MySQL server instance. Enable binary logging in the MySQL server. You may follow [this tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog). 

2. Add the MySQL JDBC driver into `<SI_HOME>/lib` as follows:
    - Download the MySQL JDBC driver from [this link](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).
    - Unzip the archive.
    - Copy `mysql-connector-java-5.1.45-bin.jar` to `<SI_HOME>/lib` directory.
    - Start the SI server.
    
3. Let's create a new database in the MySQL server which you will be using throughout this tutorial. 
```
CREATE SCHEMA production;
```  
4. Create a new user using below SQL query.
```
GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
```
5. Change into the production database and create a new table, by executing following queries:
```
use production;
```
```
CREATE TABLE SweetProductionTable (name VARCHAR(20),amount double(10,2));
```

### Capturing inserts

Now you will write a simple Siddhi application to monitor the `SweetProductionTable` for insert operations. 

Open a text file and copy-paste following application into it.

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
select name, amount
insert into LogStream;
``` 
Here the `url` parameter has being configured to `jdbc:mysql://localhost:3306/production`. Change it to point to your MySQL server.

Save this file as `CDCListenForInserts.siddhi` into `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!!info
    Above Siddhi application will capture all the inserts done to the database table `SweetProductionTable` and log them.

Now let's perform an insert operation on the MySQL table. Execute following MySQL query on the database:
```
insert into SweetProductionTable values('chocolate',100.0);
```
You will see following log on the SI console:
``` 
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : logStream : Event{timestamp=1563200225948, data=[chocolate, 100.0], isExpired=false}
```

### Capturing updates

Now you will write a Siddhi application to monitor the `SweetProductionTable` for update operations.

Open a text file and copy-paste following application into it.
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
Save this file as `CDCListenForUpdates.siddhi` into `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!!Info
    Above Siddhi application will capture all the updates done to the database table `SweetProductionTable` and log them.

Now let's perform an update operation on the MySQL table. Execute following MySQL query on the database:
```
update SweetProductionTable SET name = 'Almond cookie' where name = 'chocolate';
```
You will see following log on the SI console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : updateSweetProductionStream : Event{timestamp=1563201040953, data=[chocolate, Almond cookie, 100.0, 100.0], isExpired=false}
```
!!!Info
    Here `before_name` attribute contains the name prior to the update: `chocolate` in this case. `name` attribute contains the current name which is `Almond cookie`.

### Capturing deletes

Now you will write a Siddhi application to monitor the `SweetProductionTable` for delete operations.

Open a text file and copy-paste following application into it.
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
Save this file as `CDCListenForDeletes.siddhi` into `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!!Info
    Above Siddhi application will capture all the delete operations done to the database table `SweetProductionTable` and log them.

Now let's perform a delete operation on the MySQL table. Execute following MySQL query on the database:
```
delete from SweetProductionTable where name = 'Almond cookie';
```
You will see following log on the SI console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : DeleteSweetProductionStream : Event{timestamp=1563367367098, data=[Almond cookie, 100.0], isExpired=false}
```
!!!Info
    Here `before_name` attribute contains the deleted name: `Almond cookie` in this case. Similarly, `before_amount` attribute contains the amount being deleted.

### Preserving State of the application through a system failure

Let's try out a scenario in which you are going to deploy a siddhi app to count the total number of productions.

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
    
    Next, you are going to insert two sweet productions into the `SweetProductionTable` and shutdown the SI server before state persistence happens (in other words, before above log appears). 
    
    !!!Tip
        It is better to start pushing messages immediately after the state persistence log appears, so that you have plenty of time to push messages and shutdown the server, until next log appears.
        
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

11. Now you will see following logs on the SI console.
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions : LogStream : Event{timestamp=1564151078607, data=[220.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions : LogStream : Event{timestamp=1564151078612, data=[320.0], isExpired=false}
    ``` 
Notice that the `CDC source` has replayed the last two messages. As a result, the sweet productions count has correctly restored.   
    
## Polling mode

### Preparing server and database for this tutorial

1. It is assumed that you have access to a MySQL server instance.
  
2. Let's create a new database in the MySQL server which you will be using throughout this tutorial. 
```
CREATE SCHEMA production_pol;
```  
3. Change into the production database and create a new table, by executing following queries:
```
use production_pol;
```
```
CREATE TABLE SweetProductionTable (last_update TIMESTAMP, name VARCHAR(20),amount double(10,2));
```  
!!!Note
    You might have already done following preparation items (step 4 and step 5) if you have tried out the 'Listening mode' section of this tutorial. If so, skip following two preparation steps. 

4. Create a new user using below SQL query.
```
GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
```
5. Add the MySQL JDBC driver into `<SI_HOME>/lib` as follows:
    - Download the MySQL JDBC driver from [this link](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).
    - Unzip the archive.
    - Copy `mysql-connector-java-5.1.45-bin.jar` to `<SI_HOME>/lib` directory.
  
### Capturing inserts
 
Now you will write a simple Siddhi application to monitor the `SweetProductionTable` for insert operations. 
 
Open a text file and copy-paste following application into it.
 
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
select name, amount
insert into LogStream;
```
Here the `url` parameter has being configured to `jdbc:mysql://localhost:3306/production_pol`. Change it to point to your MySQL server.

Save this file as `CDCPolling.siddhi` into `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

!!!Info
    Above Siddhi application will periodically poll the database, capture the changes done to the database table `SweetProductionTable` during the polled interval, and log them. The polling interval has being configured using the parameter `polling.interval` in the Siddhi application, when defining the CDC source. In this example, the polling interval is 10 seconds.

Now let's perform an insert operation on the MySQL table. Execute following MySQL query on the database:
```
insert into SweetProductionTable(name,amount) values('chocolate',100.0);
```
You will see following log on the SI console:
``` 
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithPollingMode : LogStream : Event{timestamp=1563378804914, data=[chocolate, 100.0], isExpired=false}
``` 
### Capturing Updates

You will use the same Siddhi application, `CDCPolling.siddhi`, which you deployed under 'Capturing inserts' section for capturing updates.

Let's perform an update operation on the MySQL table. Execute following MySQL query on the database:
```
update SweetProductionTable SET name = 'Almond cookie' where name = 'chocolate';
```
You will see following log on the SI console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithPollingMode : logStream : Event{timestamp=1563436388530, data=[Almond cookie, 100.0], isExpired=false}  
```
### Preserving State of the application through a system failure

Let's try out a scenario in which you are going to deploy a siddhi app to count the total number of productions.

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

11. Now you will see following logs on the SI console.
    ```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions_pol : LogStream : Event{timestamp=1564386179998, data=[220.0], isExpired=false}
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CountProductions_pol : LogStream : Event{timestamp=1564386180004, data=[320.0], isExpired=false}
    ``` 
Notice that the `CDC source` has replayed the last two messages. As a result, the sweet productions count has correctly restored.   

