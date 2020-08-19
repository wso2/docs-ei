# Handling Errors

WSO2 Streaming Integrator allows you to handle any errors that may occur when handling streaming data in a graceful manner. This section explains the different types of errors that can occur and how they can be handled.

## Supported on-error actions

The supported error actions are as follows:

### STORE

This stores the events with errors in the error store. 

Before using this on-error action, you need to enable the error store in the `<SI_HOME>/conf/server/deployment.yaml` file by adding the following configuration.

```
error.store:
  enabled: true
  bufferSize: 1024
  dropWhenBufferFull: true
  errorStore: org.wso2.carbon.streaming.integrator.core.siddhi.error.handler.DBErrorStore
  config:
    datasource: ERROR_STORE_DB
    table: ERROR_STORE_TABLE
```
- `bufferSize` denotes the size of the ring buffer that is used in the disruptor when publishing events to the ErrorStore. This has to be a power of two. If not, it throws an exception during initialization. The default buffer size is `1024`.
- If the `dropWhenBufferFull` is set to `true`, the event is dropped when the capacity of the ring buffer is insufficient.

This can be used with the following.
1. With Siddhi Streams
    ```
    @OnError(action='STORE')
    define stream StreamA (symbol string, amount double);
    
    from StreamA[cast("abc", "double") > 100]
    insert into StreamB;
    ```
    The Siddhi query uses the `cast("abc", "double")` which intentionally generates an error for testing purpose.
    if no @OnError() is specified, the event will be logged and dropped.
    
2. With Sinks
    ```
    @sink(type = 'http', on.error='STORE', blocking.io='true', 
          publisher.url = "http://localhost:8090/unavailableEndpoint", 
          method = "POST", @map(type = 'json'))
    define stream StreamA (name string, volume long);
    ```

3. With Source Mappers
   If the `error.store` is enabled in the deployment.yaml, this will be automatically added to the error store.

### LOG

This logs the event with details of the error and then drops the event.
This can be used with the following.
1. With Siddhi Streams
    ```
    @OnError(action='LOG')
    define stream StreamA (symbol string, volume long);
    ```
    if no @OnError() is specified, the event will be logged and dropped.
2. With Sinks
    ```
    @sink(type = 'http', on.error='LOG', blocking.io='true', 
          publisher.url = "http://localhost:8090/unavailableEndpoint", 
          method = "POST", @map(type = 'json'))
    define stream TestPublisherStream (symbol string, volume long);
    ```
   if on.error parameter is not specified, the event will be logged and dropped.
3. With Source Mappers, when Error Store is not enabled in the deployment.yaml
   
### STREAM

Here, if an error occurs for the base stream named `StreamA` , a stream named `!StreamA` is automatically created. The base stream has two attributes named symbol and volume. Therefore, `!StreamA` has the same two attributes, and in addition, another attribute named `_error`.

This can be used with the following.
1. With Siddhi Streams
    ```
    @OnError(action='STREAM')
    define stream StreamA (symbol string, amount double);
    
    from StreamA[cast("abc", "double") > 100]
    insert into StreamB;
    
    -- consumes from the fault stream
    from !StreamA#log("Error Occured")
    select symbol, amount, _error
    insert into tempStream;
    ```
    The Siddhi query uses the `cast("abc", "double")` which intentionally generates an error for testing purpose.
    if no @OnError() is specified, the event will be logged and dropped.
    
2. With Sinks
    ```
    @OnError(action='STREAM')
    @sink(type = 'http', on.error='STREAM', blocking.io='true', 
          publisher.url = "http://localhost:8090/unavailableEndpoint", 
          method = "POST", @map(type = 'json'))
    define stream StreamA (name string, volume long);
   
    -- consumes from the fault stream
    from !StreamA#log("Error Occured")
    select symbol, volume, _error
    insert into tempStream;
    ```

!!! Note
    This is not applicable with Source Mappers

### WAIT

This on-error type is only applicable to publishing errors. Here, the thread waits in the `back-off and re-trying` state, and reconnects once the connection is re-established.
This can be only used with Sinks
```
@sink(type = 'http', on.error='WAIT', blocking.io='true', 
      publisher.url = "http://localhost:8090/unavailableEndpoint", 
      method = "POST", @map(type = 'json'))
define stream StreamA (name string, volume long);
```