# Processing Data

Processing data involves making changes to data to generate a required output. WSO2 Streaming Integrator allows you to carry out a wide range of operations to process streaming data. These operations are supported via [Siddhi Extensions](https://siddhi.io/en/v5.1/docs/extensions/). These operations can be broadly categorized into five categories as follows:

- Cleansing
- Transforming
- Enriching
- Aggregating
- Correlating

## Cleansing data

When you receive input data via the Streaming Integrator, it may consist of data that is not required to generate the required output, null values for certain attributes, etc. Cleansing data refers to refining the input data received by assigning values where there are missing values (if there are applicable values), filtering out the data that is not required, etc.

### Filtering data based on conditions

You can filter data based on the exact match of attribute, based on a regex pattern or based on multiple criteria

To understand this, consider a scenario where you receive the temperature of multiple rooms in a streaming manner. 

- **Filtering based on exact match of attribute:**

    If you want the temperature readings only for a specific room, you can add a query with a filter as follows.
    
    ```
    from TempStream [roomNo=='2233']
    select *
    insert into RoomAnalysisStream;
    ```
    With the `roomNo=='2233'` filter, you are filtering the temperature readings for the room number `2233`. These readings are then inserted into a separate stream named `RoomAnalysisStream`.
    
- **Filtering based on regex pattern**

    In the example of processing temperature readings, assume that you need to filter the temperature readings for a specific range of devices located in the Southern wing and used for purpose B. Also assume that this can be derived from the device ID because the first three characters of the device ID represent the wing, and the eighth character represents the purpose. e.g., in device ID `SOU5438B765`, the first three characters `SOU` represent the Southern wing, and the eighth character `B` represents purpose B.
    
    You can achieve this by adding a filter with a regex pattern as follows:
    
    ```
    @info(name = 'FilteredRoomRange')
    from TempStream
    select regex.find(SOU*B*) as deviceID, roomNo, temp
    insert into FilteredResultsStream;
    ```
   The `regex.find(SOU*B*)` function filters room IDs that start with the three characters `SOU` and has the character `B` with one or more characters before it as well as after it. 

- **Filtering based on multiple criteria**

    In the example of processing temperature readings, assume that you need to filter the readings for a range of rooms (e.g., rooms 100-210) where the temperature is greater than 40. For this, you can add a filter as follows.
    
     ```
     from TempStream [(roomNo >= 100 and roomNo < 210) and temp > 40]
     select *
     insert into RoomAnalysisStream;
     ```   

#### Try it out

To try out the query used in the above example, let's include it in a Siddhi Application and run it.

1. [Start and Access Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).

2. Open a new file. Then add and save the following Siddhi application.

    ```
    @App:name('TemperatureApp')
    
    define stream TempStream (deviceID string, roomNo int, temp double);
    
    @sink(type = 'log', prefix = "FilteredResult",
        @map(type = 'passThrough'))
    define stream RoomAnalysisStream (deviceID string, roomNo int, temp double);
    
    @info(name = 'Filtering2233')
    from TempStream[roomNo == '2233'] 
    select * 
    insert into RoomAnalysisStream;
    ```
3. Open the event simulator and simulate three events for the `TempStream` input stream of the `TemperatureApp` Siddhi application with the values for the attributes as given below. For instructions to simulate single events, see [Testing Siddhi Applications - Simulating a single event](testing-a-Siddhi-Application/#simulating-a-single-event).

    | **Event** | **deviceID**  | **roomNo** | **temp** |
    |-----------|---------------|------------|----------|
    | 1         | `SOU5438B765` | `2233`     | `30`     |
    | 2         | `WES1827A876` | `1121`     | `27`     |
    | 3         | `NOR1633B231` | `0899`     | `28`     |
    
    Only the first event is logged in the terminal as follows. This is because only the first event has `2233` as the value for the `roomNo` attribute.
    
    ```text
    INFO {io.siddhi.core.stream.output.sink.LogSink} - FilteredResult : Event{timestamp=1604494352744, data=[SOU5438B765, 2233, 30.0], isExpired=false} 
    ``` 
4. Now remove the `Filtering2233` query and replace it with the following query that filters based on multiple criteria.

    ```
    from TempStream [(roomNo >= 100 and roomNo < 210) and temp > 40]
    select *
    insert into RoomAnalysisStream;
    ``` 
   The complete Siddhi application is as follows:
   
   ```
    @App:name('TemperatureApp')
    
    define stream TempStream (deviceID string, roomNo int, temp double);
    
    @sink(type = 'log', prefix = "FilteredResult",
        @map(type = 'passThrough'))
    define stream RoomAnalysisStream (deviceID string, roomNo int, temp double);
    
    from TempStream [(roomNo >= 100 and roomNo < 210) and temp > 40]
    select *
    insert into RoomAnalysisStream;
   ```
   
5. Open the event simulator and simulate three events for the `TempStream` input stream of the `TemperatureApp` Siddhi application with the values for the attributes as given below. For instructions to simulate single events, see [Testing Siddhi Applications - Simulating a single event](testing-a-Siddhi-Application/#simulating-a-single-event).

    | **Event** | **deviceID**  | **roomNo** | **temp** |
    |-----------|---------------|------------|----------|
    | 1         | `SOU5438B765` | `183`      | `30`     |
    | 2         | `WES1827A876` | `136`      | `42`     |
    | 3         | `NOR1633B231` | `899`      | `41`     |
    
    Only the second event is logged because only that event matched both the criteria. The event is logged as follows:
    
    ```text
    INFO {io.siddhi.core.stream.output.sink.LogSink} - FilteredResult : Event{timestamp=1604557083556, data=[WES1827A876, 136, 42.0], isExpired=false}
    ```

### Modifying, removing, and replacing attributes

The input data may include attributes that are not required in order to generate the required output, attributes with values that need to be updated or replaced before further processing.

Assume that in the previous example, you do not need the device ID for further processing, and you need to present the room numbers as string values instead of integer values. To do this, follow the procedure below:

```
@info(name = 'CleaningData')
from FilteredResultsStream
select cast(roomNo string) as roomNo, temp
insert into CleansedDataStream;
```
Here, the `cast()` function presents the value for the `roomNo` attribute as a string value although it is received as an integer value. The `select` clause excludes the `deviceID` attribute.

#### Try it out

To try out the above example, follow the steps below:

1. [Start and Access Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).

2. Open a new file. Then add and save the following Siddhi application.

    ```
    @App:name('TemperatureApp')
    
    define stream TempStream (deviceID string, roomNo int, temp double);
    
    @sink(type = 'log', prefix = "CleanedData",
        @map(type = 'passThrough'))
    define stream CleansedDataStream (roomNo string, temp double);
    
    @info(name = 'CleaningData')
    from TempStream
    select cast(roomNo, "string") as roomNo, temp
    insert into CleansedDataStream;
    ```
   In this Siddhi application, the `Temp Stream` has an attribute named `deviceID`, but it is not selected to be included in the output events. The `roomNo`attribute is cast as an string value via `cast(roomNo, "string")`. This means although the value for this attribute is received as an integer, it is presented as a string value in the output.
   
3. Open the event simulator and simulate an event for the `TempStream` input stream of the `TemperatureApp` Siddhi application with the values for the attributes as given below. For instructions to simulate single events, see [Testing Siddhi Applications - Simulating a single event](testing-a-Siddhi-Application/#simulating-a-single-event).

    | **deviceID**  | **roomNo** | **temp** |
    |---------------|------------|----------|
    | `SOU5438B765` | `183`      | `30`     |
    
    The output is logged as follows:
    
    ```text
    INFO {io.siddhi.core.stream.output.sink.LogSink} - CleanedData : Event{timestamp=1604578130314, data=[183, 30.0], isExpired=false}
    ```


### Handling attributes with null values

In the example of processing temperature readings, assume that some events arrive with null values for the `deviceID` attribute, and you want to assign the value `unknown` in such scenarios. This can be achieved by writing a query as follows:

```
@info(name = 'AddingMissingValues')
from FilteredResultsStream
select ifThenElse(deviceID is null, "UNKNOWN", deviceID) as deviceID, roomNo, temp
insert into CleansedDataStream
```
#### Try it out.

To try out the above example, follow the steps below:

1. [Start and Access Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).

2. Open a new file. Then add and save the following Siddhi application.

    ```
    @App:name("TemperatureApp")
    @App:description("Description of the plan")
    
    define stream TempStream (deviceID string, roomNo string, temp double);
    
    @sink(type = 'log', prefix = "Cleansed Data",
    	@map(type = 'passThrough'))
    define stream CleansedDataStream (deviceID string, roomNo string, temp double);
    
    @info(name = 'AddingMissingValues')
    from TempStream
    select ifThenElse(deviceID is null, "UNKNOWN", deviceID) as deviceID, roomNo, temp
    insert into CleansedDataStream;
    ```
   In this Siddhi application, the `Temp Stream` has an attribute named `deviceID`, but it is not selected to be included in the output events. The `roomNo`attribute is cast as an string value via `cast(roomNo, "string")`. This means although the value for this attribute is received as an integer, it is presented as a string value in the output.
   
3. Open the event simulator and simulate an event for the `TempStream` input stream of the `TemperatureApp` Siddhi application with the values for the attributes as given below. For instructions to simulate single events, see [Testing Siddhi Applications - Simulating a single event](testing-a-Siddhi-Application/#simulating-a-single-event).

    | **deviceID**                                        | **roomNo** | **temp** |
    |-----------------------------------------------------|------------|----------|
    | Select the **Is Null** check box for this sttribute | `183`      | `30`     |
    
    The output is logged as follows:
    
    ```text
    INFO {io.siddhi.core.stream.output.sink.LogSink} - Cleansed Data : Event{timestamp=1604581209943, data=[UNKNOWN, 183, 30.0], isExpired=false}
    ```


## Transforming data

The Streaming Integrator allows you to perform a wide range of transformations to the input data received. A main type of transformation supported is transforming a message from one format to another. In addition, you can perform a range of mathematical, regex, string, unit conversion, map, json, etc., transformations via [Siddhi Extensions](https://siddhi.io/en/v5.1/docs/extensions/). You can also write a custom script to perform an required transofrmation of data.

### Transforming message formats

WSO2 Streaming Integrator can transform message formats when it consumes data as well as when it publishes data.

#### Transforming the message format when consuming data

Data publishers, databases, files, and messaging systems send data to WSO2 Streaming Integrator in different data formats. Therefore, WSO2 Streaming Integrator has the capability to consume data in a range of formats. Once it receives the data in a specific format, it can convert it to a different format for processing purposes.

The Streaming Integrator can consume events in the default format in which they are sent/extracted or make changes to it to match the event schema as defined via the input stream.

- **Consuming messages in a default format**

    Consuming messages via WSO2 Streaming Integrator by configuring Siddhi sources is explained in [Extracting Data From Static Sources in Real Time](extracting-data-from-static-sources-in-real-time.md) and [Receiving Data in Trasit](receiving-data-in-transit.md).
    
    To receive data in a specific format, you need to annotate a mapper to the source configuration via the @map annotation. For more information, see [Siddhi Query Guide - Source Mapper](https://siddhi.io/en/v5.1/docs/query-guide/#source-mapper).
    
    To understand how to do this, consider factory where production bots publish the production amounts in a file. The the file contains rows in the CSV format. Therefore, WSO2 Streaming Integrator needs to consume the records in that format. Therefore, the source and the mapper can be configured as shown below.
    
    ```
    @source(type='file', mode='LINE',
        file.uri='file:/Users/foo/productions.csv',
        tailing='true',
        @map(type='csv'))
    define stream SweetProductionStream (name string, amount double);
    ```
    Here, in the `@map` annotation, `csv` is specified as the mapping type.
    
- **Consuming messages in a custom format**

    Custom mapping is performed when the schema of the event received needs to be updated to match the schema of the input stream.
    
    For this scenario, let's consider an example where the production bots are publishing messages via an HTTP sender in the JSON format, and as shown in the following example.
    
    ```json    
    {
      "sweet": "Jaffa Cake",
      "batch": {
        "batch id": "batch1",
        "count": 10
      }
    }
    ```
  
    The event stream is as follows:
    
    `define stream SweetProductionStream (name string, amount double);`
    
    In the given JSON event, the value for the `name` attribute is received with the `sweet` label, and the value for the `amount` attribute is published with the `count` label that is nested under `batch`. 
    
    In this example, you can use custom mapping to derive the required data from the JSON event using JSON expressions.
    
    | **Stream Attribute Name** | **JSON Event Attribute Name** | **JSON Path Expression** |
    |---------------------------|-------------------------------|--------------------------|
    | `name`                    | `sweet`                       | `$.sweet`                |
    | `amount`                  | `count`                       | `$.batch.count`          |
    
    Now you can include an attribute configuration within the mapping configuration based on the JSON path expressions you identified as shown below.
    
    ```
    @source(type='http', 
        receiver.url='http://localhost:5005/SweetProductionEP', 
        @map(type = 'json', 
            @attributes(name = '$.sweet', amount = '$.batch.count')))
    define stream SweetProductionStream (name string, amount double);
    ```
    The attributes configuration is annotated via the `@attributes` annotation within the `@map` annotation.

#### Transforming the message format when publishing data

The different destinations to which you need to publish data via WSO2 Streaming Integrator accept data in different formats. Therefore, WSO2 Streaming Integrator supports publishing in a range of formats.

WSO2 Streaming Integrator can publish messages in the default format or in a custom format. 

- **Publishing messages in default format**

    To understand this, consider a scenario where you receive production information in the JSON format, but you need to publish that information in a file in the XML format.
    
    To do this, you can define your output [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) and connect a [sink](https://siddhi.io/en/v5.1/docs/query-guide/#sink) to it with a [mapper](https://siddhi.io/en/v5.1/docs/query-guide/#sink-mapper) of the `xml` type. as shown below.
    
    ```
    @sink(type = 'file', 
        file.uri = "file:/Users/foo/productions.csv",
        @map(type = 'xml'))
    define stream OutputStream (name string, amount double);
    ```
    Here, you have included the `@map` annotation within the `@sink` annotation and specified the mapping type as `xml` so that the WSO2 Streaming Integrator can publish the output in XML format.
    
- **Publishing messages in custom format**

    The schema of the event accepted by the destination to which you are sending your output can be different to the schema of your input event or the schema of the event at the time you were processing it. Therefore, WSO2 Streaming Integrator allows you to perform custom mapings when you publish the output. The custom mapping needs to be annotated to the mapping configuration via the `@payload` annotation.
    
    To understand this, consider the example of a sweet factory that needs to send it's production report to the factory manager. The output stream in which the output is generated after processing can be as follows:
    
    ```
    define stream SweetProductionStream (name string, amount double);
    ```
    A sample JSON event that is published from the above schema without any custom mapping is as shown in the example below:
    
    ```json
    {
        "event":{
            "name":gingerbread,
            "amount":100
        }
    }
    ```
  
    However, the head office system expects to receive the event in the following format.
    
    ```json
    {"Product":{
        "ProductionData":{
            "Name":gingerbread,
            "Quantity":100
          }
      }
    }
    ```
  
    To achieve this, the sink connected to the output event stream needs to have a mapping configuration defined within it as shown below.
    
    ```
    @sink(type='inMemory', 
        topic='{{production}}', 
        @map(type='json', 
            enclosing.element='$.Product', 
            validate.json='true', 
            @payload( """{"ProductionData":{"Name":"{{name}}","Quantity":{{amount}}}}""")))
    define stream SweetProductionStream (name string, amount double);
    ```
  
    In the above example, the mapping type the JSON. The `@payload` annotation encloses a JSON string that defines the custom mapping. The values for `name` and `amount` attributes in the stream are presented as `Name` and `Quantity` respectively. These are nested under `ProductionData` which is turn is enclosed in the `Product` enclosing element as per the required format.

### Transforming with inline operators

WSO2 Streaming Integrator is shipped with inline operators that allow you to do certain transformations to streaming data without downloading additional Siddhi extensions from the Siddhi store.

For example, assume that instead of the amount produced during the specific production run, you need to present the total amount produced for the given product with each production run as well as the average. For this, you can write a query as follows:

```
define stream SweetProductionStream (name string, amount double);

@info(name = 'Calculate Total and Average')
from SweetProductionStream 
select name, amount, sum(amount) as total, avg(amount) as average 
group by name
insert into ProductionTotalsStream;
```

In this example, the input event that reports only the name of the product and the amount produced is transformed into an output event that reports the product name, amount produced, the total produced for the given product, and the average produced per production run for the given product. The `group by` clause ensures that the calculations are done per product name. The `sum()` and `avg()` inline operators calculate the total and the average recpectively.

### Transforming with supported Siddhi extensions

When you want to perform more advanced transformations that are not supported by the inline operators of the WSO2 Streaming Integrator, you can use one or more of the Siddhi extensions from the [Siddhi Store](https://store.wso2.com/store/assets/analyticsextension/list).

Some of these extensions are shipped with the WSO2 Streaming Integrator by default. If you want to use a Siddhi extension that is not shipped by default, you need to download and install it following the instructions in [Downloading and Installing Siddhi Extensions](../admin/downloading-and-Installing-Siddhi-Extensions.md).

The following table describes the complete list of extensions that provide data transformation functionality.

| **Extension** | **Description** |
|----------------------------------|---------------------------------------------------------------------------|
| [Siddhi-execution-math](https://siddhi-io.github.io/siddhi-execution-math/) | Transforms data by performing mathematical operations. |
| [Siddhi-execution-unitconversion](https://siddhi-io.github.io/siddhi-execution-unitconversion/) | Performs unit conversions ranging from length, weight, volume, etc. |
| [Siddhi-execution-string](https://siddhi-io.github.io/siddhi-execution-string/) | Performs string manipulations. |
| [Siddhi-execution-time](https://siddhi-io.github.io/siddhi-execution-time/)| Performs time-based transformations such as converting time zones. |
| [Siddhi-execution-map](https://siddhi-io.github.io/siddhi-execution-map/)| Converts events into maps and performs transformations such as concatenating and removing attributes. |
| [Siddhi-execution-reorder](https://siddhi-io.github.io/siddhi-execution-reorder/) | Rearranges the order of the incoming event flow. |
| [Siddhi-execution-json](https://siddhi-io.github.io/siddhi-execution-json/) | Performs manipulations to JSON strings. |

### Writing a custom script to transform data

To write a custom script to transform data, you can use the [siddhi-script-js](https://siddhi-io.github.io/siddhi-script-js/) Siddhi extension.

For example, if the Sweet Factory wants to check whether the production amount reported in a production run is greater than the average production up to that production run, you can write a custom function shown in the following sample query.

```
from ProductionTotalsStream 
select name, amount, total, average, js:eval("amount > average", 'bool') as exceedsAverage
group by name 
insert into MonitorProductionTrendStream;
```

Here, `js:eval("amount > average", 'bool') as exceedsAverage` is a custom function where an attribute named `exceedsAverage` returns the value `true` or `false` depending on whether the value for the `amount` attribute exceeds the value for the `average` attribute.

### Try it out

To try out the transformations described above with some of the given examples, follow the steps below:

1. [Start and Access Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).

2. Open a new file. Then add and save the following Siddhi application.

    ```
    @App:name('ProductionTotalsApp')
    @App:description('Transform Production Statistics')
    
    @source(type = 'http', receiver.url = "http://localhost:5005/SweetProductionEP",
        @map(type = 'json',
            @attributes(amount = "$.batch.count", name = "$.sweet")))
    define stream SweetProductionStream (name string, amount double);
    
    @sink(type = 'file', file.uri = "file:/Users/foo/productions.json",
        @map(type = 'json', validate.json = "false", enclosing.element = "$.Product",
            @payload("""{"ProductionData":{"Name":"{{name}}","Quantity":"{{amount}}","Total":{{total}},"Average":{{average}}}}""")))
    define stream ProductionTotalsStream (name string, amount double, total double, average double);
    
    @sink(type = 'log', prefix = "Exceeds Average",
        @map(type = 'text'))
    define stream MonitorProductionTrendStream (name string, exceedsAverage bool);
    
    @info(name = 'Calculate Total and Average')
    from SweetProductionStream 
    select name, amount, sum(amount) as total, avg(amount) as average 
        group by name 
    insert into ProductionTotalsStream;
    
    @info(name = 'Compare with Average')
    from ProductionTotalsStream 
    select name, js:eval("('amount' > 'average')", 'bool') as exceedsAverage
    group by name 
    insert into MonitorProductionTrendStream;
    ```
   
   This Siddhi application does the following transformations:
   
   - Derives the values for `name` and `amount` attributes representing the name of the product and the amount produced. This is derived from input events sent is a custom format where the two required values are provided under the `Sweet` and `count` attributes, and the `count` attribute is nested under another attribute named `batch`.
   
   - Publishes the production statistics in a custom format. `name` and `amount` attributes are presented as `Name` and `Quantity`, and nested under `ProductionData` in the `Product` enclosing element. These events are published in the `Users/foo/productions.json` file.
   
        !!! tip
            You can save the `productions.json` file mentioned above in a different location of your choice if required.
   
   - Calculates the total production amount and the average production amount per sweet, and presents them as values for the `total` and `average` attributes in the output event published in the `productions.json` file.
   
   - Uses a custom script to check whether the amount produced of a sweet in the production run is greater than the average production for that sweets, and logs `true` or `false` in the terminal in the text format.
   
3. To simulate events for this Siddhi application, issue the following six CURL commands.

    ```
    curl -X POST \
      http://localhost:5005/SweetProductionEP \
      -H 'content-type: application/json' \
      -d '{
      "sweet": "Jaffa Cake",
      "batch": {
        "batch id": "batch1",
        "count": 10
      }
    }'
    ``` 
   
    ```
    curl -X POST \
    http://localhost:5005/SweetProductionEP \
    -H 'content-type: application/json' \
    -d '{
    "sweet": "Gingerbread",
    "batch": {
     "batch id": "batch1",
     "count": 65
    }
    }'
    ``` 
     
    ```
    curl -X POST \
     http://localhost:5005/SweetProductionEP \
     -H 'content-type: application/json' \
     -d '{
     "sweet": "Jaffa Cake",
     "batch": {
       "batch id": "batch1",
       "count": 15
     }
    }'
    ``` 
   
    ```
    curl -X POST \
    http://localhost:5005/SweetProductionEP \
    -H 'content-type: application/json' \
    -d '{
    "sweet": "Gingerbread",
    "batch": {
     "batch id": "batch1",
     "count": 55
    }
    }'
    ``` 
      
    ```
    curl -X POST \
    http://localhost:5005/SweetProductionEP \
    -H 'content-type: application/json' \
    -d '{
    "sweet": "Jaffa Cake",
    "batch": {
      "batch id": "batch1",
      "count": 25
    }
    }'
    ``` 
   
    ```
    curl -X POST \
    http://localhost:5005/SweetProductionEP \
    -H 'content-type: application/json' \
    -d '{
    "sweet": "Gingerbread",
    "batch": {
      "batch id": "batch1",
      "count": 45
    }
    }'
    ``` 
    
4. Open the `Users/foo/productions.json` file. The following content is available in it.

    ![File Log](../images/transforming-date/output.png)

5. Check the Streaming Integrator Tooling terminal. The following is logged in it.

    ![Termial Log](../images/transforming-date/terminal-log.png)

## Enriching data

Enriching data involves integrated the data received into a streaming integration flow with data from other medium such as a data store, another data stream, or an external service to derive an expected result.

### Integrating data streams and static data

### Integrating multiple data streams

### Integrating data streams with external services

### Enriching data with built-in extensions

## Summarizing data

Summarizing data refers to obtaining aggregates in an incremental manner for a specified set of time periods.

### Aggregating data based on time

### Performing short term summarizations



## Correlating data

### Correlating events to find a pattern

#### Count and match multiple events for a given pattern condition

#### Combine several patterns logically and match events

#### Find non-occurrence of events

### Correlating events to find a trend(sequence)

#### Count and match multiple events for a given trend

#### Combine several trends logically and match events

The streaming integrator can correlate data in order to detect patterns and trends in streaming data. Correlating can be done via patterns as well as sequences.