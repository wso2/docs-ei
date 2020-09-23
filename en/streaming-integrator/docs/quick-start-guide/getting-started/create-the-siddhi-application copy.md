# Step 2: Create the Siddhi Application

Let's create your first Siddhi application.

For this purpose, you can consider an example where production information is published in a database table. This information needs to be captured as and when it is published in the database, and published in a file after changing the case of the product name.

The following image depicts the procedure to be followed by the Siddhi application you create.

![Siddhi Application Flow](../../images/quick-start-guide-101/)

1. Extract the Streaming Integrator Tooling pack to a preferred location. Hereafter, the extracted location is referred to as `<SI_TOOLING_HOME>`.

2. Navigate to the `<SI_TOOLING_HOME>/bin` directory and issue the appropriate command depending on your operating system to start the Streaming Integration tooling.

    -   For Windows: `tooling.bat`

    -   For Linux/MacOS: `./tooling.sh`
    
3. Access the Streaming Integration Tooling via the `http://<HOST_NAME>:<TOOLING_PORT>/editor` URL.

    !!!info
        The default URL is `http://<localhost:9390/editor`.
        
   The Streaming Integration Tooling opens as shown below.

   ![Welcome Page](../../images/Creating-Siddhi-Applications/Welcome-Page.png)
        
4. Open a new Siddhi file by clicking **New**.

    The new file opens as follows.
    
    ![New Siddhi File](../../images/Creating-Siddhi-Applications/New_Siddhi_File.png)
    
5. Specify a name for the new Siddhi application via the `@App:name` annotation.

    ```
    @App:name("SweetFactoryApp")
    ```
   
6. First, let's define the stream that receives the input data. 

    `define stream InsertSweetProductionStream (name string,amount double);`
    
7. To allow the `InsertSweetProductionStream` stream to capture inserts from the `productions` database, connect a source of the `cdc` type to it as shown below.

    ```
    @source(type='cdc',url = "jdbc:mysql://localhost:3306/production",username = "wso2si",password = "wso2",table.name = "SweetProductionTable",operation = "insert",
    	@map(type='keyvalue'))
    define stream InsertSweetProductionStream (name string,amount double);
    ```
8. To publish the captured data into a file, define an output stream as follows.

    `define stream ProductionUpdatesStream (name string,amount double);`

9. To publish the output events to a file, connect a file source to the stream as shown below:

    ```
    @sink(type='file',file.uri = "/Users/foo/productioninserts.csv",
   	@map(type='text'))
    define stream ProductionUpdatesStream (name string,amount double);
    ```
   
   Here, you are publishing the output in the text format to a file named `productioninserts.csv` in the `/Users/foo` directory.
   
10. To convert the case of the product name from lower case to title case, and then publish the converted events to the file, write a query as follows.

    ```
    @info(name='query1')
    from InsertSweetProductionStream 
    select str:upper(name) as name, amount 
    group by name 
    insert  into ProductionUpdatesStream;
    ```
    
    This query gets the information inserted into the `productions` database table from the `InsertSweetProductionStream`stream. The `str:upper()` function included in the `select` clause converts the product name from lower case to title case. Once this conversion is done, the converted events are directed to the `ProductionUpdatesStream` stream. These events are written into the `/Users/foo/productioninserts.csv` file because it is configured via the `file` source you previously annotated to the `ProductionUpdatesStream` stream.
    
11. Save the Siddhi application.


The completed Siddhi application looks as follows:

```
@App:name('SweetFactoryApp')
@App:statistics(reporter = 'prometheus')


@source(type='cdc',url = "jdbc:mysql://localhost:3306/production",username = "wso2si",password = "wso2",table.name = "SweetProductionTable",operation = "insert",
	@map(type='keyvalue'))
define stream InsertSweetProductionStream (name string,amount double);

@sink(type='file',file.uri = "/Users/foo/productioninserts.csv",
	@map(type='text'))
define stream ProductionUpdatesStream (name string,amount double);

@info(name='query1')
from InsertSweetProductionStream 
select str:upper(name) as name, amount 
group by name 
insert  into ProductionUpdatesStream;
```

## Extending your Siddhi application

A Siddhi application can be easily extended to consume messages from more sources, to carry out more processing activities for data or to publish data to more destinations. For this example, consider a scenario where you also need to filter out the production data of eclairs and publish it to a Kafka topic so that applications that cannot read streaming data can have access to it. This involves extending the `SweetFactoryApp` Siddhi application to include Kafka in the streaming flow. To do this, follow the steps below:

1. Open the `<SI_HOME>/wso2/server/deployment/siddhi-files/SweetFactoryApp` Siddhi application in a text editor of your choice.

    !!! tip
        Alternatively, you can open this file in Streaming Integrator Tooling and deploy it again after completing the changes and saving it.

2. Define another stream to which you can direct the filtered events you need to publish in a the Kafka topic.

    `define stream FilterStream (name string,amount double);`
    
3. To publish the events filtered into the `PublishFilteredDataStream` stream, connect a source of the `kafa` type to it as shown below.

    ```
    @sink(type = 'kafka', bootstrap.servers = "localhost:9092", topic = "eclair_production", is.binary.message = "false", partition.no = "0",
             @map(type = 'json'))
    define stream PublishFilteredDataStream (name string,amount double);
    ```
   
   The above sink annotation publishes all the events received into the `PublishFilteredDataStream` stream into a topic named `eclair_production` in `json` format.
   
4. Let's create another stream to read from the `/Users/foo/productioninserts.csv` file to which you have been publishing data.

    !!! tip
        Alternatively, you can write the query to read from one of the existing streams. However, in this example, let's create a new stream to understand how WSO2 Streaming Integrator reads data from files.
        
    ```
    @source(type='file', mode='LINE',
       file.uri='file:/Users/foo/productions.csv',
       tailing='true',
       @map(type='csv'))
    define stream FilterStream (name string,amount double);
    ```
   
    Here, you are configuring the file to be read line by line in the tailing mode. Therefore, any new row added to the file is captured as an event in the `FilterStream` stream as and when it is added.


5. Now let's add the query to filter the required information and publish it.

    ```
    from FilterStream [name=='Eclairs']
    select * 
    group by name 
    insert  into PublishFilteredDataStream;
    ```
   
    In the `from` clause, `[name=='Eclairs']` filters all production runs where the name of the sweet produced is `Eclairs`. Then all the filtered events are inserted into the `PublishFilteredDataStream` stream so that they can be published in the `eclair_production` Kafka topic.
    
6. Save your changes.






