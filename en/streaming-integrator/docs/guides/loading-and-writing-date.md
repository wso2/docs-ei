# Loading and Writing Data

Loading and writing data involves publishing the data in a destination where it can be extracted again at any given time for further processing. WSO2 Streaming Integrator supports loading and writing data to databases, files, and cloud storages.

## Loading data to databases

WSO2 Streaming Integrator allows you to load information to databases by defining [Siddhi tables](https://siddhi.io/en/v5.1/docs/query-guide/#table) that are connected to database tables, and then writing queries to publish data into those tables so that it can be transferred to the connected database tables. This is useful when you need to save data in a static manner to be used for further processing when you perform ETL (Extract, Transform, Load) operations. This data could be unprocessed data recived from another source or the results of any processing carried out by WSO2 Streaming Integrator.

![Loading Data to Databases](../images/loading-and-writing-data/load-data-to_database.png)

To understand this, consider an example of an online store that needs to save all its sales records in a database table. To address this, you can write a Siddhi application as follows:

- **Define a table**

    In this example, let's define a table named `SalesRecords` as follows:
    
    ```
    @primaryKey('ref')
    @index('name')
    @store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/SalesRecordsDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")
    define table SalesRecords(ref string, name string, amount int);
    ```
  The above table definition captures sales records. The details captured in each sales record includes the transaction reference (`ref`), the name of the product (`name`), and sales amount (`amount`). The `ref` attribute is the primary key because there cannot be two or more records with the same transaction reference. The `name` attribute is an index attribute.
  
  A data store named `SalesRecordsDB` is connected to this table definition via the `@store` annotation. This is the data store to which you are loading data.

- **Add a query**

    You need to add a query to specify how events are selected to be inserted into the table you defined. You can add this query as shown below. 
    
    ```
    from SalesRecordsStream
    select *
    insert into SalesRecords;
    ``` 
    The above query gets all the input events from the `SalesRecordsStream` stream and loads them to the `SalesRecords` table.
    
### Try it out

To try out the example given above, follow the steps below:

1. Set up MySQL as follows:

    1. Download and install MySQL.    
    
    2. Start the MySQL server, create the database and the database table you require as follows:
    
        1. To create a new database, issue the following MySQL command.
        
            ```
            CREATE SCHEMA sales;
            ```
                            
        2. Switch to the `sales` database by issuing the following command.
            
           ```
           use sales;
           ```
           
        3. Create a new table, by issuing the following command.        
            
            ```
            CREATE TABLE sales.SalesRecords (
              ref INT NOT NULL AUTO_INCREMENT,
              name VARCHAR(45) NULL,
              amount INT NULL,
              PRIMARY KEY (ref));
            ```
        
2. [Start and access WSO2 Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).

3. Install the `RDBMS - MySQL` extension in Streaming Integrator tooling. For instructions to install an extension, see [Installing Siddhi Extensions](../develop/installing-siddhi-extensions.md).

4. Open a new file in WSO2 Streaming Integrator Tooling and add the following Siddhi content to it.

    ```
    @App:name('SalesApp')
    
    define stream SalesRecordsStream (name string, amount int);
    
    @store(type = 'rdbms', jdbc.url = "jdbc:mysql://localhost:3306/sales?useSSL=false", username = "root", password = "root", jdbc.driver.name = "com.mysql.jdbc.Driver")
    @primaryKey('ref' )
    @index('name' )
    define table SalesRecords (name string, amount int);
    
    from SalesRecordsStream 
    select * 
    insert into SalesRecords;
    ```
   
   Save the Siddhi application.   
    
5. To simulate an event to the `SalesApp` Siddhi application, click the **Event Simulator** icon in the side panel. 

    ![simulate event](../images/loading-and-writing-data/event-simulator-icon.png)

    Then simulate an event as follows:
    
    1. In the **Siddhi App Name** field, select **SalesApp**.
    
    2. In the **Stream Name** field, select **SalesRecordsStream**.
    
    3. In the **Attributes** section, enter values for the attributes as follows:
    
        ![Simulate Single Event](../images/loading-and-writing-data/simulate-single-event.png)
    
        | **Attribute** | **Value**        |
        |---------------|------------------|
        | **ref**       | `AA000000000001` |
        | **name**      | `fruit cake`     |
        | **amount**    | `100`            |
        
    4. Click **Start and Send**.

6. To check whether the `sales` mysql table is updated, issue the following command in the MySQL server.

    `select * from SalesRecords;`
    
    The table is displayed as follows, indicating that the event you generated is added as a record.
    
    ![Updated MySQL Table](../images/loading-and-writing-data/updated-mysql-table.png)
    
### Publishing data on demand via store queries

To understand how to publish data on demand, see [Processing Data - Aggregating Data](processing-data.md#aggregating-data)

### Supported databases

WSO2 Streaming supports the following database types via Siddhi extensions:

| **Database Type** | **Siddhi Extension**                                                                |
|-------------------|-------------------------------------------------------------------------------------|
| RDBMS             | [rdbms](https://siddhi-io.github.io/siddhi-store-rdbms/api/latest/#store)           |
| MongoDB           | [mongodb](https://siddhi-io.github.io/siddhi-store-mongodb/api/latest/#store)       |
| Redis             | [redis](https://siddhi-io.github.io/siddhi-store-redis/api/latest/#store)           |
| elasticsearch     | [elasticsearch](https://siddhi-io.github.io/siddhi-store-elasticsearch/api/latest/) |

## Writing data to files

## Storing data in Cloud storage