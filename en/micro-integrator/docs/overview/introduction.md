# Introduction

**WSO2 Enterprise Integrator 7.1.0** is a powerful configuration-driven approach to integration, which allows developers to build integration solutions graphically. 

This is a hybrid platform that enables API-centric integration and supports various integration architecture styles: microservices architecture, cloud-native architecture, or a centralized ESB architecture. This integration platform offers a graphical/configuration-driven approach to developing integrations for any of the architectural styles.

## Centralized ESB

The heart of WSO2 Micro Integrator is an event-driven, standards-based messaging engine (the **Bus**), which supports message routing, message transformations, and other types of messaging use cases. If your organization uses an API-driven, centralized, integration architecture, WSO2 Micro Integrator can be used as the central integration layer that implements the message mediation logic connecting all the systems, data, events, APIs, etc. in your integration ecosystem.

<img src="../../assets/img/intro/mi-esb-architecture.png" alt="centralized ESB" name="centralized ESB" width="600">

## Microservices 

WSO2 Micro Integrator is also light-weight and container friendly. This allows you to leverage the comprehensive enterprise messaging capabilities of WSO2 Micro Integrator in your decentralized, cloud-native integrations. 

<img src="../../assets/img/intro/mi-microservices-architecture.png" alt="decentralized micro services" name="decentralized microservices" width="700">

As shown above, if your organization is running on a decentralized, cloud-native, integration architecture where micro services are used for integrating the various APIs, events, and systems, WSO2 Micro Integrator can easily function as your **Integration** (micro) services and **API** (micro) services.

## Low code integration

WSO2 Micro Integrator is coupled with [WSO2 Integration Studio](../../develop/WSO2-Integration-Studio); a comprehensive graphical integration flow designer for building integrations using a simple drag-and-drop functionality.

## Administration

WSO2 [Micro Integrator Dashboard](../../administer-and-observe/working-with-monitoring-dashboard) and [CLI](../../administer-and-observe/using-the-command-line-interface) are specifically designed for monitoring and administration of the Micro Integrator instances. Each of these tools are capable of binding 
to a single runtime at a given instance by invoking the [management API](../../administer-and-observe/working-with-management-api) that is exposed by the server to allow users to view and manage artifacts, logs/log configurations and users of a server instance.

## Streaming integration

The [Streaming Integrator](https://ei.docs.wso2.com/en/latest/streaming-integrator/overview/overview/) is a cloud-native, lightweight component that understands, captures, analyzes, processes, and acts upon streaming data and events in real-time. It utilizes the SQL-like query language ‘Siddhi’ to implement the solution.

The Streaming Integrator allows you to integrate static data sources with streaming data sources. Thus, it enables various types of applications (e.g., files, cloud based applications, data stores, streaming applications) to access streaming data and also exposes their output in a streaming manner. This is useful for performing ETL (Extract, Transform, Load) operations, capturing change data (i.e., CDC operations), and stream processing.