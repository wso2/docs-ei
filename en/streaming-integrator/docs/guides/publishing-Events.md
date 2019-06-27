# Publishing Events

Once data is analyzed by WSO2 SP, the resulting data is output via a
selected transport to the required interface in the required data
format. In order to output results, a Siddhi application that processes
events must have one or more sinks configured in it.

A sink configuration specifies the following:

-   [Source types](#PublishingEvents-Sourcetypes)
-   [Event format](#PublishingEvents-Eventformat)

### Source types

Each sink configuration must specify the transport type via which the
events to be published by the Siddhi appliction should be published. The
parameters to be configured for the transport differs based on the
transport types. The following is the list of transport types supported
by WSO2 SP. For detailed instructions to configure a sink of a specific
transport type, click on the relevant link.

-   [HTTP](https://wso2-extensions.github.io/siddhi-io-http/api/latest/#sink)
-   [Kafka](https://wso2-extensions.github.io/siddhi-io-kafka/api/latest/#sink)
-   [TCP](https://wso2-extensions.github.io/siddhi-io-tcp/api/latest/#tcp-sink)
-   [In-memory](https://siddhi-io.github.io/siddhi/api/latest/#inmemory-sink)
-   [WSO2Event](https://wso2-extensions.github.io/siddhi-io-wso2event/#siddhi-wso2event-sink)
-   [Email](https://wso2-extensions.github.io/siddhi-io-email/api/latest/#sink)
-   [JMS](https://wso2-extensions.github.io/siddhi-io-jms/api/latest/#sink)
-   [File](https://wso2-extensions.github.io/siddhi-io-file/api/latest/#file-sink)
-   [RabbitMQ](https://wso2-extensions.github.io/siddhi-io-rabbitmq/api/1.0.1-SNAPSHOT/#rabbitmq-sink)
-   [MQTT](https://wso2-extensions.github.io/siddhi-io-mqtt/api/latest/#sink)

!!! info

A sink could also be defined externally, and referred to from several
siddhi applications as described below. A \<PROFILE\> could refer to
dashboard, editor, manager or worker.

Multiple sinks can be defined in the
`         <SP HOME>/conf/<PROFILE>/deployment.yaml        ` file. The
following is a sample configuration of a sink.

``` java
    siddhi:
      refs:
        -
          ref:
            name: 'sink1'
            type: '<sink.type>'
            properties:
              <property1>: <value1>
              <property2>: <value2>
```
    
      
    
    You can refer to a source configured in the
    `          <SP HOME>/conf/<PROFILE>/deployment.yaml         ` file from
    a Siddhi application as shown in the example below.
    
      
    
``` java
    @Sink(ref='sink1', @map(type='json', @attributes( name='$.name', amount='$.quantity')))
    define stream SweetProductionStream (name string, amount double);
```
    
      
    
                    
    
                  
    

### Event format

The format in which the output events need to be published is specified
as the mapping type in the sink configuration. The parameters related to
mapping that need to be configured differ based on the mapping type. The
following is a list of supported mapping types. For detailed
instructions to configure each type, click on the relevant link.

-   [WSO2Event](https://wso2-extensions.github.io/siddhi-map-wso2event/api/latest/#wso2event-sink-mapper)
-   [XML](https://wso2-extensions.github.io/siddhi-map-xml/api/latest/#xml-sink-mapper)
-   [TEXT](https://wso2-extensions.github.io/siddhi-map-text/api/1.0.3-SNAPSHOT/#sinkmapper)
-   [JSON](https://wso2-extensions.github.io/siddhi-map-json/api/4.0.4-SNAPSHOT/#sinkmapper)
-   [Binary](https://wso2-extensions.github.io/siddhi-map-binary/api/latest/#sinkmapper)
-   [Key
    Value](https://wso2-extensions.github.io/siddhi-map-keyvalue/api/latest/#sinkmapper)
