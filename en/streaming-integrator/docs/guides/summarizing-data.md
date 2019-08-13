# Summarizing Data

## Introduction

Summarizing data refers to obtaining aggregates in an incremental manner for a specified set of time periods.

## Performing clock-time-based summarization

Performing clock-time based based summarization involves calculating, storing, and then retrieving aggregations for a 
selected range of time granularities. This process is carried out in two parts:

1. Calculating the aggregations for the selected time granularities and storing the results.
2. Retrieving previously calculated aggregations for selected time granularities. 

To understand data summarization further, consider the scenario where a business that sells multiple brands stores its sales data in a physical database for the purpose of retrieving them later to perform sales analysis. Each sales transaction is received with the following details:
                                                                                                                                                
`symbol`: The symbol that represents the brand of the items sold.
`price`: the price at which each item was sold.
`amount`: The number of items sold.

The Sales Analyst needs to retrieve the total number of items sold of each brand per month, per week, per day etc., and then retrieve these totals for specific time durations to prepare sales analysis reports.
                                                                                                                                                
!!!info
    It is not always required to maintain a physical database for incremental analysis, but it enables you to try out your aggregations with ease.
    
The following sections explain how to calculate and store time-based aggregations for this scenarios, and then retrieve them.


### Calculate and store clock-time-based aggregate values
To calculate and store time-based aggregation values for the scenario explained above, follow the procedure below.

1. To capture the input events based on which the aggregations are calculated, define an input stream as follows.
    `define stream TradeStream (symbol string, price double, quantity long, timestamp long);`
    
    !!!info
        In addition to the `symbol`, `price`, and `quantity` attributes to capture the input details already mentioned, 
        the above stream definition includes an attribute named timestamp to capture the time at which the sales transaction 
        occurs. The aggregations are executed based on this time. This attribute's value could either be a long value 
        (reflecting the Unix timestamp in milliseconds), or a string value adhering to one of the following formats.
        
        - **`<YYYY>-<MM>-<dd> <HH>:<mm>:<ss> <Z>`**: This format can be used if the timezone needs to be specified explicitly. Here the ISO 8601 UTC offset must be provided for <Z> . e.g., +05:30 reflects the India Time Zone. If time is not in GMT, this value must be provided.)
        - **`<yyyy>-<MM>-<dd> <HH>:<mm>:<ss> '**: This format can be used if the timezone is in GMT.
   

2. To persist the aggregates that are calculated via your Siddhi application, include a store definition as follows. if not the data is stored in-memory and lost when siddhi app is stopped.

    ```sql
    define stream TradeStream (symbol string, price double, quantity long, timestamp long);
    
    @store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/TestDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")

    ```

    !!!info
        If the store definition is not provided, the data is stored in-memory, and then there is a risk of it being lost when the Siddhi application is stopped.
   
3. Define an aggregation as follows. You can name it `TradeAggregation`.

    !!!info
        When you save aggregate values in a store, the system uses the aggregation name you define here as part of the database table name. Table name is `<Aggregation_Name>_<Granularity>`. Some database types have length limitations for table names (e.g., for Oracle, it is 30 characters). Therefore, you need to check whether the database type you have used for the store has such table name length limitation, and make sure that the aggregation name does not exceed that maximum length.
        
    `define aggregation TradeAggregation;`


4. To calculate aggregations, include a query as follows:

    1. To get input events from the TradeStream stream that you previously defined, add a `from` clause as follows.
        `from TradeStream `
    
    2. To select attributes to be included in the output event, add a `select` clause as follows.
        `select symbol, avg(price) as avgPrice, sum(quantity) as total`
    
    3. To group the output by the symbol, add a group by clause as follows.
        `group by symbol`
    
    4. The timestamp included in each input event allows you to calculate aggregates for the range of time granularities seconds-years. Therefore, to calculate aggregates for each time granularity within this range, add the aggregate by clause to this aggregate query as follows.
        `aggregate by timestamp every sec ... year;`
        
The completed Siddhi application is as follows.
```
@App:name('TradeApp')
 
define stream TradeStream (symbol string, price double, quantity long, timestamp long);
 
@store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/TestDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")
 
define aggregation TradeAggregation
from TradeStream
select symbol, avg(price) as avgPrice, sum(quantity) as total
group by symbol
aggregate by timestamp every sec ... year;

```


### Retrieve the stored aggregate values

This section  involves retrieving the aggregate values that you calculated and persisted in the [Calculate and store clock-time-based aggregate values subsection](#calculate-and-store-clock-time-based-aggregate-values). 
To do this, let's add the Siddhi definitions and queries required for retrieval to the `TradeApp` Siddhi application that you have already started creating for this scenario.

1. Open the `TradeApp` Siddhi application.

2. To retrieve aggregations, you need to make retrieval requests. To capture these requests as events, let's define a stream as follows.
    `define stream TradeSummaryRetrievalStream (symbol string);`
    
3. To process the events captured via the `TradeSummaryRetrievalStream` stream you defined, add a new query as follows.

    ```
    from TradeSummaryRetrievalStream as b join TradeAggregation as a
    on a.symbol == b.symbol 
    within "2014-02-15 00:00:00 +05:30", "2014-03-16 00:00:00 +05:30" 
    per "days" 
    select a.symbol, a.total, a.avgPrice 
    insert into TradeSummaryStream;

    ```
    
The completed Siddhi application is as follows.

```
@App:name('TradeApp')
 
define stream TradeStream (symbol string, price double, quantity long, timestamp long);

define stream TradeSummaryRetrievalStream (symbol string);
 
@store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/TestDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")
 
define aggregation TradeAggregation

@info(name = 'CalculatingAggregates') 
from TradeStream
select symbol, avg(price) as avgPrice, sum(quantity) as total
group by symbol
aggregate by timestamp every sec ... year;

@info(name = 'RetrievingAggregates') 
from TradeSummaryRetrievalStream as b join TradeAggregation as a
on a.symbol == b.symbol 
within "2014-02-15 00:00:00 +05:30", "2014-03-16 00:00:00 +05:30" 
per "days" 
select a.symbol, a.total, a.avgPrice 
insert into TradeSummaryStream;

```

## Summarizing data based on built in windowing criterias

This section explains how to 
 - siddhi-execution-unique
 
 Other windows will also be explained here.
