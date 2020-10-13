# Extracting Data from Static Sources

WSO2 Streaming Integrator can extract data from static sources such as databases, files and cloud storages in real-tme. 

## Consuming data from databases

A database table is a stored collection of records of a specific schema. Each record can be equivalent to an event. WSO2 Streaming Integrator can integrate databases into the streaming flow by extracting records in database tables as streaming events.

To understand how data is extracted from a database into a streaming flow, consider a scenario where an online booking site automatically save all online bookings of vacation packages in a database. The company wants to monitor the bookings in real-time. Therefore, this data stored in the database needs to be extracted in real time.

!!! tip "Before you begin:"
    To try out the examples in this guide, you can set up a MySQL database and a table as follows:<br/><br/>
    - You need to have access to a MySQL instance.<br/><br/>
    - Enable binary logging in the MySQL server. For detailed instructions, see [Enabling the Binlog tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog).<br/><br/>
    - Once you install MySQL and start the MySQL server, create the database and the database table you require as follows:<br/><br/>
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
        5. Download the `cdc-mysql`Siddhi extension for Streaming Integrator Tooling. For instructions, see [Installing Siddhi Extensions](../develop/installing-siddhi-extensions.md/#installing-an-extension).
                      

You can use the [siddhi-io-cdc](https://siddhi-io.github.io/siddhi-io-cdc/api/latest/) extension for this purpose. This extension allows you to extract data from the database in two modes:

- Polling Mode
    Here, the data source is periodically polled in order to capture changes. The polling period can be specified. This mode allows only `insert` and `update` operations to be captured.

- Listening Mode
    Here, the source continuously listens to the change log of the database and notifies as soon as a change takes place. Unlike the polling mode, change data is captured immediately instead of periodically. This mode allows `insert`, `update`, and `delete` operations to be captured.
    
Let's understand how to capture change data in both the modes.

### Polling mode

To capture change data in a database in the polling manner, define an input [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) with the appropriate schema to capture the information you require, and then connect a [source](https://siddhi.io/en/v5.1/docs/query-guide/#source) of the `cdc` type as shown below.

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

Here, the `cdc` extension reads data from the MySQL table named `OnlineBookingsTable`. The mode is `polling`. The polling interval is set to `10`. Therefore, this source polls the `OnlineBookingsTable` every 10 seconds and captures any changes (i.e., any insert operations or update operations) that took place during that time interval as events.

To try out the above source, include it in a Siddhi application as follows and save it in Streaming Integrator Tooling.

```sql
@App:name("VacationsApp")
@App:description("Captures cdc events from MySQL table")

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

@sink(type = 'log')
define stream LogStream (ref int, timestamp long, name string, package string, people int);

@info(name = 'query')
from OnlineBookingsStream
select *
insert into LogStream;
```
!!! tip
    The above Siddhi application logs thge captured change data in the console. For more information, see [Publishing Data](publishing-data-to-event-stream-consumers.md).

Then perform an insert operation and an update operation in the `OnlineBookingsTable` MySQL table as follows.

`insert into OnlineBookingsTable(ref,timestamp,name,package,people) values('1',1602506738000,'jem','best of rome',2);`

`update OnlineBookingsTable SET package = 'dolce vita' where package = 'best of rome';`

The following is logged in the Streaming Integrator Tooling terminal.

`INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - VacationsApp : LogStream : Event{timestamp=1563378804914, data=[1, 1602506738000, jem, best of rome, 2], isExpired=false}`

`INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - VacationsApp : LogStream : Event{timestamp=1563378804914, data=[1, 1602506738000, jem, dolce vita, 2], isExpired=false}`

### Listening mode

To capture changes in the listening mode, let's do an update to the source you previously created as follows:

```sql
@source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "insert", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
	@map(type = 'keyvalue'))
define stream OnlineBookingsStream (ref int, timestamp int, name string, package string, people int);
```
Here, you have changed the mode to `listening`. When you use the `listening` mode, you are required to specify the operation. In the above example, the operation is `insert`. Therefore, let's insert an event into the `OnlineBookingsTable` MySQL table as follows.

`insert into OnlineBookingsTable(ref,timestamp,name,package,people) values('2',1602506739000,'john doe','garden tour',1);`

This generates the following log in the console.

``INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - VacationsApp : LogStream : Event{timestamp=1563378804914, data=[1, 1602506739000, john doe, garden tour, 1], isExpired=false}``

**To capture updates**:

Specify `update` as the value for the `operation` parameter of the CDC source as shown below:

```sql
@source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "update", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
	@map(type = 'keyvalue'))
define stream OnlineBookingsStream (ref int, timestamp int, name string, package string, people int);
```

**To capture deletes**:

Specify `delete` as the value for the `operation` parameter of the CDC source as shown below:

```sql
@source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "delete", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
	@map(type = 'keyvalue'))
define stream OnlineBookingsStream (ref int, timestamp int, name string, package string, people int);
```

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