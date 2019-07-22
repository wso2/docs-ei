#Consuming Messages

## Introduction

The first step in a streaming integration flow is to consume the data to be cleaned, enriched, transformed or summarized 
to produce the required output. 
 
For the Streaming Integrator to consume events, the following is required.

* A message schema: The Streaming Integrator identifies the messages that it selects into a streaming integration flows by their schemas. The schema based on which the messages are selected are defined via a *stream*.

* A source: The messages are consumed from different sources including streaming applications, cloud-based applications, databases, and files. The source is defined via a *source configuration*.
  ![Receiving events](../images/consuming-messages/ConsumingMessages.png)
  
  A source configuration consists of the following:
  
  + **`@source`**: This annotation defines the source type via which the messages are consumed, and allows you to configure the source parameters (which change depending on the source type). For the complete list of supported source types, see [Siddhi Query Guide - Source](https://siddhi.io/en/v4.x/docs/query-guide/#source)
  + **`@map`**: This annotation specifies the format in which messages are consumed, and allows you to configure the mapping parameters (which change based of the mapping type/format selected). For the complete list of supported mapping types, see [Siddhi Query Guide - Source Mapper](https://siddhi.io/en/v4.x/docs/query-guide/#source-mapper)
  + **`@attributes`**: This annotation specifies a custom mapping based on which events to be selected into the streaming integration flow are identified. This is useful when the attributes of the incoming messages you want the Streaming Integrator to consume are different to the corresponding attribute name in the stream definition. e.g., In a scenario where the Streaming Integrator is reading employee records, the employee name might be defined as `emp No` in the database from which you are extracting the records. However, the corresponding attribute name in the stream definition is `employeeNo` because that is how yoy want to refer to the attribute in the streaming integration flow. In this instance, you need a custom mapping to indicate that `emp No` is the same as `employeeNo`.
  
  
  In a Siddhi application, you can define a source configuration inline or refer to a source configuration defined externally in a configuration file.

## Defining event source inline in the Siddhi application

To create a Siddhi application with the source configuration defined inline, follow the steps below.

1. Open the Streaming Integrator Studio and start creating a new Siddhi application. For more information, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
2. Enter a name for the Siddhi application as shown below.<br/>
   `@App:name("<Siddhi_Application_Name>)`<br/>e.g., `@App:name("SalesTotalsApp")`<br/>
   
3. Define an input stream to define the schema based on which input events are selected to the streaming integrator flow as follows.

    `define stream <Stream_Name>(attribute1_name attribute1_type, attribute2_name attribute2_type, ...)`<br/>
    e.g., `define stream ConsumeSalesTotalsStream (transNo int, product string, price int, quantity int, salesValue long)`<br/>
    
    
4. Connect a source to the input stream you added as follows.
    ```
    @source(type='<SOURCE_TYPE>')
    define stream <Stream_Name>(attribute1_name attribute1_type, attribute2_name attribute2_type, ...);
    ```
    e.g., You can add a source of the `http` type to the `ConsumeSalesTotalsStream` input stream in the example of the previous step.
    ```
    @source(type='<http>')
    define stream ConsumeSalesTotalsStream (transNo int, product string, price int, quantity int, salesValue long);
    ```
5. Configure parameters for the source you added.
   e.g., You can specify an HTTP URL for the `http` source in the example used.
   
       ```
       @source(type='http', receiver.url='http://localhost:5005/SweetProductionEP')
       define stream ConsumeSalesTotalsStream (transNo int, product string, price int, quantity int, salesValue long);
       ```
       
6. Complete the Siddhi application by adding the rest of the required Siddhi constructs as follows.<br/>
     
    1. To publish the output, define an output stream and connect a sink to it as follows.<br/>
        
        !!!info
                Publishing the output is explained in detail in the [Publishing Data guide](publishing-data.md).
        
        ```jql
        @sink(type='<SINK_TYPE>')
        define stream <Stream_Name>(attribute1_name attribute1_type, attribute2_name attribute2_type, ...);
        ```<br/>
        
        e.g., Assuming that you are publishing the events with the existing values as logs in the output console without any further processing, you can add an output stream connected to a sink as follows.
            
            ```
            @source(type='log', prefix='Sales Totals:')
            define stream PublishSalesTotalsStream (transNo int, product string, price int, quantity int, salesValue long);
            ```
            
      
    2. Add a Siddhi query to specify how the output is derived.
        ```jql
        from <INPUT_STREAM_NAME>
        select <ATTRIBUTE1_Name>, <ATTRIBUTE2_NAME>, ... 
        group by <ATTRIBUTE_NAME>
        insert into <OUTPUT_STREAM_NAME>;
        ```

    e.g., Assuming that you are publishing the events with the existing values as logs in the output console without any further processing, you can define the query as follows.
    
        ```jql
        from ConsumeSalesTotalsStream
        select transNo, product, price, quantity, salesValue
        group by product
        insert into PublishSalesTotalsStream;
        ```
        
7. Save the Siddhi Application.

    

## Defining event source externally in the configuration file

If you want to use the same source configuration in multiple Siddhi applications, you  can define it externally in the 
`<SI_HOME>/conf/server/deployment.yaml` file and then refer to it from Siddhi applications. To understand how to do this, 
follow the procedure below.


1. Open the `<SI_HOME>/conf/server/deployment.yaml` file.
2. Add a section named `siddi`, and then add a subsection named `refs:` as shown below.
    ```jql    
    siddhi:  
     refs:
      -
    ```
3. In the `refs` subsection, enter a parameter named `name` and enter a name for the source.
    ```jql    
    siddhi:  
     refs:
      -
       name:`<SOURCE_NAME>`
    ```
    
4. To specify the source type, add another parameter named `type` and enter the relevant source type.
    ```jql
    siddhi:  
     refs:
      -
       name:'<SOURCE_NAME>'
       type: '<SOURCE_TYPE>'
    ```
5. To configure other parameters for the source (based on the source type), add a subsection named `properties` as shown below.
    ```jql
    siddhi:  
     refs:
      -
       name:'SOURCE_NAME'
       type: '<SOURCE_TYPE>'
       properties
           <PROPERTY1_NAME>:'<PROPERTY1_VALUE>'
           <PROPERTY2_NAME>:'<PROPERTY2_VALUE>'
           ...
    ```
    
6. Save the configuration file.

e.g., The HTTP source used as the example in the previous section can be defined externally as follows:
```jql
siddhi:  
 refs:
  -
   name:'HTTPSource'
   type: 'http'
   properties
       receiver.url:'http://localhost:5005/SweetProductionEP'
```

The source configuration you added can be referred to in a Siddhi application as follows:
```jql
@source(ref='SOURCE_NAME')
define stream ConsumeSalesTotalsStream (transNo int, product string, price int, quantity int, salesValue long);
```
e.g., The HTTP source that you previously created can be referred to as follows.

```
@source(ref='HTTP')
define stream ConsumeSalesTotalsStream (transNo int, product string, price int, quantity int, salesValue long);
```
### Supported Event Source types
<source categories table here> 

### Supported message formats

#### Consuming a message in default format

#### Consuming a message in custom format