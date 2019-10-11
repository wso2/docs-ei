# Aggregating Data Incrementally

## Purpose

This example demonstrates how to get running statistics using Siddhi. The sample Siddhi application aggregates the data relating to the raw material purchases of a sweet production factory.

!!!info "Before you begin:"
    1. Install MySQL.<br/>
    2. Add the MySQL JDBC driver to your Streaming Integrator library as follows:<br/>
        1. Download the JDBC driver from the [MySQL site](https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz). <br/>
        2. Extract the MySQL JDBC Driver zip file you downloaded. Then use the `jarbundle` tool in the `<SI_TOOLING_HOME>/bin` directory to convert the jars in it into OSGi bundles. To do this, issue one of the following commands:<br/>
            - **For Windows**: `<SI_HOME>/bin/jartobundle.bat <PATH_OF_DOWNLOADED_JAR> <PATH_OF_CONVERTED_JAR>`<br/>
            - **For Linux**: `<SI_HOME>/bin/jartobundle.sh <PATH_OF_DOWNLOADED_JAR> <PATH_OF_CONVERTED_JAR`<br/>
        3. Copy the converted bundles to the `<SI_HOME>/lib` directory.<br/>
    3. Create a data store named `sweetFactoryDB` in MySQL with relevant access privileges.<br/>
    4. Replace the values for the `jdbc.url`, `username`, and `password` parameters in the sample.<br/>
        e.g., <br/>
        - `jdbc.url` - `jdbc:mysql://localhost:3306/sweetFactoryDB`<br/>
        - `username` - `root`<br/>
        - `password` - `root`<br/>
    5. Save the sample Siddhi application.<br/>

            ```sql
            @App:name("AggregateDataIncrementally")
            @App:description('Aggregates values every second until year and gets statistics')
            define stream RawMaterialStream (name string, amount double);
            @sink(type ='log')
            define stream RawMaterialStatStream (AGG_TIMESTAMP long, name string, avgAmount double);
            @store( type="rdbms",
                    jdbc.url="jdbc:mysql://localhost:3306/sweetFactoryDB",
                    username="root",
                    password="root",
                    jdbc.driver.name="com.mysql.jdbc.Driver")
            define aggregation stockAggregation
            from RawMaterialStream
            select name, avg(amount) as avgAmount, sum(amount) as totalAmount
            group by name
            aggregate every sec...year;
            define stream TriggerStream (triggerId string);
            @info(name = 'query1')
            from TriggerStream as f join stockAggregation as s
            within "2016-06-06 12:00:00 +05:30", "2020-06-06 12:00:00 +05:30"
            per 'hours'
            select AGG_TIMESTAMP, s.name, avgAmount
            insert into RawMaterialStatStream;
            ```


## Executing the Sample

1. Start the Siddhi application by clicking **Run** => **Run**.

2. If the Siddhi application starts successfully, the following message appears in the console.

   `AggregateDataIncrementally.siddhi - Started Successfully!.`

## Testing the Sample

1. To open the Event Simulator, click the **Event Simulator** icon.

    ![Event Simulator Icon](../../images/Testing-Siddhi-Applications/Event_Simulation_Icon.png)

2. To simulate events for the `RawMaterialStream` stream of the `AggregateDataIncrementally`  enter information in the **Single Simulation** tab as follows.

    | **Field**                   | **Value**                              |
    |-----------------------------|----------------------------------------|
    | **Siddhi App Name**         | `AggregateDataIncrementally`           |
    | **StreamName**              | `RawMaterialStream`                    |

    ![Select Siddhi Application and Stream](../../images/aggregate-data-incrementally-sample/aggregate-data-incrementally-event-simulation.png)

    As a result, the attributes of the `RawMaterialStream` stream appear as marked in the image above.


2. Send four events by entering values as shown below. Click **Send** after each event.

    - Event 1

        - **name**: `chocolate cake`

        - **amount**: 100

    - Event 2

        - **name**: `chocolate cake`

        - **amount**: 200

    - Event 3

        - **name**: `chocolate ice cream`

        - **amount**: `50

    - Event 4

        - **name**: `chocolate ice cream`

        - **amount**: `150`

3. In the **StreamName** field, select **TriggerStream***. In the **triggerId** field, enter `1` as the trigger ID, and then click **Send**.



## Viewing the Results:

The input and the corresponding output is displayed in the console as follows.

!!!info
    The timestamp displayed is different because it is derived based on the time at which you send the events.

```
    INFO {io.siddhi.core.stream.output.sink.LogSink} - AggregateDataIncrementally : RawMaterialStatStream : [Event{timestamp=1513612116450, data=[1537862400000, chocolate ice cream, 100.0], isExpired=false}, Event{timestamp=1513612116450, data=[chocolate cake, 150.0], isExpired=false}]
    [INFO {io.siddhi.core.stream.output.sink.LogSink} - AggregateDataIncrementally : RawMaterialStatStream : [Event{timestamp=1513612116450, data=[1537862400000, chocolate ice cream, 100.0], isExpired=false}, Event{timestamp=1513612116450, data=[chocolate cake, 150.0], isExpired=false}]
```
    
