# Processing Streaming Events

[Forrester](https://reprints.forrester.com/#/assets/2/202/%27RES136545%27/reports)
defines Streaming Analytics as follows:

!!! info

"Software that provides analytical operators to **orchestrate data
flow** , **calculate analytics** , and **detect patterns** on event
data from **multiple, disparate live data sources** to allow developers
to build applications that **sense, think, and act in real time** .


The stream processing capabilities of WSO2 SP allow you to capture high
volume data flows and process them in real time, and present results in
a streaming manner.

Following are a few stream processing capabilities of WSO2 SP.

## Functions

The following functions shipped with Siddhi, consume zero, one or more
parameters from streaming events and produce a desired value as output.
These functions are executed per event. For more information on Siddhi
functions, please refer to [Siddhi Query Guide -
Functions.](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#function)
More functions are made available as [Siddhi
Extensions](https://siddhi-io.github.io/siddhi/extensions/) .

-   eventTimestamp - Returns the timestamp of the processed event

    **eventTimeStamp example**

    ``` sql
        from fooStream
        select symbol as name, eventTimestamp() as eventTimestamp
        insert into barStream
    ```

-   UUID - Generates a UUID (Universally Unique Identifier)

    **UUID example**

    ``` sql
            from fooStream
            select UUID() as messageID, messageDetails
            insert into barStream;
    ```

-   default - Checks if the 'attribute' parameter is null and if so
    returns the value of the 'default' parameter. The function is given
    as default(\<attribute\>, \<default value\>)

    **default example**

    ``` sql
            from fooStream
            select default(temp, 0.0) as temp, roomNum
            insert into barStream;
    ```

-   cast - Converts the first parameter according to the cast-to
    parameter. Incompatible arguments cause Class Cast exceptions if
    further processed.

    **cast example**

    ``` sql
            from fooStream
            select symbol as name, cast(temp, 'double') as temp
            insert into barStream;
    ```

-   convert - Converts the first input parameter according to the
    convert-to parameter

    **convert example**

    ``` sql
            from fooStream
            select convert(temp, 'double') as temp
            insert into barStream;
    ```

-   ifThenElse - Evaluates the 'condition' parameter and returns value
    of the 'if.expression' parameter if the condition is true, or
    returns value of the 'else.expression' parameter if the condition is
    false. The function is given as ifThenElse(\<condition\>,
    \<if.expression\>, \<else.expression\>)

    **ifThenElse example**

    ``` sql
            from fooStream
            ifThenElse(sensorValue>35,'High','Low')
            insert into barStream;
    ```

-   minimum - Returns the minimum value of the input parameters

    **minimum example**

    ``` sql
            from fooStream
            select minimum(price1, price2, price3) as minPrice
            insert into barStream;
    ```

-   maximum - Returns the maximum value of the input parameters. This
    function could be used similar to how 'minimum' function is used in
    a query.
-   coalesce - Returns the value of the first input parameter that is
    not null. All input parameters have to be of the same type.

    **coalesce example**

    ``` sql
            from fooStream
            select coalesce('123', null, '789') as value
            insert into barStream;
    ```

-   instanceOfBoolean - Returns 'true' if the input is a instance of
    Boolean. Otherwise returns 'false'.

    **instanceOfBoolean example**

    ``` sql
            from fooStream
            select instanceOfBoolean(switchState) as state
            insert into barStream;
    ```

-   instanceOfDouble - Returns 'true' if the input is a instance of
    Double. Otherwise returns 'false'. This function could be used
    similar to how ' instanceOfBoolean ' function is used in a query.
-   instanceOfFloat - Returns 'true' if the input is a instance of
    Float. Otherwise returns 'false'. This function could be used
    similar to how 'instanceOfBoolean' function is used in a query.
-   instanceOfInteger - Returns 'true' if the input is a instance of
    Integer. Otherwise returns 'false'. This function could be used
    similar to how 'instanceOfBoolean' function is used in a query.
-   instanceOfLong - Returns 'true' if the input is a instance of Long.
    Otherwise returns 'false'. This function could be used similar to
    how 'instanceOfBoolean' function is used in a query.
-   instanceOfString - Returns 'true' if the input is a instance of
    String. Otherwise returns 'false'. This function could be used
    similar to how 'instanceOfBoolean' function is used in a query.

## Filters

Filters are applied to input data received in streams to filter
information based on given conditions. For more information, see [Siddhi
Query Guide -
Filters](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#filter)
.

e.g., Filtering cash withdrawals from an ATM machine where the
withdrawal amount is greater tha $100, and the withdrawal data is
between 01/12/2017-15/12/2017.

## Windows

Windows allow you to capture a subset of events based on a duration or
number of events criterion, from an input stream for calculation. Each
input stream can only have a maximum of one window.

##### Criterion - Time windows vs length windows

The subset of events can be captured based on one of the following.

-   **Time** : This involves capturing all the events that arrive during
    a specific time interval (e.g., writing a query that is applicable
    to events that occur during a period of 10 minutes).
-   **Length** : This involves capturing a subset of events based on the
    number of events (e.g., writing a query applicable to each group
    that consists of 10 events).

##### Method of processing - Sliding windows vs batch windows

Consider 10 events that have arrived in a stream.

When a sliding length window is included in a Siddhi query, the
following event groups are identified:

-   Events 1-5
-   Events 2-6
-   Events 3-7
-   Events 4-8
-   Events 5-9
-   Events 6-10

When a batch window is included in a Siddhi query, the folowing event
groups are identified:

-   Events 1-5
-   Events 6-10

This window feature differs from the Defined Window concept elaborated
in [Siddhi Application Overview](_Siddhi_Application_Overview_) due to
this being specific to a single query only. If a window is to be shared
among queries, the [Defined
Window](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#defined-window)
must be used

For more information about windows, see [Siddhi Query Guide -
Window](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#window)
.

## Aggregate Functions

Aggregation functions allow executing aggregations such as sum, avg,
min, etc. on a set of events grouped by a window. If a window is not
defined, the aggregation(s) would be calculated by considering all the
events arriving at a stream.

Consider the following events arriving at a stream, where the prices
vary from one another.

-   Event 1: price = 10.00
-   Event 2: price = 20.00
-   Event 3: price = 30.00
-   Event 4: price = 40.00
-   Event 5: price = 50.00

Consider the following two queries, where sum of price is calculated
based on a length window of 2, and without a window respectively

**Query1: Aggregate based on length window**

``` sql
    from fooStream#window.length(2)
    select sum(price) as totalPrice
    insert into barStream;
```

**Query2: Aggregate without a window**

``` sql
    from fooStream
    select sum(price) as totalPrice
    insert into barStream;
```

  

The following output would be generated for Query 1.

-   totalPrice = 10.00
-   totalPrice = 30.00
-   totalPrice = 50.00
-   totalPrice = 70.00
-   totalPrice = 90.00

The following output would be generated for Query 2.

-   totalPrice = 10.00
-   totalPrice = 30.00
-   totalPrice = 60.00
-   totalPrice = 100.00
-   totalPrice = 150.00

For more information on aggregate function, please refer to [Siddhi
Query Guide - Aggregate
Functions](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#aggregate-function)
.

## Group By

With the group by  functionality, events could be grouped based on a
certain attribute, when performing aggregations.

Consider the following events, which have a symbol attribute and a price
attribute.

-   Event 1: symbol = wso2, price = 10.00
-   Event 2: symbol = wso2, price = 20.00
-   Event 3: symbol = abc, price = 30.00
-   Event 4: symbol = abc, price = 40.00
-   Event 5: symbol = abc, price = 50.00

When the sum aggregation is calculated for a window of length 3, after
grouping by symbol, the given output is generated

**Query1: Aggregate based on length window**

``` sql
    from fooStream#window.length(3)
    select symbol, sum(price) as totalPrice
    group by symbol
    insert into barStream;
```

  

Output:

-   symbol = wso2, totalPrice = 10.00
-   symbol = wso2, totalPrice = 30.00
-   symbol = abc, totalPrice = 30.00
-   symbol = abc, totalPrice = 70.00
-   symbol = abc, totalPrice = 120.00

For more information on group by, please refer to [Siddhi Query Guide -
Group
By](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#group-by)
.

## Having

Having allows to filter events after processing the select statement.

This is useful if the filtering is based on some value derived by
applying a function/ aggregation. For example, if you want to find all
the events where maximum production total across 3 days is less than
1000 units, such filtering could be achieved with a query as follwos

**Having example**

``` sql
    from fooStream
    select item, maximum(productionOnDay1, productionOnDay2, productionOnDay3) as maxProduction
    having maxProduction < 1000
    insert into barStream;
```

For more information on having clause, please refer to [Siddhi Query
Guide -
Having](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#having)
.

## Join

Join is an important feature of Siddhi, which allows combining pair of
streams, pair of windows, stream with window, stream/ window with a
table and stream/window with an aggregation

The join logic can be defined with 'on' condition as well, which
restricts the events combined in a join.

For example, assume that we need to combine a transaction stream with a
table containing blacklisted credit card numbers, to identify fraudulent
transactions. Following query helps achieve such a requirement.

**Join query example**

``` sql
    from transactionStream as t join blacklistedCardsTable as b
    on t.cardNumber = b.cardNumber
    select t.cardNumber, t.transactionDetails, b.fraudDescription
    insert into suspiciousTransactionStream;
```

For more information on join queries, please refer to [Siddhi Query
Guide -
Join](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#join-stream)
.

## Output Rate Limiting

Output rate limiting allows queries to output events periodically based
on a specified condition. This helps to limit continuously sending
events as output.

For more information on output rate limiting, please refer to [Siddhi
Query Guide - Output Rate
Limiting](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#output-rate-limiting)

## Partitioning

Partitioning in Siddhi allows to logically seperate events arriving at a
stream, and to process them separately, in parallel.

For example, assume that the total number of transactions per company
needs to be monitored at a stock exchange. However, if all the
transactions are arriving at a single stream, we would need to logically
seperate them based on the company symbol. The following example depicts
how this can be achieved with Siddhi partitioning.

**Partition example**

``` sql
    partition with (symbol of stockStream )
    begin
        from stockStream
        select symbol, count() as transactionCount
        insert into transactionsPerSymbol;
    end;
```

Partitioning can be done based on an attribute value as above, or based
on a condition. For more information on partitioning, please refer to
[Siddhi Query Guide -
Partitioning](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#partition)

## Trigger

Triggers could be used to get events generated by the system itself,
based on some time duration.

An example for a trigger definition is as follows.

**Trigger example**

``` sql
    define trigger FiveMinTriggerStream at every 5 sec;
```

This would generate an event every 5 seconds. The generated event would
contain an attribute of type 'Long' named 'triggered\_time', reflecting
the time at which event was triggered in milliseconds (epoch time).

Trigger could be defined as a time interval, a cron job or to generate
an event when Siddhi is started. For more information on triggers,
please refer to [Siddhi Query Guide -
Trigger](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#trigger)

## Script

Scripts allow to define function operations in a different programming
language. An example is as follows.

**Script example**

``` sql
    define function concatFn[javascript] return string {
        var str1 = data[0];
        var str2 = data[1];
        var str3 = data[2];
        var response = str1 + str2 + str3;
        return response;
    };
    
    define stream TempStream(deviceID long, roomNo int, temp double);
    
    from TempStream
    select concatFn(roomNo,'-',deviceID) as id, temp 
    insert into DeviceTempStream;
```

For more information on scripts, please refer to [Siddhi Query Guide -
Script](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#script)
