# Integrating Data Stores in Streamming Integration

## Introduction

WSO2 Streaming Integrator allows you to incorporate data stores when performing various streaming integration activities. The methods in which this is done include:

- Change data capture
- Storing received data/processed data in data stores
- Performing CRUD operations in data stores

[Performing Real-time Change Data Capture with MySQL] tutorial covers how to perform change data capture in details. Therefore, in this tutorial, let's learn how WSO2 Streaming Integrator can incorporate data stores in streaming operations by performing CRUD operations.

## Scenario

Let's consider the example of a Sweet Factory that stores the following information in three different databases:

- Records of material purchases to be used in production
- Records of material dispatches for production
- The current stock of materials

In order to manage the material stock and to maintain the required records, the Factory Manager needs to do the following activities:

- Record each material purchase in the store for purchases.
- Record each dispatch of material for production in the store for dispatches.
- Update the store with current stock for each material after each purchase and dispatch to keep it up to date.

Recording purchases and dispatches involve inserting new records into data stores. To maintain the current stock records, the Factory Manager needs to retrieve information about both purchases and dispatches, calculate the impact of both on the current stock and then perform an insert/update operation to the store with the stock records. 

To understand how the WSO2 Streaming Integrator performs these operations, follow the steps below.

## Tutorial steps

!!! tip  "Before you begin:"
    You need to complete the following prerequisites before you begin:<br/><br/>
    - You need to have access to a MySQL instance.<br/><br/>
    - Install `rdbms-mysql` extension in WSO2 Streaming Integrator as follows:<br/><br/>
        1. Start WSO2 Streaming Integrator by navigating to the `<SI_HOME>/bin` directory and issuing the appropriate command based on your operating system.<br/><br/>
            - **For Linux**  : `./server.sh`
            - **For Windows**: `server.bat --run`<br/><br/>
        2. To install the `rdbms-mysql` extension, navigate to the to the `<SI_HOME>/bin` directory and issue the appropriate command based on your operating system:<br/><br/>
            - **For Linux**  : `./extension-installer.sh`
            - **For Windows**: `extension-installer.bat --run`<br/><br/>
        3. Restart the WSO2 Streaming Integrator server.<br/><br/>
    - Install the `rdbms-mysql` extension in WSO2 Streaming Integrator as follows.<br/><br/>
        1. Start WSO2 Streaming Integrator Tooling by navigating to the `<SI_TOOLING_HOME>/bin` directory and issuing the appropriate command based on your operating system.<br/><br/>
            - **For Linux**  : `./tooling.sh`
            - **For Windows**: `tooling.bat --run`<br/><br/>
        2. Access Streaming Integrator Tooling. Then click **Tools** -> **Extension Installer** to open the **Extension Installer** dialog box.<br/><br/>
        3. In the **Extension Installer** dialog box, click **Install** for the **RDBMS-MYSQL** extension. Then click **Install** in the message that appears to confirm whether you want to proceed.<br/><br/>
        4. Restart WSO2 Streaming Integrator Tooling.<br/><br/>
    - Start the MySQL server.<br/><br/>
    - Create three MySQL databases by issuing the following commands.<br/><br/>
        `CREATE SCHEMA purchases;`<br/><br/>`CREATE SCHEMA dispatches;`<br/><br/>`CREATE SCHEMA closingstock;`<br/><br/>
        
### Connecting a Siddhi application to data stores

In this section, let's learn the different ways in which you can connect a Siddhi application to a data store.

In Streaming Integrator Tooling, open a new file and start creating a new  Siddhi Application named `StockManagementApp`.

    ```
    @App:name("StockManagementApp")
    @App:description("Managing Raw Materials")
    ```
    
Now let's connect to the data stores (i.e., databases) you previously created to the Siddhi application. There are three methods in which this can be done. To learn them, let's connect each of the three databases in a different method.
      
#### Connecting to a store via a data source

To connect to the `closingstock` database via a data source, follow the steps below:

1. Define a data source in the `<SI_TOOLING_HOME>/conf/server/deployment.yaml` file as follows:

    ```
      - name: Stock_DB
        description: The datasource used for stock records
        jndiConfig:
          name: jdbc/closingstock
        definition:
          type: RDBMS
          configuration:
            jdbcUrl: 'jdbc:mysql://localhost:3306/closingstock?useSSL=false'
            username: root
            password: root
            driverClassName: com.mysql.jdbc.Driver
            minIdle: 5
            maxPoolSize: 50
            idleTimeout: 60000
            connectionTestQuery: SELECT 1
            validationTimeout: 30000
            isAutoCommit: false
    ```
   The above data source connects to the `closingstock` database you previously created via the `jdbcUrl` specified.
   
2. Now include the following table definition in the `StockManagementApp` Siddhi application that you started creating.

    ```
    @store(type = 'rdbms', datasource = "Stock_DB")
    @primaryKey('name' )
    define table StockTable (name string, amount double);
    ```
    
    In the above table definition:
    
    - The table has the two attributes `name` and `amount` to match the `closingstock` database you previously created.
    
    - The `@store`annotation specifies the database type as `rdbms` and connects the table to the `Stock_DB` data source you configured. Thus, you are connected to the `closingstock` database via the data source.
    
    - The `@primaryKey` annotation specifies `name` as the primary key of the table, requiring each record in the table to have a unique value for `name`.

#### Referring to an externally defined store

To connect to the `purchases` database via a reference, follow the steps below:

1. In the `<SI_TOOLING_HOME>/conf/server/deployment.yaml` file, add a subsection for refs, and then add a ref as shown below:

    ```
    siddhi:
      refs:
        -
          ref:
            name: 'purchases'
            type: 'rdbms'
            properties:
              jdbc.url: "jdbc:mysql://localhost:3306/purchases?useSSL=false"
              username: 'root'
              password: 'root'
              jdbc.driver.name: 'com.mysql.jdbc.Driver'
    ```
   The above reference connects to the `purchases` database that you previously created.
   
2. Now include the following table definition in the `StockManagementApp` Siddhi application.

    ```
    @store(type = 'rdbms', ref = "purchases")
    define table PurchasesTable (timestamp long, name string, amount double);
    ```

#### Configuring the data store inline

You can define the data store configuration for the `dispatches` database by adding a table definition in the `StockManagementApp` Siddhi application as follows:

```
@store(type = 'rdbms', jdbc.url = "jdbc:mysql://localhost:3306/dispatches?useSSL=false", username = "root", password = "root", jdbc.driver.name = "com.mysql.jdbc.Driver")
define table DispatchesTable (timestamp long, name string, amount double);
```
Here, you are configuring the data store configuration in the Siddhi application itself. The Siddhi application connects to the `dispatches` database via the specified JDBC URL.

### Writing Siddhi queries to perform CRUD operations

In this section, let's complete the `StockManagementApp` Siddhi application by adding the streams and queries to perform CRUD operations.

1. First, let's define the streams that receive information about material purchases and dispatches as follows.

    - For purchases:
    
        ```
        define stream MaterialPurchasesStream (timestamp long, name string, amount double);
        ```
    
    - For dispatches:
    
        ```
        define stream MaterialDispatchesStream (timestamp long, name string, amount double);
        ```
Now let's write Siddhi queries to perform different CRUD operations as follows:

 - **Inserts**
 
    To insert values into `purchases` and `dispatches` databases, let's write two queries as follows:
    
    - For purchases:
    
        ```
        @info(name = 'Save purchase records')
        from MaterialPurchasesStream 
        select * 
        insert into PurchasesTable;
        ```
    
    - For dispatches:
    
        ```
        @info(name = 'Save purchase records')
        from MaterialPurchasesStream 
        select * 
        insert into PurchasesTable;
        ```
    To try out these queries, simulate events for the streams via the Event Simulator as follows:
    
    1. Save the Siddhi application.
    
        The complete Siddhi application looks as follows:
        
        ```
        @App:name('StockManagementApp')
        
        @App:description('Managing Raw Materials')
        
        define stream MaterialDispatchesStream (timestamp long, name string, amount double);
        
        define stream MaterialPurchasesStream (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', jdbc.url = "jdbc:mysql://localhost:3306/dispatches?useSSL=false", username = "root", password = "root", jdbc.driver.name = "com.mysql.jdbc.Driver")
        define table DispatchesTable (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', ref = "purchases")
        define table PurchasesTable (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', datasource = "Stock_DB")
        @primaryKey("name")
        define table StockTable (name string, amount double);
        
        @info(name = 'Save material dispatch records')
        from MaterialDispatchesStream 
        select * 
        insert into DispatchesTable;
        
        @info(name = 'Save purchase records')
        from MaterialPurchasesStream 
        select * 
        insert into PurchasesTable;
        ```
    
        Then start it by clickinhg the play icon for it in the top panel.   
           
    2. Click the **Event Simulator** icon to open the event simulator.
    
        ![Event Simulation icon](../images/Testing-Siddhi-Applications/Event_Simulation_Icon.png)
        
        It opens the left panel for event simulation as follows.
        
        ![Event Simulation Panel](../images/Testing-Siddhi-Applications/Event_Simulation_Panel.png)
        
    3. To simulate purchase events, select `StockManagementApp` for the **Siddhi App Name** field, and `MaterialPurchasesStream` for the **Stream Name** field.
    
        Then enter values for the attribute fields and click **Send** to send them. For this scenario, let's send three events with the following values.
        
        - Event 1
        
            | **timestamp**   | **name** | **amount**|
            |-----------------|----------|-----------|
            | `1608025546000` | `flour`  | `150`     |
        
        - Event 2
        
            | **timestamp**   | **name** | **amount**|
            |-----------------|----------|-----------|
            | `1608027346000` | `sugar`  | `120`     |
        
        - Event 3
        
            | **timestamp**   | **name** | **amount** |
            |-----------------|----------|------------|
            | `1608028546000` | `honey`  | `100`      |
            
    4. To simulate three events for material dispatches, select `StockManagementApp` for the **Siddhi App Name** field, and `MaterialDispatchesStream` for the **Stream Name** field. 
    
        Then enter values for the attribute fields and click **Send** to send them. For this scenario, let's send three events with the following values.
       
        - Event 1
        
            | **timestamp**   | **name** | **amount**|
            |-----------------|----------|-----------|
            | `1608041746000` | `flour`  | `100`     |
        
        - Event 2
        
            | **timestamp**   | **name** | **amount**|
            |-----------------|----------|-----------|
            | `1608043746000` | `sugar`  | `70`     |
        
        - Event 3
        
            | **timestamp**   | **name** | **amount**|
            |-----------------|----------|-----------|
            | `1608045646000` | `honey`  | `50`      | 
            
    5. To check whether the above insertions were successful, issue the following MySQL commands in the terminal in which you are running the MySQL server.
    
        - For the `purchases` database:
        
            ```
            use purchases;
            ```
          
            ```
            select * from PurchasesTable;
            ```
          
          The following table is displayed.
          
          ![Saved Purchase Records](../images/integrating-stores/saved-purchase-records.png)
        
        - For the `dispatches` database:
        
            ```
            use dispatches;
            ```
          
            ```
            select * from DispatchesTable;
            ```
          
          The following table is displayed.
        
          ![Saved Dispatch Records](../images/integrating-stores/saved-dispatch-records.png)
         
 - **Retrievals**
 
    Assume that the Factory Manager needs to view all the purchase records for honey. This can be done by following the steps below:
    
    1. To receive the record retrieval requests as input events, define an input stream as follows:
    
        ```
        define stream PurchaseRecordRetrievalStream (name string);
        ```
       
       This stream only has the `name` attribute because only the name is needed to filter the search results
    
    2. To present the retrieved records, define an output stream as follows:
    
        ```text
        @sink(type = 'log', prefix = "Search Results",
        	@map(type = 'passThrough'))
        define stream SearchResultsStream (timestamp long, name string, amount double);
        ```
    
        The `SearchResultsStream` output stream has all the attributes of the `PurchasesTable` table to retrieve the complete record. Also, the `@sink` annotation connects this stream to a log sink so that the search results can be logged.
        
    3. Now lets add a join query to join the `PurchaseRecordRetrievalStream` and the `PurchasesTable` table.
    
        ```
        @info(name = 'Retrieve purchase records')
        from PurchaseRecordRetrievalStream as s 
        join PurchasesTable as p 
        	on s.name == p.name 
        select p.timestamp as timestamp, s.name as name, p.amount as amount 
        	group by p.name 
        insert into SearchResultsStream;
        ```
       
       Note the following about the above join query.
       
       - The stream is assigned the short name `s` and the table is assigned the short name `p`.
       
       - Based on the previous point, `on s.name == p.name ` condition specifies that a matching event is identified when the `PurchasesTable` has a record where the value for the `name` attribute is the same as that of the stream.
       
       - The `select` clause the query specifies that when such a matching event is identified, attribute values for the output event should be selected as follows:
       
            - The timestamp from the table
            - The name from the stream
            - The amount from the table
            
       - The `insert into` clause specifies that the output events derived as stated above should be inserted into the `SearchResultsStream`.
       
    4. Save the Siddhi application. The complete Siddhi application after the above changes looks as follows:
    
        ```
        @App:name('StockManagementApp')
        @App:description('Managing Raw Materials')
        
        define stream MaterialDispatchesStream (timestamp long, name string, amount double);
        
        @sink(type = 'log', prefix = "Search Results",
        	@map(type = 'passThrough'))
        define stream SearchResultsStream (timestamp long, name string, amount double);
        
        define stream MaterialPurchasesStream (timestamp long, name string, amount double);
        
        define stream PurchaseRecordRetrievalStream (name string);
        
        @store(type = 'rdbms', jdbc.url = "jdbc:mysql://localhost:3306/dispatches?useSSL=false", username = "root", password = "root", jdbc.driver.name = "com.mysql.jdbc.Driver")
        define table DispatchesTable (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', ref = "purchases")
        define table PurchasesTable (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', datasource = "Stock_DB")
        @primaryKey("name")
        define table StockTable (name string, amount double);
        
        @info(name = 'Save material dispatch records')
        from MaterialDispatchesStream 
        select * 
        insert into DispatchesTable;
        
        @info(name = 'Save purchase records')
        from MaterialPurchasesStream 
        select * 
        insert into PurchasesTable;
        
        @info(name = 'Retrieve purchase records')
        from PurchaseRecordRetrievalStream as s 
        join PurchasesTable as p 
        	on s.name == p.name 
        select p.timestamp as timestamp, s.name as name, p.amount as amount 
        	group by p.name 
        insert into SearchResultsStream;
        ```
    5. Open the Event Simulator and simulate an event for the `PurchaseRecordRetrievalStream` stream of the `StockManagementApp` Siddhi application with `honey` as the value for the **name** attribute.
    
        The following is logged in the terminal.
        
        ![Retirieved Event](../images/integrating-stores/retrieved-event.png)
        
 -  **Updates**
 
    To calculate the latest stock after a specific time duration, all the purchases during that time interval need to be added to the current stock, and all the dispatches during that same time interval need to be deducted from the current stock. Once this calculation is done, the result is loaded into the `PurchasesTable` table. This results in updating the existing record for each material or the result being inserted as a new record if the stock for the material is not already recorded in the database.
    
    1. First, let's add a join query to add the purchases to the current stock.
    
        ```
        @info(name = 'Add purchases to current stock ')
        from MaterialPurchasesStream as p 
        join StockTable as s 
            on p.name == s.name 
        select p.name as name, sum(p.amount) + s.amount as amount 
            group by p.name 
        insert into UpdateStockwithPurchasesStream;
        ```
        Note the following about the above join query.
        
        - The stream is assigned the short name `p` and the table is assigned the short name `s`.
        
        - Based on the above point, `p.name == s.name ` condition specifies that a matching event is identified when the `MaterialPurchasesStream`stream has an event where the value for the `name` attribute is the same as that of the `StockTable` table.
        
        - The `select` clause the query specifies that when such a matching event is identified, attribute values for the output event should be selected as follows:
        
            - The name from the `MaterialPurchasesStream`stream
            
            - The amount derived by calculating the total for the `amount` attribute from all the events in the `MaterialPurchasesStream`stream, and then adding that total to the amount in the table record.
            
        - The `insert into` clause specifies that the output events derived as stated above should be inserted into the `UpdateStockwithPurchasesStream` stream.
        
    2. Next, perform a join to subtract the total dispatches from the current  stock that is updated with the latest purchases.
    
        ```
        @info(name='Update stock with dispatches') 
        from UpdateStockwithPurchasesStream as u 
        join MaterialDispatchesStream as s 
            on u.name == s.name 
        select u.name as name, sum(u.amount) - sum(s.amount) as amount 
        insert into StockTable;
        ```   
    3. Save the Siddhi application. The complete Siddhiapplication with the latest queries you added looks as follows:
    
        ```
        @App:name('StockManagementApp')
        @App:description('Managing Raw Materials')
        
        define stream MaterialDispatchesStream (timestamp long, name string, amount double);
        
        @sink(type = 'log', prefix = "Search Results",
        	@map(type = 'passThrough'))
        define stream SearchResultsStream (timestamp long, name string, amount double);
        
        define stream MaterialPurchasesStream (timestamp long, name string, amount double);
        
        define stream PurchaseRecordRetrievalStream (name string);
        
        @store(type = 'rdbms', jdbc.url = "jdbc:mysql://localhost:3306/dispatches?useSSL=false", username = "root", password = "root", jdbc.driver.name = "com.mysql.jdbc.Driver")
        define table DispatchesTable (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', ref = "purchases")
        define table PurchasesTable (timestamp long, name string, amount double);
        
        @store(type = 'rdbms', datasource = "Stock_DB")
        @primaryKey("name")
        define table StockTable (name string, amount double);
        
        @info(name = 'Save material dispatch records')
        from MaterialDispatchesStream 
        select * 
        insert into DispatchesTable;
        
        @info(name = 'Save purchase records')
        from MaterialPurchasesStream 
        select * 
        insert into PurchasesTable;
        
        @info(name = 'Retrieve purchase records')
        from PurchaseRecordRetrievalStream as s 
        join PurchasesTable as p 
        	on s.name == p.name 
        select p.timestamp as timestamp, s.name as name, p.amount as amount 
        	group by p.name 
        insert into SearchResultsStream;
        
        @info(name = 'Add purchases to current stock ')
        from MaterialPurchasesStream as p 
        join StockTable as s 
            on p.name == s.name 
        select p.name as name, sum(p.amount) + s.amount as amount 
            group by p.name 
        insert into UpdateStockwithPurchasesStream;
        
        @info(name='Update stock with dispatches') 
        from UpdateStockwithPurchasesStream as u 
        join MaterialDispatchesStream as s 
            on u.name == s.name 
        select u.name as name, sum(u.amount) - sum(s.amount) as amount 
        insert into ClosingStockStream;
        ```
    4. Simulate two events as follows:
    
        
    
        
 
    