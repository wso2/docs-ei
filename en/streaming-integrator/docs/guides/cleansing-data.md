# Cleansing Data

## Introduction

When you receive input data via the Streaming Integrator, it may consist of data that is not required to generate the 
required output, null values for certain attributes, etc.  Cleansing data refers to refining the input data received by 
applying null values, 

## Filtering data based on conditions

To understand the different ways you can filter the specific data you need to transform and enrich in order to generate 
the required output, follow the procedures below:
    
    
 - **Filtering based on exact match of attribute:**
 1. Open the Streaming Integrator Studio and start creating a new Siddhi application. For more information, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
 2. Enter a name for the Siddhi application via the `@App:name` annotation. In this example, let's name it `TemperatureApp`.
 3. Define an input stream to specify the schema based on which events are selected to the Streaming Integration flow.
    ```
    define stream TempStream (deviceID long, roomNo string, temp double);
    ```
    !!!info
        For more information about defining input streams to receive events, see the [Consuming Data guide](consuming-messages.md).
 
 4. Add a query to generate filtered temperature readings as follows. For this example, let's assume that you want to 
 filter only temperature readings for a specific room number (e.g., room no `2233`).
    1. Add the `from` clause and enter `TempStream` as the input stream from which the input data. However, because you 
    only need to extract readings for room no `2233`, include a filter in the `from` clause as shown below:
        ```
        from TempStream [roomNo=='2233']
        ```
    2. Add the `select` clause with `*` to indicate that all the attributes should be selected without any changes.
        ```
        from TempStream [roomNo=='2233']
        select *
        ```
    3. Add the `insert to` clause and direct the output to a stream named `FilteredResultsStream`.
        !!!info
            Note that the `FilteredResultsStream` is not defined in the Siddhi application. It is inferred by specifying
             it as an output stream.
        ```
        from TempStream [roomNo=='2233']
        select *
        insert into FilteredResultsStream;
        ```
       The saved Siddhi application is as follows:
        ```
        @App:name("TemperatureApp")
        @App:description("Description of the plan")
        
        define stream TempStream (deviceID long, roomNo string, temp double);
        
        
        from TempStream [roomNo=='2233']
        select *
        insert into FilteredResultsStream;
        ```
 - **Filtering based on pattern match of attribute - regex extension**
    For this purpose, you can use the `TemperatureApp` Siddhi application that you created in the previous example. 
    
 - **Filtering based on multiple criteria**
    For this purpose, you can use the `TemperatureApp` Siddhi application that you created in the previous example. 
    However, instead of filtering only readings for room No `2233`, assume that you need to filter the readings for a range
    of rooms (e.g., rooms 100-210) where the temperature is greater than 40. For this, you can update the filter as follows.
    
    `[(roomNo >= 100 and roomNo < 210) and temp > 40]`
    
    Here, the `and` logical expression is used to indicate that both the filter conditions provided need to be considered
    

## Modifying, removing and replacing attributes
Modifying and replacing will be referred to transform and enriching sections

## Handling attributes with null values
