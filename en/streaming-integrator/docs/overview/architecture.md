# Architecture
Stream integration refers to collecting, processing and, integrate or acting on data generated during business activities by various sources. This description is paramount when designing a solution to address a streaming integration use case. 
 
- **Collecting**: Receiving or capturing data from various data sources. 
- **Processing**: Manipulation of data to identify interesting patterns and to extract information. 
- **Integrating**: Making processed data available for consumers to consume in an expected format via a given protocol or a medium. 
- **Acting**: Taking actions based on the results and findings done via processing the data. The action can be executing some random code, calling an external service, or triggering a complex integration. 
 
The WSO2 SI architecture reflects this natural flow in its design as illustrated below. 

![Streaming Integrator/ Architecture](../images/overview/si-architecture.png)


WSO2 SI contains Siddhi.io as its core to collect, nd aND 60+ connectors to connect with various sources and destinations. The following are the major components and constructs of the Streaming Integrator.
 
 ####Source
 The source is the construct that is used in Siddhi to receive data from an external source. A stream must be attached to the source so that the data received at the source is passed into the stream to be accessed in subsequent queries. A source handles all the transport-related functionalities when connecting to a data source.
 
 ####Source Mapper
 Data received by the source is passed into the source mappers and it maps incoming data to a format understandable by the Siddhi engine. 
 
 ####Siddhi
 Siddhi is a stream processing library written in Java. Users can write stream processing logic in the Siddhi Query Language (SiddhiQL). The Siddhi engine can run such queries against continuous streams of data. 
  
 ####Siddhi App
 A Siddhi application is a flat-file with siddhi extensions. It includes stream processing logic written in SiddhiQL. This is the deployable artifact in Siddhi which developers compile by writing stream processing logic. 
 
 ####SiddhiQL
 An SQL-like language that lets users write stream processing logic that can be read by the Siddhi engine. 
 
 ####Store API
 A REST API hosted in the SI server to let users fetch data stored in a persistent store or in-memory on-demand using ad-hoc siddhi queries.
 
 ####Sink
 The sink is a construct in Siddhi that publishes data to an external destination. You can attach a stream to a sink so that all the events flowing through this stream are published via the sink. The sink handles all transport-related functionality.
 
 #####Sink Mapper
 Event streams to be published in an external destination have to be mapped to a data format (e.g., JSON, XML) required by the destination. The sink mapper does this mapping.

 
 
 
