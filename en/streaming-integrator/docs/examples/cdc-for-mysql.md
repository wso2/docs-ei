# Change Data Capturing for MySQL

## Introduction

Streaming Integrator allows you to capture changes to a database table, in a streaming way.

This tutorial takes you through the different modes and  options you could use to perform Change Data Capturing (CDC) using the Streaming Integrator. In this tutorial, we will be using a MySQL datasource.  

#### Listening mode and Polling mode 

There are two modes you could perform CDC using the Streaming Integrator: **Listening mode** and **Polling mode**.  

In polling mode, the datasource is periodically polled for capturing the changes. The polling period can be configured.
 
On listening mode, the Streaming Integrator will keep listening to the Change Log of the database and notify in case a change has taken place. Here, you are immediately notified about the change, compared to polling mode.

#### Type of events captured

You could capture following type of changes done to a database table:
- Insert operations  
- Update operations
- Delete operations (available for Listening mode only) 

## Tutorial Outline
- Listening mode
    - Preparing server and database for this tutorial
    - Capturing inserts
    - Capturing updates
    - Capturing deletes   
- Polling mode
    - Preparing server and database for this tutorial
    - Capturing inserts
    - Capturing updates 

## Listening mode

### Preparing server and database for this tutorial

1. It is assumed that you have access to a MySQL server instance. Enable binary logging in the MySQL server. You may follow https://debezium.io/docs/connectors/mysql/#enabling-the-binlog. 

2. Add the MySQL JDBC driver into {WSO2SPHome}/lib as follows:
    - Download the MySQL JDBC driver from: https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz
    - Unzip the archive.
    - Copy mysql-connector-java-5.1.45-bin.jar to {WSO2SPHome}/lib directory.
    - Start the Stream Processor server.
    
3. Let's create a new database in the MySQL server which we will be using throughout this tutorial. 
```
CREATE SCHEMA production;
```  
4. Create a new user using below SQL query.
```
GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2sp' IDENTIFIED BY 'wso2';
```
5. Change into the production database and create a new table, by executing following queries:
```
use production;
```
```
CREATE TABLE SweetProductionTable (name VARCHAR(20),amount double(10,2));
```

### Capturing inserts

Now we will write a simple Siddhi app to monitor the `SweetProductionTable` for insert operations. 

Open a text file and copy-paste following app into it.

``` 
@App:name('CDCListenForInserts')

@App:description('Capture MySQL Inserts using CDC listening mode.')

@source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2sp', password = 'wso2', table.name = 'SweetProductionTable', operation = 'insert', 
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

Save this file as `CDCListenForInserts.siddhi` into {WSO2SPHome}/wso2/worker/deployment/siddhi-files directory.

> **_INFO:_** Above Siddhi app will capture all the inserts done to the database table `SweetProductionTable` and log them.

Now let's perform an insert operation on the MySQL table. Execute following MySQL query on the database:
```
insert into SweetProductionTable values('chocolate',100.0);
```
You will see following log on the SP console:
``` 
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : logStream : Event{timestamp=1563200225948, data=[chocolate, 100.0], isExpired=false}
```

### Capturing updates

Now we will write a Siddhi app to monitor the `SweetProductionTable` for update operations.

Open a text file and copy-paste following app into it.
``` 
@App:name('CDCListenForUpdates')

@App:description('Capture MySQL Updates using CDC listening mode.')

@source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2sp', password = 'wso2', table.name = 'SweetProductionTable', operation = 'update', 
	@map(type = 'keyvalue'))
define stream UpdateSweetProductionStream (before_name string, name string, before_amount double, amount double);

@sink(type = 'log')
define stream LogStream (before_name string, name string, before_amount double, amount double);

@info(name = 'query')
from UpdateSweetProductionStream
select name, amount
insert into LogStream;
``` 
Save this file as `CDCListenForUpdates.siddhi` into {WSO2SPHome}/wso2/worker/deployment/siddhi-files directory.

> **_INFO:_** Above Siddhi app will capture all the updates done to the database table `SweetProductionTable` and log them.

Now let's perform an update operation on the MySQL table. Execute following MySQL query on the database:
```
update SweetProductionTable SET name = 'Almond cookie' where name = 'chocolate';
```
You will see following log on the SP console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : updateSweetProductionStream : Event{timestamp=1563201040953, data=[chocolate, Almond cookie, 100.0, 100.0], isExpired=false}
```
> **_INFO:_** Here `before_name` attribute contains the name prior to the update: `chocolate` in this case. `name` attribute contains the current name which is `Almond cookie`.

### Capturing deletes

Now we will write a Siddhi app to monitor the `SweetProductionTable` for delete operations.

Open a text file and copy-paste following app into it.
``` 
@App:name('CDCListenForDeletes')

@App:description('Capture MySQL Deletes using CDC listening mode.')

@source(type = 'cdc', url = 'jdbc:mysql://localhost:3306/production', username = 'wso2sp', password = 'wso2', table.name = 'SweetProductionTable', operation = 'delete', 
	@map(type = 'keyvalue'))
define stream DeleteSweetProductionStream (before_name string, before_amount double);

@sink(type = 'log')
define stream LogStream (before_name string, before_amount double);

@info(name = 'query')
from DeleteSweetProductionStream
select name, amount
insert into LogStream;
```
Save this file as `CDCListenForDeletes.siddhi` into {WSO2SPHome}/wso2/worker/deployment/siddhi-files directory.

> **_INFO:_** Above Siddhi app will capture all the delete operations done to the database table `SweetProductionTable` and log them.

Now let's perform a delete operation on the MySQL table. Execute following MySQL query on the database:
```
delete from SweetProductionTable where name = 'Almond cookie';
```
You will see following log on the SP console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithListeningMode : DeleteSweetProductionStream : Event{timestamp=1563367367098, data=[Almond cookie, 100.0], isExpired=false}
```
> **_INFO:_** Here `before_name` attribute contains the deleted name: `Almond cookie` in this case. Similarly, `before_amount` attribute contains the amount being deleted.

## Polling mode

### Preparing server and database for this tutorial

1. It is assumed that you have access to a MySQL server instance.
  
2. Let's create a new database in the MySQL server which we will be using throughout this tutorial. 
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
> **_NOTE:_** You might have already done following preparation items (step 4 and step 5) if you have tried out the 'Listening mode' section of this tutorial. If so, skip following two preparation steps. 

4. Create a new user using below SQL query.
```
GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2sp' IDENTIFIED BY 'wso2';
```
5. Add the MySQL JDBC driver into {WSO2SPHome}/lib as follows:
    - Download the MySQL JDBC driver from: https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz
    - Unzip the archive.
    - Copy mysql-connector-java-5.1.45-bin.jar to {WSO2SPHome}/lib directory.
  
### Capturing inserts
 
Now we will write a simple Siddhi app to monitor the `SweetProductionTable` for insert operations. 
 
Open a text file and copy-paste following app into it.
 
``` 
@App:name("CDCPolling")

@App:description("Capture MySQL changes, using CDC source - polling mode.") 

@source(type = 'cdc',
	url = 'jdbc:mysql://localhost:3306/production_pol?useSSL=false',
	mode = 'polling',
	jdbc.driver.name = 'com.mysql.jdbc.Driver',
	polling.column = 'last_update',
	polling.interval = '10',
	username = 'wso2sp',
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

Save this file as `CDCPolling.siddhi` into {WSO2SPHome}/wso2/worker/deployment/siddhi-files directory.

> **_INFO:_** Above Siddhi app will periodically poll the database, capture the changes done to the database table `SweetProductionTable` during the polled interval, and log them. The polling interval has being configured using the parameter `polling.interval` in the Siddhi app, when defining the CDC source. In this example, the polling interval is 10 seconds.

Now let's perform an insert operation on the MySQL table. Execute following MySQL query on the database:
```
insert into SweetProductionTable(name,amount) values('chocolate',100.0);
```
You will see following log on the SP console:
``` 
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithPollingMode : LogStream : Event{timestamp=1563378804914, data=[chocolate, 100.0], isExpired=false}
``` 
### Capturing Updates

We will use the same Siddhi app, `CDCPolling.siddhi`, we deployed under 'Capturing inserts' section for capturing updates.

Let's perform an update operation on the MySQL table. Execute following MySQL query on the database:
```
update SweetProductionTable SET name = 'Almond cookie' where name = 'chocolate';
```
You will see following log on the SP console:
```
INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - CDCWithPollingMode : logStream : Event{timestamp=1563436388530, data=[Almond cookie, 100.0], isExpired=false}  
```

