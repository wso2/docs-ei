# Change Data Capturing for MySQL

## Introduction

The Streaming Integrator (SI) allows you to capture changes to a database table, in a streaming manner.

This tutorial takes you through the different modes and  options you could use to perform Change Data Capturing (CDC) using the SI. In this tutorial, you are using a MySQL datasource.  

#### Listening mode and Polling mode 

There are two modes in which you could perform CDC using the SI: **Listening mode** and **Polling mode**.  

In the polling mode, the datasource is periodically polled for capturing the changes. The polling period can be configured.
 
In listening mode, the SI keeps listening to the Change Log of the database and notifies if a change takes place. Here, unlike the polling mode, you are notified about the change immediately.

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
- [Polling mode](#polling-mode)
    - [Preparing server and database for this tutorial](#Preparing-server-and-database-for-this-tutorial-1)
    - [Capturing inserts](#Capturing-inserts-1)
    - [Capturing updates](#Capturing-updates-1)

## Listening mode

### Preparing server and database for this tutorial

!!!Prerequisites
    Before you begin:<br/>
    - You need to have access to a MySQL instance.<br/>
    - Enable binary logging in the MySQL server. For detailed instructions, see [Enabling the Binlog tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog). 

1. Add the MySQL JDBC driver into the `<SI_HOME>/lib` directory as follows:
    1. Download the MySQL JDBC driver from [the MySQL site](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz).
    2. Unzip the archive.
    3. Copy the `mysql-connector-java-5.1.45-bin.jar` to the `<SI_HOME>/lib` directory.
    4. Start the SI server.
    
2. Let's create a new database in the MySQL server which you are to use throughout this tutorial. To do this, execute the following query.
    ```
    CREATE SCHEMA production;
    ```  
3. Create a new user by executing the following SQL query.
    ```
    GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
    ```
4. Switch to the `production` database and create a new table, by executing the following queries:
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


