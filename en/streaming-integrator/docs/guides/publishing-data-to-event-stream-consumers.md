# Publishing Data

This guide covers how WSO2 Streaming Integrator publishes data to destinations and messaging systems.

## Publishing data to destinations

Publishing to destinations involve using transports such as HTTP, TCP, email, etc., where the data is sent to an endpoint that is available to listen to messages and respond. 

![Publishing to destinations](../images/publishing-data/publishing-to-destination.png)

To understand this, consider a warehouse that needs to publish each stock update to a specific endpoint so that the stock can be monitored by the warehouse manager. To address this requirement via the WSO2 Streaming Integrator, you can define an output stream and connect a sink to it as shown below. In this example, let's use an HTTP sink.

```
@sink(type = 'http', publisher.url = 'http://stocks.com/stocks',
      @map(type = 'json'))
define stream StockStream (symbol string, price float, volume long);
```

The above sink configuration publishes all the events in the `StockStream` output stream to the `http://stocks.com/stocks` HTTP URL in JSON format.

### Try it out

To try out the above example, follow the steps below:

1. [Start and access WSO2 Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).
   
2. Open a new file and copy the following Siddhi Application to it.

    ```
    @App:name("PublishStockUpdatesApp")  
   
    define stream InputStream (symbol string, price float, volume long);
    
    @sink(type = 'http', publisher.url = 'http://localhost:5005/stocks',
          @map(type = 'json'))
    define stream StockStream (symbol string, price float, volume long);
    
    from InputStream
    select *
    insert into StockStream;
    ```
   
   Save the Siddhi application.
   
   This Siddhi application publishes stock updates as HTTP events via the `http` sink in the previous example.
   
3. To monitor whether the HTTP events generated via the `PublishStockUpdatesApp` Siddhi application are getting published to the `http://localhost:5005/stocks` URL as specified, create and save another Siddhi Application as follows:

    ```
    @App:name('ListenToStockUpdatesApp')
    
    @source(type = 'http', receiver.url = "http://localhost:5005/stocks",
    	@map(type = 'json'))
    define stream StockStream (symbol string, price float, volume long);
    
    @sink(type = 'log', prefix = "Stock Updates",
    	@map(type = 'passThrough'))
    define stream OutputStream (symbol string, price float, volume long);
    
    @info(name = 'query1')
    from StockStream 
    select * 
    insert into OutputStream;
    ```
   
    This Siddhi application listens for events in the `http://localhost:5005/stocks` endpoint and logs them in the Streaming Integrator Tooling console.
    
4. Start both the Siddhi applications. To do this, opoen each siddhi application and click the **Play** icon.

    ![Play](../images/extracting-data-from-static-sources/play.png)
    
5. Simulate an event with the following values to the `InputStream` stream of the `PublishStockUpdatesApp` Siddhi application. For instructions to simulate events, see [Testing Siddhi Applications - Simulating Events](../develop/testing-a-Siddhi-Application.md)



## Publishing data to messaging systems

WSO2 Streaming Integrator allows you to publish data to messaging systems such as Kafka, JMS, NATS, GoolePbSub, etc. so that you can expose streaming data to applications that cannot read streaming data, but are able to subscribe for data in messaging systems.

![Publishing to messaging systems](../images/publishing-data/publishing-to-message-broker.png)


