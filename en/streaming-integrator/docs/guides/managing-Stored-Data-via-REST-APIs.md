# Managing Stored Data via REST APIs

The actions such as inserting, searching, updating, retrieving, and
deleting records can be carried out by invoking the
`         POST/stores/query        ` REST API. These actions can be
performed for tables as well as windows and aggregations. The following
sections provide sample commands for each action.

-   [Retrieving
    records](#ManagingStoredDataviaRESTAPIs-Retrievingrecords)
-   [Updating records](#ManagingStoredDataviaRESTAPIs-Updatingrecords)
-   [Deleting records](#ManagingStoredDataviaRESTAPIs-Deletingrecords)
-   [Inserting/updating
    records](#ManagingStoredDataviaRESTAPIs-Inserting/updatingrecords)

  

!!! info

Configuring Store Query Endpoint

The Siddhi store query endpoint can be configured as follows in
worker/editor profiles:

1.  In the `            siddhi.stores.query.api:           ` section of
    the `            <SP_HOME>/conf/worker/deployment.yaml           `
    file, configure the following properties. The following is a sample
    configuration with default values.

    ``` xml
        siddhi.stores.query.api:
          transportProperties:
            -
              name: "server.bootstrap.socket.timeout"
              value: 60
            -
              name: "client.bootstrap.socket.timeout"
              value: 60
            -
              name: "latency.metrics.enabled"
              value: true
           listenerConfigurations:
            -
              id: "default"
              host: "0.0.0.0"
              port: 7070
            -
              id: "msf4j-https"
              host: "0.0.0.0"
              port: 7443
              scheme: https
              keyStoreFile: "${carbon.home}/resources/security/wso2carbon.jks"
              keyStorePassword: wso2carbon
              certPass: wso2carbon
    ```
    
          
    
        -   **`              transportProperties             `**
            -   `                               server.bootstrap.socket.timeout                              :              `
                The number of seconds after which the connection socket of
                the bootstrap server times out.
            -   **`                client.bootstrap.socket.timeout               `**
                : The number of seconds after which the connection socket of
                the bootstrap server times out.
            -   **`                latency.metrics.enabled               `**
                : If this is set to `               true              ` ,
                the latency metrics are enabled and logged for the HTTP
                transport.
        -   **`              listenerConfigurations             `** :
            Multiple listeners can be configured as shown in the above
            sample.
            -   **`                id               `** : A unique ID for
                the listener.
            -   **`                host               `** : The host of the
                listener.
            -   **`                port               `** : The port of the
                listener.
            -   **`                scheme               `** : This specifies
                whether the transport scheme is HTTP or HTTPS.
            -   **`                keyStoreFile               `** : If the
                transport scheme is HTTPS, this parameter specifies the path
                to the key store file.
            -   **`                keyStorePassword               `** : If
                the transport scheme is HTTPS, this parameter specifies the
                key store password.
    
    2.  In the `            siddhi.stores.query.api:           ` section of
        the `            <SP_HOME>/conf/editor/deployment.yaml           `
        file, the following properties are configured by default:
    
    ``` xml
            siddhi.stores.query.api:
              transportProperties:
                -
                  name: "server.bootstrap.socket.timeout"
                  value: 60
                -
                  name: "client.bootstrap.socket.timeout"
                  value: 60
                -
                  name: "latency.metrics.enabled"
                  value: true
               listenerConfigurations:
                -
                  id: "default"
                  host: "0.0.0.0"
                  port: 7370
    ```
        
            The same parameter descriptions provided for the
            `            <SP_HOME>/conf/worker/deployment.yaml           ` file
            apply to this configuration.
        

Inserting records

This allows you to insert a new record to the table with the attribute
values you define in the `         select        ` section.

#### Syntax

``` sql
    select <attribute name>, <attribute name>, ...
    insert into <table>;
```

#### Sample cURL command

The following cURL command submits a query that inserts a new record
with the specified attribute values to the table
`         RoomOccupancyTable        ` .

-   For worker:

    ``` java
            curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "RoomService", "query" : "select 10 as roomNo, 2 as people insert into RoomOccupancyTable;" }' -k
    ```

-   For editor:

    ``` java
            curl -X POST http://localhost:7370/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "RoomService", "query" : "select 10 as roomNo, 2 as people insert into RoomOccupancyTable;" }' -k
    ```

## Retrieving records

This store query retrieves one or more records that match a given
condition from a specified table/window/aggregator.

### Retrieving records from tables and windows

This is the store query to retrieve records from a table or a window.

#### Syntax

``` sql
    from <table/window>
    <on condition>? 
    select <attribute name>, <attribute name>, ...
    <group by>? 
    <having>? 
    <order by>? 
    <limit>?
```

#### Sample cURL command

The following cURL command submits a query that retrieves room numbers
and types of the rooms starting from room no 10, from a table named
`         roomTypeTable.        `

The `         roomTypeTable        ` table must be defined in the
`         RoomService        ` Siddhi application.

-   For worker:

    ``` java
            curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "RoomService", "query" : "from roomTypeTable on roomNo >= 10 select roomNo, type; " }' -k
    ```

-   For editor:

    ``` java
            curl -X POST http://localhost:7370/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "RoomService", "query" : "from roomTypeTable on roomNo >= 10 select roomNo, type; " }' -k
    ```

#### Sample response

The following is a sample response to the sample cURL command given
above.

``` java
    {"records":[
      [10,"single"],
      [11, "triple"],
      [12, "double"]
    ]}
```

### Retrieving records from aggregations

This is the store query to retrieve records from an aggregation.

#### Syntax

``` sql
    from <aggregation>  
      <on condition>?
      within <time range> 
      per <time granularity>
    select <attribute name>, <attribute name>, ...
      <groupby>?
      <having>?
      <order by>?
      <limit>?
```

#### Sample cURL command

The following cURL command submits a query that retrieves average price
of a stock `         .        `

-   For worker:

    ``` java
            curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "StockAggregationAnalysis", "query" : "from TradeAggregation on symbol=='FB' within '2018-**-** +05:00' per 'hours' select AGG_TIMESTAMP, symbol, total, avgPrice" }' -k
    ```

-   For editor:

    ``` java
            curl -X POST http://localhost:7370/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "StockAggregationAnalysis", "query" : "from TradeAggregation on symbol=='FB' within '2018-**-** +05:00' per 'hours' select AGG_TIMESTAMP, symbol, total, avgPrice" }' -k
    ```

#### Sample response

The following is a sample response to the sample cURL command given
above.

``` java
    {"records":[
      [1531180800, 'FB', 10000.0, 250.0],
      [1531184400, 'FB', 11000.0, 260.0],
      [1531188000, 'FB',  9000.0, 240.0]
    ]}
```

## Updating records

This store query updates selected attributes stored in a specific table
based on a given condition.

#### Syntax

``` sql
    select <attribute name>, <attribute name>, ...?
    update <table>
        set <table>.<attribute name> = (<attribute name>|<expression>)?, <table>.<attribute name> = (<attribute name>|<expression>)?, ...
        on <condition>
```

#### Sample cURL command

The following cURL command updates the room occupancy for selected
records in the table, `         RoomOccupancyTable        ` The records
that are updated are ones of which the roon number is greater than
`         10        ` . The room occupancy is updated by adding
`         1        ` to the existing value of the
`         people        ` attribute.

-   For worker:

    ``` java
            curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" -d '{"appName" : "RoomService", "query" : "select 10 as roomNumber, 1 as arrival update RoomTypeTable  set RoomTypeTable.people = RoomTypeTable.people + arrival on RoomTypeTable.roomNo == roomNumber;" }' -k
    ```

-   For editor:

    ``` java
            curl -X POST http://localhost:7370/stores/query -H "content-type: application/json" -u "admin:admin" -d '{"appName" : "RoomService", "query" : "select 10 as roomNumber, 1 as arrival update RoomTypeTable  set RoomTypeTable.people = RoomTypeTable.people + arrival on RoomTypeTable.roomNo == roomNumber;" }' -k
    ```

## Deleting records

This store query deletes selected records from a specified table.

#### Syntax

``` sql
    <select>?  
    delete <table>  
    on <conditional expresssion>
```

#### Sample cURL command

The following cURL command submits a query that deletes a record in the
table named `         RoomTypeTable        ` if it has value for the
`         roomNo        ` attribute that matches the value for the
`         roomNumber        ` attribute of the selection that has 10 as
the actual value.

-   For worker:

    ``` java
            curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "RoomService", "query" : "select 10 as roomNumber
            delete RoomTypeTable on RoomTypeTable.roomNo == roomNumber;;" }' -k
    ```

-   For editor:

    ``` java
            curl -X POST http://localhost:7370/stores/query -H "content-type: application/json" -u "admin:admin" 
            -d '{"appName" : "RoomService", "query" : "select 10 as roomNumber
            delete RoomTypeTable on RoomTypeTable.roomNo == roomNumber;;" }' -k
    ```

## Inserting/updating records

#### Syntax

``` sql
    select <attribute name>, <attribute name>, ...
    update or insert into <table>
        set <table>.<attribute name> = <expression>, <table>.<attribute name> = <expression>, ...
        on <condition>
```

#### Sample cURL command

The following cURL command submits a query that attempts to update
selected records in the `         RoomAssigneeTable        ` table. The
records that are selected to be updated are ones with room numbers that
match the numbers specified in the select clause. If matching records
are not found, it inserts a new record with the values provided in the
select clause.

-   For worker:

    ``` java
            curl -X POST https://localhost:7443/stores/query -H "content-type: application/json" -u "admin:admin" -d '{"appName" : "RoomService", "query" : "select 10 as roomNo, "single" as type, "abc" as assignee update or insert into RoomAssigneeTable  set RoomAssigneeTable.assignee = assignee  on RoomAssigneeTable.roomNo == roomNo;" }' -k
    ```

-   For editor:

    ``` java
            curl -X POST http://localhost:7370/stores/query -H "content-type: application/json" -u "admin:admin" -d '{"appName" : "RoomService", "query" : "select 10 as roomNo, "single" as type, "abc" as assignee update or insert into RoomAssigneeTable  set RoomAssigneeTable.assignee = assignee  on RoomAssigneeTable.roomNo == roomNo;" }' -k
    ```

  

!!! info

For more information and examples for store queries, see [Siddhi Query
Guide - Store
Query](http://siddhi-io.github.io/siddhi/documentation/siddhi-4.0/#store-query)
.

