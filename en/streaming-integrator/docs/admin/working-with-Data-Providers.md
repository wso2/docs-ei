# Working with Data Providers

Data providers are the sources from which information is fetched to be
displayed in widgets. This section describes how to configure the data
providers that are currently supported in WSO2 Stream Processor are as
follows. These configurations determine the parameters that are
available to be configured for each data provider when creating widgets
via the Widget Generation wizard. For more information, see [Generating
Widgets](https://docs.wso2.com/display/SP440/Generating+Widgets) .

-   [RDBMS Batch Data
    Provider](#WorkingwithDataProviders-RDBMSBatchDataProvider)
-   [RDBMS Streaming Data
    Provider](#WorkingwithDataProviders-RDBMSStreamingDataProvider)
-   [Siddhi Store Data
    Provider](#WorkingwithDataProviders-SiddhiStoreDataProvider)
-   [Web Socket Provider](#WorkingwithDataProviders-WebSocketProvider)

## RDBMS Batch Data Provider

This data provider queries static tables. The following configuration is
an example of an RDBMS Batch Data Provider:

``` java
    "config": {
              "datasourceName": "Twitter_Analytics",
              "queryData": {
                "query": "select type as Sentiment, count(TweetID) as Rate from sentiment where PARSEDATETIME(timestamp, 'yyyy-mm-dd hh:mm:ss','en') > CURRENT_TIMESTAMP()-86400 group by type"
              },
              "tableName": "sentiment",
              "incrementalColumn": "Sentiment",
              "publishingInterval": 60
    }
```

## RDBMS Streaming Data Provider

This data provider queries dynamic tables. Here, the newer records are
published as soon as the table is updated. The following configuration
is an example of an RDBMS Streaming Data Provider:

``` java
    "configs": {
            "type": "RDBMSStreamingDataProvider",
            "config": {
              "datasourceName": "Twitter_Analytics",
              "queryData": {
                "query": "select id,TweetID from sentiment"
              },
              "tableName": "sentiment",
              "incrementalColumn": "id",
              "publishingInterval": 5,
              "publishingLimit": 5,
              "purgingInterval": 6,
              "purgingLimit": 6,
              "isPurgingEnable": false
            }
          }
```

## Siddhi Store Data Provider

This data provider runs a siddhi store query. The following
configuration is an example of a Siddhi Store Data Provider:  

!!! info

The Siddhi application included in this query must not contain any
source configurations.


``` java
    "configs":{
                    "type":"SiddhiStoreDataProvider",
                    "config":{
                        "siddhiApp":"@App:name(\"HTTPAnalytics\") define stream ProcessedRequestsStream(timestamp long, serverName string, serviceName string, serviceMethod string, responseTime double, httpRespGroup string, userAgent string, requestIP string); define aggregation RequestAggregation from ProcessedRequestsStream select serverName, serviceName, serviceMethod, httpRespGroup, count() as numRequests, avg(responseTime) as avgRespTime group by serverName, serviceName, serviceMethod, httpRespGroup aggregate by timestamp every sec...year;",
                        "queryData":{
                            "query":"from RequestAggregation within \"2018-**-** **:**:**\" per \"days\" select AGG_TIMESTAMP, serverName, avg(avgRespTime) as avgRespTime"
                        },
                        "publishingInterval":60,
                        "timeColumns": "AGG_TIMESTAMP"
                    }
                }
```

  

## Web Socket Provider

This data provider utilizes web siddhi-io-web socket sink to provide
data to the clients. It creates endpoints as follows for the web socket
sinks to connect and publish information.

``` java
      wss://host:port/websocket-provider/{topic}
```

!!! info

The host and port will be the host and port of the Portal Web
application


  

The following configuration is an example of a web socket data provider.

``` java
    {
        configs: {
            type: 'WebSocketProvider', 
            config: {
                subscriberTopic: 'sampleStream', 
                mapType: 'json' 
            }
        }
    }
```
