# Receiving Data in Transit

Data in transit, also known as data in flight refers to data that is in the process of being moved and therefore not permanently stored in a location where they can be in a static state. Streaming data, messages in a queue or a topic in a messaging system, and requests sent to a HTTP listening port are a few examples.

The sources from which data in transit/flight are received can be classified into two categories as follows:

- **Push data sources**: You can receive data from these sources without subscribing to receive it. (e.g., HTTP, HTTPS, TCP, email, etc.)

- **Pull data sources**: You need to subscribe to receive data from the source. (e.g., messaging systems such as Kafka, JMS, MQTT, etc.)

## Receiving data from push sources

To receive data from a push data source, define an input [stream](https://siddhi.io/en/v5.1/docs/query-guide/#stream) and connect a [source] annotation of a type that receives data from a push data source as shown in the example below.

```siddhi
@source(type='http', 
    receiver.url='http://localhost:5005/StudentRegistrationEP', 
    @map(type = 'json'))
define stream StudentRegistrationStream (name string, course string);
```
In this example, an online student registration results in an HTTP request in JSON format being sent to the endpoint named `StudentRegistrationEP` to the `5005` port of the localhost. The source generates an event in the `StudentRegistrationStream` stream for each of these requests.

### Try it out

To try out the example given above, let's include the source configuration in a Siddhi application and simulate an event to it.

1. Open and access Streaming Integrator Tooling. For instructions, see [Streaming Integrator Tooling Overview - Starting Streaming Integrator Tooling](../develop/streaming-integrator-studio-overview.md/#starting-streaming-integrator-tooling).

2. Open a new file and add the following Siddhi application to it.

    ```siddhi
    @App:name('StudentRegistrationApp')
     
    @source(type = 'http', receiver.url = "http://localhost:5005/StudentRegistrationEP",
        @map(type = 'json'))
    define stream StudentRegistrationStream (name string, course string);
    
    @sink(type = 'log', prefix = "New Student",
        @map(type = 'passThrough'))
    define stream StudentLogStream (name string, course string, total long);
    
     
    @info(name = 'TotalStudentsQuery')
    from StudentRegistrationStream 
    select name, course, count() as total 
    insert into StudentLogStream;
    ```

    Save the Siddhi application.
    
    This Siddhi application contains the `http` source of the previously used example. The `TotalStudentsQuery` query selects all the student registrations captured as HTTP requests and directs them to the `StudentLogStream` output stream. A log sink connected to this output stream logs these registrations in the terminal. Before logging the events the Siddhi application also counts the number of registrations via the `count()` function. This count is presented as `total` in the logs.
    
3. Start the Siddhi application by clicking on the play icon in the top panel.

    ![Play](../images/extracting-data-from-static-sources/play.png)
    
4. To simulate an event, issue the following two CURL commands.

    ```text    
    curl -X POST \
      http://localhost:5005/StudentRegistrationEP \
      -H 'content-type: application/json' \
      -d '{
      "event": {
        "name": "John Doe",
        "course": "Graphic Design"
      }
    }'
    ```
   
    ```text    
    curl -X POST \
     http://localhost:5005/StudentRegistrationEP \
     -H 'content-type: application/json' \
     -d '{
     "event": {
       "name": "Michelle Cole",
       "course": "Graphic Design"
     }
    }'
    ```
   
   The following is logged in the terminal.
   
    ```text
    INFO {io.siddhi.core.stream.output.sink.LogSink} - New Student : Event{timestamp=1603185021250, data=[John Doe, Graphic Design, 1], isExpired=false}
    ```
   
    ```text
    INFO {io.siddhi.core.stream.output.sink.LogSink} - New Student : Event{timestamp=1603185486763, data=[Michelle Cole, Graphic Design, 2], isExpired=false}
    ```
    
### Supporting Siddhi extensions

The following are Siddhi extensions that allow you to capture data in transit/flight.

- [HTTP](https://siddhi-io.github.io/siddhi-io-http/api/latest/#source)
- [TCP](https://siddhi-io.github.io/siddhi-io-tcp/api/latest/#source)
- [email](https://siddhi-io.github.io/siddhi-io-email/api/latest/#email-source)

## Receiving data from pull sources

### Supporting Siddhi extensions