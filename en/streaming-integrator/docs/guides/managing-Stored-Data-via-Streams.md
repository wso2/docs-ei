# Managing Stored Data via Streams

This section covers how to manage stored data in event tables via
streams.

-   [Inserting records](#ManagingStoredDataviaStreams-Insertingrecords)
-   [Retrieving
    records](#ManagingStoredDataviaStreams-Retrievingrecords)
-   [Updating a table](#ManagingStoredDataviaStreams-Updatingatable)
-   [Deleting Records](#ManagingStoredDataviaStreams-DeletingRecords)
-   [Searching records](#ManagingStoredDataviaStreams-Searchingrecords)
-   [Inserting/updating
    records](#ManagingStoredDataviaStreams-Inserting/updatingrecords)

## Inserting records

#### Prerequisites

In order to insert events to a table:

-   [General
    prerequisites](#ManagingStoredDataviaStreams-GeneralPrerequisites)
    should be completed.
-   The event stream from which the events to be inserted are taken
    should be defined.
-   The table to which events are to be inserted should be defined. For
    more information, see [Defining a
    table](#ManagingStoredDataviaStreams-Define) .

#### Query syntax

The following is the syntax to insert events into a table.

``` sql
    from <STREAM_NAME>
    select <ATTRIBUTE1_NAME>, <ATTRIBUTE2_NAME>, <ATTRIBUTE3_NAME> ...
    insert into <TABLE_NAME>;
```

  

#### Example

The following query inserts events from the FooStream stream to the
`         FooTable        ` table with the `         symbol        ` ,
`         price        ` , and `         volume        ` attributes.

``` sql
    from FooStream
    select symbol, price, volume
    insert into FooTable;
```

## Retrieving records

#### Prerequisites

In order to retrieve events from a table:

-   [General
    prerequisites](#ManagingStoredDataviaStreams-GeneralPrerequisites)
    should be completed.
-   The table to be read should be already defined. For more
    information, see [Defining a
    table](#ManagingStoredDataviaStreams-Define) .
-   One or more events should be inserted into the table. For more
    information, see [Inserting
    events](#ManagingStoredDataviaStreams-Insert) .

#### Query syntax

The following is the query syntax to retrieve events from an existing
table. For more information, please refer to [Siddhi Query Guide - Join
Table](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#join-table)
.

``` sql
    from <STREAM_NAME> join <TABLE_NAME>
        on <CONDITION>
    select (<STREAM_NAME>|<TABLE_NAME>).<ATTRIBUTE1_NAME>, (<STREAM_NAME>|<TABLE_NAME>).<ATTRIBUTE2_NAME>, ...
    insert into <OUTPUT_STREAM>
```

#### Example

The following query joins the `         FooStream        ` events with
the events stored in the `         StockTable        ` table. An output
event is created for each matching pair of events, and it is inserted
into another stream named `         OutputStream        ` .

``` sql
    from FooStream#window.length(1) join StockTable
    select FooStream.symbol as checkSymbol, StockTable.symbol as symbol, StockTable.volume as volume 
    insert into OutputStream
```

The information inserted with the output event is as follows.

| Source                                      | Value of                                    | Attribute name in the output event       |
|---------------------------------------------|---------------------------------------------|------------------------------------------|
| `             FooStream            ` stream | `             symbol            ` attribute | `              checkSymbol             ` |
| `             StockTable            ` table | `             symbol            ` attribute | `             symbol            `        |
| `             StockTable            ` table | `             volume            ` attribute | `             volume            `        |

## Updating a table

This section explains how to update the selected records of an existing
table.

#### Prerequisites

In order to update events in a table:

-   [General prerequisites](#ManagingStoredDataviaStreams-General)
    should be completed.
-   The table to be updated should be already defined. For more
    information, see [Defining a
    table](#ManagingStoredDataviaStreams-Define) .
-   The event stream with the events based on which the updates are made
    must be already defined.
-   One or more events should be inserted into the table. For more
    information, see [Inserting
    events](#ManagingStoredDataviaStreams-Insert) .

#### Query syntax

``` sql
    from <STREAM_NAME> 
    select <ATTRIBUTE1_NAME>, <ATTRIBUTE2_NAME>, ...
    update <TABLE_NAME> (for <OUTPUT_EVENT_TYPE>)? 
        set <TABLE_NAME>.<ATTRIBUTE_NAME> = (<ATTRIBUTE_NAME>|<EXPRESSION>)?, <TABLE_NAME>.<ATTRIBUTE_NAME> = (<ATTRIBUTE_NAME>|<EXPRESSION>)?, ...
        on <CONDITION>
```

#### Example

The following query updates the events in the
`         FooTable        ` table with values from the latest events of
the `         FooStream        ` event stream. The events in the table
are updated only if the existing record in the table has the same value
as the new event for the `         symbol        ` attribute, and a
value greater than `         50        ` for the
`         price        ` attribute.

``` sql
    from FooStream
    select symbol, price, volume
    update FooTable 
    set FooTable.symbol = symbol, FooTable.price = price, FooTable.volume = volume 
    on (FooTable.symbol == symbol and price > 50)
```

#### Methods of Updating the columns in a table

This section gives further information on methods of updating the
columns in an existing table.

The value used for updating a table column can be any of the following:

-   A constant

    ``` sql
            FROM sensorStream
            SELECT sensorId, temperature, humidity
            UPDATE sensorTable
                SET sensorTable.column_temp = 1 
                ON  sensorId < sensorTable.column_ID
    ```

-   One of the attributes specified in the `           SELECT          `
    clause

    ``` sql
            FROM fooStream
            SELECT roomNo, time: timestampInMilliseconds() as ts
            UPDATE barTable
                SET barTable.timestamp = ts
                ON  barTable.room_no == roomNo AND roomNo > 2
    ```

-   A basic arithmetic operation applied on an output attribute

    ``` sql
            FROM sensorStream
            SELECT sensorId, temperature, humidity
            UPDATE sensorTable
                SET sensorTable.column_temp = temperature + 10  
                ON  sensorId < sensorTable.column_ID
    ```

-   A basic arithmetic operation applied to a column value in the event
    table

    ``` sql
            FROM sensorStream
            SELECT sensorId, temperature, humidity
            UPDATE sensorTable
                SET sensorTable.column_temp = sensorTable.column_temp + 10  
                ON  sensorId < sensorTable.column_ID
    ```

## Deleting Records

This section explains how to delete existing records in a table based on
a specific condition.

#### Prerequisites

In order to delete selected events in a table:

-   [General prerequisites](#ManagingStoredDataviaStreams-Ge) should be
    completed.
-   The table with the records to be deleted should be already
    defined. For more information, see [Defining a
    table](#ManagingStoredDataviaStreams-De) .
-   The event stream with the events with which the records in the table
    are compared (i.e., to apply the condition based on which the events
    are deleted) must be already defined.
-   One or more events should be inserted into the table. For more
    information, see [Inserting
    events](#ManagingStoredDataviaStreams-Insert) .

#### Query syntax

``` sql
    from <STREAM_NAME> 
    select <ATTRIBUTE1_NAME>, <ATTRIBUTE2_NAME>, ...
    delete <TABLE_NAME> (for <OUTPUT_EVENT_TYPE>)?
        on <CONDITION>
```

#### Example

This query deletes the events in the RoomTypeTable table if its value
for the roomNo attribute is equal to the roomNumber attribute value of
DeleteStream.

``` sql
    from DeleteStream
    delete RoomTypeTable
        on RoomTypeTable.roomNo == roomNumber;
```

## Searching records

This section explains how to check whether a specific record exists in
an event table.

#### Prerequisites

In order to search for a record in a table that matches a specific
condition:

-   [General prerequisites](#ConfiguringEventTablestoStoreData-Ge)
    should be completed.
-   The table to be searched should be already defined. For more
    information, see [Defining a
    table](#ConfiguringEventTablestoStoreData-De) .
-   The event stream with the events with which the records in the table
    are compared (i.e., to apply the condition based on which the events
    are searched) must be already defined.
-   One or more events should be inserted into the table. For more
    information, see [Inserting
    events](#ConfiguringEventTablestoStoreData-Insert) .

#### Query syntax

``` sql
    from <STREAM_NAME>[<CONDITION> in <TABLE_NAME>]
    select <ATTRIBUTE1_NAME>, <ATTRIBUTE2_NAME>, ...
    insert into <OUTPUT_STREAM_NAME>
```

#### Example

The following query matches events arriving from the
`         FooStream        ` event stream with the existing recored
stored in the `         StockTable        ` table. If
the symbol attribute of an event saved in the table has the same value
as the event from the `         FooStream        ` stream, that event is
inserted into the `         OutputStream        ` stream.

``` sql
    from FooStream[StockTable.symbol==symbol in StockTable]
    insert into OutputStream;
```

## Inserting/updating records

This section explains how to update a selection of records in a table
based on the new events from a specific event stream. The selection is
made based on a specific condition that matches events from the stream
with events in the table. When the events from the stream have no
matching events in the table, they are inserted into the table as new
events.

#### Prerequisites

-   [General prerequisites](#ConfiguringEventTablestoStoreData-Ge)
    should be completed.
-   The table for which this operations is to be performed must be
    already defined. For more information, see [Defining a
    table](#ConfiguringEventTablestoStoreData-De) .
-   The event stream from which the events with which the records in the
    table are compared (i.e., to apply the condition based on which the
    events are inserted/updated) must be already defined.

#### Query syntax

The query syntax to perform the insert/update operation for a table is
as follows.

``` sql
    from <STREAM_NAME> 
    select <ATTRIBUTE1_NAME>, <ATTRIBUTE2_NAME>, ...
    update or insert into <TABLE_NAME> (for <OUTPUT_EVENT_TYPE>)? 
        set <TABLE_NAME>.<ATTRIBUTE_NAME> = <EXPRESSION>, <TABLE_NAME>.<ATTRIBUTE_NAME> = <EXPRESSION>, ...
        on <CONDITION>
```

#### Example

This query matches events from the `         FooStream        ` stream
with the events stored in the `         StockTable        ` table. When
an event in the table has the same value for the
`         symbol        ` attribute as the matching new event from the
event stream, it is updated based on the new event. If a new event from
the event stream does not have a matching event in the table (i.e., an
event with the same value for the `         symbol        ` attribute),
that event is inserted as a new event.

``` sql
    from FooStream
    select *
    update or insert into StockTable
    on StockTable.symbol == symbol
```
