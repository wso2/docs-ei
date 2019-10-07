
# Introduction

WSO2 Streaming Integrator(SI) is a streaming data processing server that allows you to integrate streaming data and take action based on streaming data. This runtime intends to deliver the streaming integration capabilties of EI

WSO2 SI can be effectively used for:

- **Realtime ETL**: CDC for DBs, tailing files, scraping HTTP Endpoints, etc.
- **Work with streaming messaging systems**: It is fully compatible with Kafka and NATS and provides advanced stream processing capabilities required to utlize the full potential of streaming data.
- **Streaming data Integration**: Allows you to treat all data sources as streams and connect such streams with any destination.
- **Execute complex integrations based on streaming data**: SI has native support to work hand-in-hand with WSO2 Micro integrator, so that complex integration flows can be triggered based on decisions derived via stateful stream processing logic.

##Key Features
WSO2 SI is powered by [Siddhi.io](https://siddhi.io/), a well known cloud native open source stream processing engine. Siddhi allows you to write complex stream processing logic using an intuitive SQL-like language known as [SiddhiQL](https://siddhi.io/en/v5.0/docs/). You can perform the following actions on the fly using Siddhi queries and constructs.


- **Transforming** your data from one format to another (e.g., to/from XML, JSON, AVRO, etc.).
- **Enriching** data received from a specific source by combining it with databases, and services, via inline calculations as well as using custom functions.
- **Correlating** data streams by joining multiple streams to create an aggregate stream.
- **Cleaning** data by filtering it and by modifying the content (e.g., obfuscating) in messages.
- Deriving **insights** by identifying interesting patterns and sequences of events in data streams.
- **Summarizing** data as and when it is generated using temporal windows and incremental time series aggregations.
 
 ![Streaming Integrator/ Workflow](../images/overview/streaming-integrator.png)

WSO2 SI allows you to connect to any data source with any destination regardless of the different protocols and data formats used by the different endpoints. WSO2 SI has 60+ prebuilt and well tested collection of connectors that can be used to connect to various sources and destinations. 

The SI Store API can expose aggregated and collected data streams to in-memory and persistence storages via a REST API. Via the store API, you can execute queries and generate summarized information on demand via ad-hoc queries.

Synapse integration flows deployed in [WSO2 Micro Integration(MI)](https://github.com/wso2/micro-integrator) can be executed directly by SI. This allows you to build robust data processing and integration pipelines by combining powerful stream processing and integration capabilities.

##Tooling
The SI tooling provides a web-based IDE that has all required funtionalities to develop, test and deploy siddhi apps 

- **Develop:** SI tooling has drag-and-drop graphical editor which lets business users build complex data processing pipelines without writing any quaries, or a more technical user can use the streaming SQL editor which has intelisense, syntax coloring, syntax coloring etc. 
- **Test:** Event stream simulation, stream replay and Siddhi query debuging features in SI tooling lets users test their funtionality throughly before deploying them in production.   
- **Deploy:** Furthermore, you can directly deploy the Siddhi applications on a SI server through from the tooling, or export the siddhi app to be deployed K8s or as a Docker image.

##Centralized and Decentralized Deployment
WSO2 SI can be deployed in VM, Docker or K8s easily. It is container-friendly by design with a small image size, low resource footprint, a startup time less than two seconds, etc.

SI has native support for Kubernetes with a [K8s Operator](https://siddhi.io/en/v5.1/docs/siddhi-as-a-kubernetes-microservice/) designed to provide a convenient way of deploying SI on a K8s cluster with a single command by eliminating the need for manual configurations. 
 
The deployment architecture of the SI lets users achieve high availability with zero data loss with just two nodes of SI.

