# Collecting Events

The first step in stream processing is to collect the data that needs to
be analyzed. Collection of data is done through Siddhi source which can
be defined via  @source() annotation on a steam.

To collect data to be processed a stream should have been defined in the
Siddhi Application along with the @source() annotation and deployed in
WSO2 SP. Here a single SiddhiApp can contain multiple streams and each
of those steams can also contain multiple @source() annotations.

**Example Input Source definition as bellow.**

``` sql
    @Source(type = 'http',        
            receiver.url='http://localhost:8006/productionStream',
            basic.auth.enabled='false',
            @map(type='json', @attributes( name='$.name', amount='$.quantity')))
    define stream SweetProductionStream (name string, amount double);
```

  
The source defines the following :

-   Input source type: via `          type = 'http'         `
-   Source type configurations (This is optional. I n this example h ttp
    source type configurations are defined via
    `          receiver.url='http://localhost:8006/productionStream', basic.auth.enabled='false')         `
-   Input message format: with `          @map()         `
    sub-annotation.
-   Custom attribute mapping of the input message format (optional):
    with optional
    `          @attributes( name='$.name', amount='$.quantity')         `
    sub-annotation of `          @map.         `  
    `                   `

!!! info

A source could also be defined externally, and referred to from several
siddhi applications as described below,Multiple sources can be defined
in the `         <SP HOME>/conf/<PROFILE>/deployment.yaml        ` file.
A \<PROFILE\> could refer to dashboard, editor, manager or worker. The
following is a sample configuration of a source.

``` xml
    siddhi:
    
      refs:
        -
          ref:
            name: 'source1'
            type: '<store.type>'
            properties:
              <property1>: <value1>
              <property2>: <value2>
```
    
    You can refer to a source configured in the
    `         <SP HOME>/conf/<PROFILE>/deployment.yaml        ` file from a
    Siddhi application as shown in the example below.
    
      
    
``` xml
    @Source(ref='source1', basic.auth.enabled='false',
            @map(type='json', @attributes( name='$.name', amount='$.quantity')))
    define stream SweetProductionStream (name string, amount double);
```
    
    For detailed instructions to configure a source, see [Siddhi Guide -
    Source](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#source)
    .
    

  

### Source types

WSO2 SP supports following source types out of the box, to receive
events via corresponding transports. Click on the required source type
for instructions to configure a source to receive events via them.

-   [HTTP](https://wso2-extensions.github.io/siddhi-io-http/api/latest/#source)
-   [Kafka](https://wso2-extensions.github.io/siddhi-io-kafka/api/latest/#kafka-source)
-   [TCP](https://wso2-extensions.github.io/siddhi-io-tcp/api/latest/#source)
-   [In-memory](https://siddhi-io.github.io/siddhi/api/latest/#inmemory-source)
-   [WSO2
    Event](https://wso2-extensions.github.io/siddhi-io-wso2event/#siddhi-wso2event-source)
-   [Email](https://wso2-extensions.github.io/siddhi-io-email/api/latest/#source)
-   [JMS](https://wso2-extensions.github.io/siddhi-io-jms/api/latest/#source)
-   [File](https://wso2-extensions.github.io/siddhi-io-file/api/latest/#source)
-   [RabbitMQ](https://wso2-extensions.github.io/siddhi-io-rabbitmq/api/1.0.1-SNAPSHOT/#source)
-   [MQTT](https://wso2-extensions.github.io/siddhi-io-mqtt/api/latest/#source)

  

### Event format

WSO2 Siddhi allows events to be received in multiple formats. The
following formats are currently supported. Once an event is received in
a specific format, it is internally converted to a Siddhi event so that
it can be processed by applying the WSO2 Siddhi logic. Click on the
required format for detailed information on how a source can be
configured to receive events in that format.

-   [WSO2Event](https://wso2-extensions.github.io/siddhi-map-wso2event/api/latest/#sourcemapper)
-   [XML](https://wso2-extensions.github.io/siddhi-map-xml/api/latest/#sourcemapper)
-   [Text](https://wso2-extensions.github.io/siddhi-map-text/api/1.0.3-SNAPSHOT/#sourcemapper)
-   [JSON](https://wso2-extensions.github.io/siddhi-map-json/api/4.0.4-SNAPSHOT/#sourcemapper)
-   [Binary](https://wso2-extensions.github.io/siddhi-map-binary/api/latest/#sourcemapper)
-   [Key
    Value](https://wso2-extensions.github.io/siddhi-map-keyvalue/api/latest/#sourcemapper)

  

  
