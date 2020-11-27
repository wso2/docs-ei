# Receiving Prometheus Metrics

## Purpose:
This application demonstrates how  to retrieve Prometheus metrics that are exported at an HTTP endpoint via the `prometheus` source.

## Prerequisites:

The following steps must be executed to enable WSO2 Streaming Integrator to publish and retrieve events via Prometheus.

1. To complete the installation of the Prometheus extension in WSO2 Streaming Integrator Tooling, follow the steps below:

    1. Download the following JARs.
    
        - [simpleclient_common-0.5.0.jar](https://mvnrepository.com/artifact/io.prometheus/simpleclient_common/0.5.0)

        - [simpleclient-0.5.0.jar](https://mvnrepository.com/artifact/io.prometheus/simpleclient/0.5.0)
        
        - [simpleclient_httpserver-0.5.0.jar](https://mvnrepository.com/artifact/io.prometheus/simpleclient_httpserver/0.5.0)
        
        - [simpleclient_pushgateway-0.5.0.jar](https://mvnrepository.com/artifact/io.prometheus/simpleclient_pushgateway/0.5.0)
        
    2. Place the JARs you downloaded in the `<SI_TOOLING_HOME>/lib` directory.
		    
2. Start WSO2 Streaming Integrator Tooling, navigate to the `<SI_TOOLING_HOME>/bin` directory and issue the appropriate command based on your operating system.

    - **For Windows**: `server.bat --run`
    - **For Linux/MacOS**: `./server.sh`

3. Save the sample `EnergyAlertApp` Siddhi application.

    When the Siddhi application is successfully deployed in Streaming Integrator Tooling, the following message is logged in the console.

    `"Siddhi App EnergyAlertApp successfully deployed"`
    
4. Navigate to `<SI_TOOLING_HOME>/samples/sample-clients/prometheus-client` and issue the `ant` command as follows.

    `ant`

## Executing the Sample:

1. Start the `EnergyAlertApp` Siddhi application you saved by opening it and then clicking the **Start** button in the toolbar.

    When the Siddhi application is successfully started, the following messages are logged in the terminal.
    
    * `ReceivePrometheusMetrics.siddhi - Started Successfully!`
    * `PowerConsumptionStream has successfully connected at http://localhost:9080`

## Viewing the Results:

Messages similar to the following are logged in the terminal.
```
- INFO {io.siddhi.core.stream.output.sink.LogSink} - HIGH POWER CONSUMPTION : Event{timestamp=1*********, data=[server001, F3Room2, **, **], isExpired=false}
- INFO {io.siddhi.core.stream.output.sink.LogSink} - HIGH POWER CONSUMPTION : Event{timestamp=1*********, data=[server002, F2Room2, **, **], isExpired=false}
```

The complete sample Siddhi Application is as follows.

```sql
@App:name("EnergyAlertApp")
@App:description("Use siddhi-io-prometheus retrieve and analyse Prometheus metrics in Siddhi")


@source(type='prometheus' , target.url='http://localhost:9080/metrics',metric.type='counter', metric.name='total_device_power_consumption_WATTS', scrape.interval='5',
@map(type='keyvalue'))
define stream devicePowerStream(deviceID string, roomID string, value int);

@sink(type='log', priority='WARN', prefix ='High power consumption')
define stream AlertStream (deviceID string, roomID string, initialPower int, finalPower int);

@sink(type='log', priority='WARN', prefix ='Logging purpose')
define stream LogStream (deviceID string, roomID string, power int);

@info(name='power increase pattern')
from every( e1=devicePowerStream ) -> e2=devicePowerStream[ e1.deviceID == deviceID and (e1.value + 5) <= value]
    within 1 min
select e1.deviceID, e1.roomID, e1.value as initialPower, e2.value as finalPower
insert into AlertStream;
```