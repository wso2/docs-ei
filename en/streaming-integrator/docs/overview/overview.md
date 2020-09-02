
# Introduction

WSO2 Streaming Integrator(SI) is a streaming data processing server that integrates streaming data and takes action based on streaming data. The streaming integration capabilities of EI are delivered via this runtime

WSO2 SI can be effectively used for:

- **Realtime ETL**: CDC for DBs, tailing files, scraping HTTP Endpoints, etc.
- **Work with streaming messaging systems**: It is fully compatible with Kafka and NATS, and provides advanced stream processing capabilities required to utilize the full potential of streaming data.
- **Streaming data Integration**: Allows you to treat all data sources as streams and connect them with any destination.
- **Execute complex integrations based on streaming data**: SI has native support to work hand-in-hand with WSO2 Micro integrator to trigger complex integration flows based on decisions derived via stateful stream processing logic.


!!! info "Try it out!"
    To try out each of the above use cases with SI, see [Tutorials](../examples/tutorials-overview.md)

## Key Features

WSO2 SI is powered by [Siddhi.io](https://siddhi.io/), a well known cloud native open source stream processing engine. Siddhi allows you to write complex stream processing logic using an intuitive SQL-like language known as [SiddhiQL](https://siddhi.io/en/v5.0/docs/). You can perform the following actions on the fly using Siddhi queries and constructs.


- [**Transforming**](../guides/transforming-data.md) your data from one format to another (e.g., to/from XML, JSON, AVRO, etc.).
- [**Enriching**](../guides/enriching-data.md) data received from a specific source by combining it with databases, services, etc., via inline calculations and custom functions.
- [**Correlating**](../guides/correlating-events.md#correlate-a-stream-and-a-static-datasource-to-enrich.md) data streams by joining multiple streams to create an aggregate stream.
- [**Cleaning**](../guides/cleansing-data.md) data by filtering it and by modifying the content (e.g., obfuscating) in messages.
- [Deriving **insights**](../guides/correlating-events.md#correlating-events-to-find-a-pattern.md) by identifying interesting patterns and sequences of events in data streams.
- [**Summarizing**](../guides/summarizing-data.md) data as and when it is generated using temporal windows and incremental time series aggregations.
 
 ![Streaming Integrator/ Workflow](../images/overview/streaming-integrator.png)

With 60+ prebuilt and well tested collection of connectors, WSO2 SI allows you to connect any data source with any destination regardless of the different protocols and data formats used by the different endpoints.

The SI Store API exposes aggregated and collected data streams to in-memory and persistence storages via a REST API, allowing you to execute queries and generate summarized information on demand.

Synapse integration flows deployed in [WSO2 Micro Integration(MI)](https://github.com/wso2/micro-integrator) can be executed directly by SI. This allows you to build robust data processing and integration pipelines by combining powerful stream processing and integration capabilities.

## Tooling

WSO2 SI is coupled with the [Streaming Integrator Tooling](../develop//streaming-integrator-studio-overview.md); a comprehensive streaming integration flow designer for [**developing**](../develop/creating-a-Siddhi-Application.md)) Siddhi applications
 by writing queries or via the drag-and-drop functionality, [**testing**](../develop/testing-a-Siddhi-Application.md) them thoroughly before using them in production, and then [**deploying**](../develop/deploying-Streaming-Applications.md) them in the SI server or [exporting them to be deployed as a K8 or a Docker image](../develop/exporting-Siddhi-Applications.md).



## Centralized and Decentralized Deployment

Being container-friendly by design with a small image size, low resource footprint, a startup time less than two seconds, etc., WSO2 SI can be easily deployed in VMs, Docker or K8s.

Its native support for Kubernetes with a [K8s Operator](https://siddhi.io/en/v5.1/docs/siddhi-as-a-kubernetes-microservice/) provides a convenient way to deploy SI on a K8s cluster with a single command, thereby eliminating the need for manual configurations.

Deploying SI as a highly available [minimum HA cluster](../setup/deploying-si-as-minimum-ha-cluster.md) allows you to achieve zero data loss with just two SI nodes.

!!! tip "What's Next"
    - [Quick Start Guide](../quick-start-guide/quick-start-guide.md)<br/>
    - [Create Your First Siddhi Application](../quick-start-guide/getting-started/getting-started-guide-overview.md)

