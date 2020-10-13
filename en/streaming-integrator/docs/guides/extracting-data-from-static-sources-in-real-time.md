# Extracting Data from Static Sources

WSO2 Streaming Integrator can extract data from static sources such as databases, files and cloud storages in real-tme. 

## Consuming data from databases

A database table is a stored collection of records of a specific schema. Each record can be equivalent to an event. WSO2 Streaming Integrator can integrate databases into the streaming flow by extracting records in database tables as streaming events. This can be done via change data capture or by polling a database.

![Extracting data from databases](../images/extracting-data-from-static-sources/extract-data-from-databases.png)

To understand how data is extracted from a database into a streaming flow, consider a scenario where an online booking site automatically save all online bookings of vacation packages in a database. The company wants to monitor the bookings in real-time. Therefore, this data stored in the database needs to be extracted in real time. You can either capture this data as change data or poll the database. The `cdc` Siddhi extensions can be used for both methods as explained in the following subsections.

### Capturing change data.

Capturing change data involves extracting any change that takes place in a selected database (i.e., any insert, update or a deletion) in real-time.

To capture change data via the WSO2 Streaming Integrator Tooling, define an input [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) with the appropriate schema to capture the information you require, and then connect a [source](https://siddhi.io/en/v5.1/docs/query-guide/#source) of the `cdc` type as shown in the example below.

```sql
@source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "insert", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
	@map(type = 'keyvalue'))
define stream OnlineBookingsStream (ref int, timestamp int, name string, package string, people int);
```
Here, note that the `mode` parameter of the `cdc` source is set to `listening`. This mode involves listening to the database for the specified database operation. In this example, the `operation` parameter is set to `insert`. Therefore, the source listens to new records inserted into the `OnlineBookingsTable` table and generates an input event in the `OnlineBookingsStream` stream for each insert.

If you want to capture updates to the records in the `OnlineBookingsTable` database table in real time, you can change the value for the `operation` parameter to `update` as shown below.

```sql
@source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "update", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
	@map(type = 'keyvalue'))
define stream OnlineBookingsStream (ref int, timestamp int, name string, package string, people int);
```
Similarly, if you want to capture deletions in the `OnlineBookingsTable` database table in real time, you can change the value for the `operation` parameter to `delete` as shown below.

```sql
@source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "delete", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
	@map(type = 'keyvalue'))
define stream OnlineBookingsStream (ref int, timestamp int, name string, package string, people int);
```

### Polling databases

This method involves periodically polling a database table to capture changes in the data. Similar to capturing change data, you can  define an input [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) with the appropriate schema to capture the information you require, and then connect a [source](https://siddhi.io/en/v5.1/docs/query-guide/#source) of the `cdc` type as shown in the example below. However, for polling, the value for the `mode` parameter must be `polling`

```sql
@source(type = 'cdc',
    url = 'jdbc:mysql://localhost:3306/tours?useSSL=false',
    mode = 'polling',
    jdbc.driver.name = 'com.mysql.jdbc.Driver',
    polling.column = 'timestamp',
    polling.interval = '10',
    username = 'wso2si',
    password = 'wso2',
    table.name = 'OnlineBookingsTable',
    @map(type = 'keyvalue' ))
define stream OnlineBookingsStream (ref int, timestamp long, name string, package string, people int);
```
The above source polls the `OnlineBookingsTable`table every 10 seconds and captures all inserts and updates to the database table that take place during that time interval. An input event is generated in the `OnlineBookingsStream` stream for each insert and update.

!!! tip
    - The `polling` mode only captures insert and update operations. Unlike in the `listening` mode, you do not need to specify the operation.
    

If you want to try out the use case described above, click the link below.

??? tip "Try out an example"
    Let's try out the example where you want to view the online bookings saved in a database in real time. To do this, follow the steps below:<br/><br/>
    1. Download and install MySQL.
    2. Enable binary logging in the MySQL server. For detailed instructions, see [Enabling the Binlog tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog).<br/><br/>
    3. Start the MySQL server, create the database and the database table you require as follows:<br/><br/>
        1. To create a new database, issue the following MySQL command.
            ```
            CREATE SCHEMA tours;
            ```<br/><br/>   
        2. Create a new user by executing the following SQL query.<br/><br/>
           ```
           GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
           ```<br/><br/>
        3. Switch to the `tours` database and create a new table, by executing the following queries:<br/><br/>
            `use tours;`<br/><br/>
            `CREATE TABLE production.new_table (
              ref INT NOT NULL AUTO_INCREMENT,
              timestamp LONGTEXT NULL,
              name VARCHAR(45) NULL,
              package VARCHAR(45) NULL,
              people INT NULL,
              PRIMARY KEY (ref));`<br/><br/>
        4. [Start the Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).<br/><br/>
        5. Download the `cdc-mysql`Siddhi extension for Streaming Integrator Tooling. For instructions, see [Installing Siddhi Extensions](../develop/installing-siddhi-extensions.md/#installing-an-extension).<br/><br/>
        6. In Streaming Integrator Tooling, open a new file. Copy and paste the following Siddhi application to it.<br/><br/>
            ```sql
            @App:name("VacationsApp")<br/>
            @App:description("Captures cdc events from MySQL table")<br/><br/>          
            @source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "insert", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
                @map(type = 'keyvalue'))<br/>
            define stream OnlineBookingsStream (ref int, timestamp long, name string, package string, people int);<br/><br/>           
            @sink(type = 'log')<br/>
            define stream LogStream (ref int, timestamp long, name string, package string, people int);<br/><br/>       
            @info(name = 'query')<br/>
            from OnlineBookingsStream<br/>
            select *<br/>
            insert into LogStream;
            ```<br/><br/>
            Then save the Siddhi application.<br/><br/>This Siddhi application uses a `cdc` source that extracts events in the change data capturing (i.e., listening) mode and logs the captured records in the console via a `log` sink
        7. Start the Siddhi Application  by clicking the play button.<br/>
            ![Play](../images/extracting-data-from-static-sources/play.png)<br/><br/>
        8. To insert a record into the `OnlineBookingsTable`, issue the following MySQL command:<br/><br/>
            `insert into OnlineBookingsTable(ref,timestamp,name,package,people) values('1',1602506738000,'jem','best of rome',2);`<br/><br/>
            The following is logged in the Streaming Integrator Tooling terminal.<br/><br/>            
            `INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - VacationsApp : LogStream : Event{timestamp=1563378804914, data=[1, 1602506738000, jem, best of rome, 2], isExpired=false}`


### Supporting Siddhi extensions

The following is a list of Siddhi extensions that support change data capturing to allow you to extract database records as input events in real time.

| **Extension**                    | **Name**         | **Description**                                 |
|----------------------------------|------------------|-------------------------------------------------|
| Change Data Capture - Mongo DB   | `cdc-mongodb`    | Captures change data from Mongo DB databases.   | 
| Change Data Capture - MS SQL     | `cdc-mssql`      | Captures change data from MS SQL databases.     |
| Change Data Capture - MySQL      | `cdc-mysql`      | Captures change data from MySQL databases.      | 
| Change Data Capture - Oracle     | `cdc-oracle`     | Captures change data from Oracle databases.     |
| Change Data Capture - PostgreSQL | `cdc-postgresql` | Captures change data from PostgreSQL databases. |


## Consuming data from files

## Consuming data from cloud storages