
# Overview of WSO2 Streaming Integrator


Overview

WSO2 Streaming Integrator(SI) is a streaming data processing sever which lets users integrate streaming data and take action based on streaming data. This runtime intends to deliver the streaming integration capabilties of EI

WSO2 SI can be effectively used for 
+ **Realtime ETL**: CDC for DBs, tailing files, scraping HTTP Endpoints, etc.
+ **Work with streaming messaging systems**: fully compatible with Kafka and NATS.
+ **Streaming data Integration**: Let’s users treat all data sources as streams and connect such streams with any destination. 
+ **Execute complex integrations based on streaming data**: SI has native support work hand-in-hand with WSO2 Micro integrator, so that complex integration flows can be triggered based on decision derived with stateful stream processing logics


It’s powered by [Siddhi.io](https://siddhi.io/), a well known cloud native open source stream processing engine. Siddhi lets users write complex stream processing logics using a intuitive SQL-like language called [SiddhiQL](https://siddhi.io/en/v5.0/docs/). Users can perform the following actions on the fly using Siddhi queries and constructs. 
 + **Transforming** ther data from one format to another (e.g., to/from XML, JSON, AVRO, etc.)
 + **Enriching** data received from a specific source by combining it with databases, services, and via inline calculations and using custom funtions
 + **Corelating** data streams by joining multiple streams to create an aggregate stream.
 + **Cleanin**g data by filtering it and by modifying the content(e.g., obfuscation) in messages.
 + Deriving **insights** by identifying interesting patterns and sequences of events in data streams.
 + **Summarizing** data as an when it is generated using temporal windows and incremental time series aggregations.
 
 ![Streaming Integrator/ Workflow](docs/images/streaming-integrator.png)

WSO2 SI lets users connect any data source with any destination regardless of the different protocols, data formats that different endpoints use. WSO2 SI has 60+ prebuilt, well tested collection of connectors that can be used to connect to various sources and destinations. SI Store API can be used to expose aggregated and collected data streams in memory and into persistence storages via a REST API, using store API users can fetch row data as well execute queries and generate summarized information on demand with ad-hoc quarries.

The [SI tooling](https://github.com/wso2/streaming-integrator-tooling) provides a web-based IDE which allows users to build Siddhi apps with a drag-and-drop graphical editor or a streaming SQL editor with intellisense, syntax coloring etc. It also has the capability to simulate and replay data streams and debug siddhi queries to allow users to test their Siddhi apps prior to deployment. Furthermore, the Siddhi apps can be directly deployed on a SI server though the IDE or exports as K8s artifacts or as a Docker image.

WSO2 SI can be deployed in VM, Docker or K8s easily. It’s container friendly by design with small images size, low resource footprint, <2s startup time, etc. SI has native support for Kubernetes with a [K8s Operator] (https://siddhi.io/en/v5.1/docs/siddhi-as-a-kubernetes-microservice/) designed to provide a convenient way of deploying SI on a K8s cluster with a single command by eliminating need of manual configs. Users can achieve high availability with zero data loss with two nodes of SI.

Synapse integration flows deployed in [WSO2 Micro Integration(MI)] (https://github.com/wso2/micro-integrator) can be executed directly by SI, this allows users to build robust data processing and integration pipelines by combining powerful stream processing and integration capabilities. 
 

 
