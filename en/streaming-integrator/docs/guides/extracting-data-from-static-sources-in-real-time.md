# Extracting Data from Static Sources in Real Time

WSO2 Streaming Integrator can extract data from static sources such as databases, files and cloud storages in real-tme. 

## Consuming data from databases

A database table is a stored collection of records of a specific schema. Each record can be equivalent to an event. WSO2 Streaming Integrator can integrate databases into the streaming flow by extracting records in database tables as streaming events. This can be done via change data capture or by polling a database.

![Extracting data from databases](../images/extracting-data-from-static-sources/extract-data-from-databases.png)

To understand how data is extracted from a database into a streaming flow, consider a scenario where an online booking site automatically save all online bookings of vacation packages in a database. The company wants to monitor the bookings in real-time. Therefore, this data stored in the database needs to be extracted in real time. You can either capture this data as change data or poll the database. The `cdc` Siddhi extensions can be used for both methods as explained in the following subsections.

### Change data capture

Change data capture involves extracting any change that takes place in a selected database (i.e., any insert, update or a deletion) in real-time.

To capture change data via the WSO2 Streaming Integrator Tooling, define an input [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) with the appropriate schema to capture the information you require, and then connect a [source](https://siddhi.io/en/v5.1/docs/query-guide/#source) of the `cdc` type as shown in the example below.

```sql
@source(type = 'cdc', 
    url = "jdbc:mysql://localhost:3306/tours?useSSL=false", 
    username = "wso2si", 
    password = "wso2", 
    table.name = "OnlineBookingsTable", 
    operation = "insert", 
    mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
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

This method involves periodically polling a database table to capture changes in the data. Similar to change data capture, you can  define an input [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) with the appropriate schema to capture the information you require, and then connect a [source](https://siddhi.io/en/v5.1/docs/query-guide/#source) of the `cdc` type as shown in the example below. However, for polling, the value for the `mode` parameter must be `polling`

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
    

### Try out an example

Let's try out the example where you want to view the online bookings saved in a database in real time. To do this, follow the steps below:

1. Download and install MySQL.

2. Enable binary logging in the MySQL server. For detailed instructions, see [Enabling the Binlog tutorial by debezium](https://debezium.io/docs/connectors/mysql/#enabling-the-binlog).

3. Start the MySQL server, create the database and the database table you require as follows:

    1. To create a new database, issue the following MySQL command.
    
        ```
        CREATE SCHEMA tours;
        ```
                        
    2. Create a new user by executing the following SQL query.
        
       ```
       GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'wso2si' IDENTIFIED BY 'wso2';
       ```
       
    3. Switch to the `tours` database and create a new table, by executing the following queries.
    
        `use tours;`
        
        `CREATE TABLE production.new_table (
          ref INT NOT NULL AUTO_INCREMENT,
          timestamp LONGTEXT NULL,
          name VARCHAR(45) NULL,
          package VARCHAR(45) NULL,
          people INT NULL,
          PRIMARY KEY (ref));`
          
    4. [Start the Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).
    
    5. Download the `cdc-mysql`Siddhi extension for Streaming Integrator Tooling. For instructions, see [Installing Siddhi Extensions](../develop/installing-siddhi-extensions.md/#installing-an-extension).
    
    6. In Streaming Integrator Tooling, open a new file. Copy and paste the following Siddhi application to it.
    
        ```siddhi
        @App:name("VacationsApp")
        @App:description("Captures cdc events from MySQL table")
               
        @source(type = 'cdc', url = "jdbc:mysql://localhost:3306/tours?useSSL=false", username = "wso2si", password = "wso2", table.name = "OnlineBookingsTable", operation = "insert", mode = "listening", jdbc.driver.name = "com.mysql.jdbc.Driver",
            @map(type = 'keyvalue'))
        define stream OnlineBookingsStream (ref int, timestamp long, name string, package string, people int);
                  
        @sink(type = 'log')
        define stream LogStream (ref int, timestamp long, name string, package string, people int);
              
        @info(name = 'query')
        from OnlineBookingsStream
        select *
        insert into LogStream;
        ```
       
        Then save the Siddhi application.<br/><br/>This Siddhi application uses a `cdc` source that extracts events in the change data capturing (i.e., listening) mode and logs the captured records in the console via a `log` sink.
        
    7. Start the Siddhi Application  by clicking the play button.
    
        ![Play](../images/extracting-data-from-static-sources/play.png)
        
    8. To insert a record into the `OnlineBookingsTable`, issue the following MySQL command:
    
        `insert into OnlineBookingsTable(ref,timestamp,name,package,people) values('1',1602506738000,'jem','best of rome',2);`
        
        The following is logged in the Streaming Integrator Tooling terminal.
                   
        `INFO {org.wso2.siddhi.core.stream.output.sink.LogSink} - VacationsApp : LogStream : Event{timestamp=1563378804914, data=[1, 1602506738000, jem, best of rome, 2], isExpired=false}`


### Supported databases

[siddhi-io-cdc source](https://siddhi-io.github.io/siddhi-io-cdc/api/latest/) via which the WSO2 Steaming Integrator extracts database records supports the following database types.

The following is a list of Siddhi extensions that support change data capturing to allow you to extract database records as input events in real time.

| **Database Type** | **Extension Name** | **Description**                                 |
|-------------------|--------------------|-------------------------------------------------|
| Mongo DB          | `cdc-mongodb`      | Captures change data from Mongo DB databases.   | 
| MS SQL            | `cdc-mssql`        | Captures change data from MS SQL databases.     |
| MySQL             | `cdc-mysql`        | Captures change data from MySQL databases.      | 
| Oracle            | `cdc-oracle`       | Captures change data from Oracle databases.     |
| PostgreSQL        | `cdc-postgresql`   | Captures change data from PostgreSQL databases. |


## File Processing

File Processing involves two types of operations related to files:

- **Extracting data from files**: This involves extracting the content of file as input data for further processing.

- **Managed file transfer**: This involves using statistics of operations carried out for files (e.g., creating, editing, moving, deleting, etc.) as input data for further processing.

e.g., In a sweet factory where the production bots publishes the production statistics in a file after each production run. Extracting the production statistics from the files for further processing can be considered reading files and extracting data. Checking whether a file is generated to indicate a completed production run, and checking whether a file is moved to a different location after its content is processed can be considered as managed file transfer.

To understand how you can perform these file processing activities via the WSO2 Streaming Integrator, see the subtopics below.


### Extracting data from files

WSO2 Streaming Integrator extracts data from files via the [File Source](https://siddhi-io.github.io/siddhi-io-file/api/latest/#file-source). 

![Extracting data from databases](../images/extracting-data-from-static-sources/file-content-processing.png)

You can extract data from a single file or from multiple files in a directory. This is specified via the `file.uri` and `dir.uri` parameters as shown below:

- **Reading a single file**

    In the following example, the `file.uri` parameter specifies the `productioninserts.csv` file in the `/Users/foo` directory as the file from which the source should extract data. 
    
    ```
    @source(type = 'file',file.uri = "file:/Users/foo/productioninserts.csv",
        @map(type = 'csv'))
    define stream ProductionStream (name string, amount double);
    ```
    
- **Reading multiple files within a directory**

    In the following example, the `dir.uri` parameter specifies the `/Users/foo/production` as the directory with the files from which the source extracts information. According to the following configuration, all the files in the directory are read.
    
    ```
    @source(type = 'file',
        dir.uri = "file:/Users/foo/production",
        @map(type = 'csv'))
    define stream ProductionStream (name string, amount double);
    ```
  
    If you want the source to read only specific files within the directory, you need to specify the required files via the `file.name.list` parameter as shown below.
    
    ```
    @source(type = 'file', 
        dir.uri = "file:/Users/foo/production", 
        file.name.list = "productioninserts.csv,AssistantFile.csv,ManagerFile.csv",
    	@map(type = 'csv'))
    define stream ProductionStream (name string, amount double);
    ```
 
 The `file` source allows you to specify the mode in which you want the file to be read. The mode is specified via the `mode` parameter as shown in the extract below.

  
## Managing file transfers

![Managed File Transfers](../images/extracting-data-from-static-sources/file-events-processing.png)

## Consuming data from cloud storages