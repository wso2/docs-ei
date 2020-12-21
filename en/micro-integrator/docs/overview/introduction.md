# Introduction

**WSO2 Enterprise Integrator 7.1.0** is a powerful configuration-driven approach to integration, which allows developers to build integration solutions graphically. 

This is a hybrid platform that enables API-centric integration and supports various integration architecture styles: microservices architecture, cloud-native architecture, or a centralized ESB architecture. This integration platform offers a graphical/configuration-driven approach to developing integrations for any of the architectural styles.

## Centralized ESB

The heart of WSO2 EI 7.1 is the Micro Integrato server, which is an event-driven, standards-based messaging engine (the **Bus**). This ESB supports message routing, message transformations, and other types of messaging use cases. If your organization uses an API-driven, centralized, integration architecture, the Micro Integrator can be used as the central integration layer that implements the message mediation logic connecting all the systems, data, events, APIs, etc. in your integration ecosystem.

<img src="../../assets/img/intro/mi-esb-architecture.png" alt="centralized ESB" name="centralized ESB" width="600">

## Microservices 

The Micro Integrator of WSO2 EI is also lightweight and container friendly. This allows you to leverage the comprehensive enterprise messaging capabilities of the Micro Integrator in your decentralized, cloud-native integrations.

<img src="../../assets/img/intro/mi-microservices-architecture.png" alt="decentralized micro services" name="decentralized microservices" width="700">

As shown above, if your organization is running on a decentralized, cloud-native, integration architecture where microservices are used for integrating the various APIs, events, and systems, WSO2 Micro Integrator can easily function as your **Integration** (micro) services and **API** (micro) services.

## Low-code integration

The WSO2 Micro Integrator is coupled with [WSO2 Integration Studio](../../develop/WSO2-Integration-Studio); a comprehensive graphical integration flow designer for building integrations using a simple drag-and-drop functionality.

<img src="../../assets/img/intro/integration-studio.png" alt="Integration Studio" name="Integration Studio" width="700">

## Administration

The [Micro Integrator Dashboard](../../administer-and-observe/working-with-monitoring-dashboard) and [CLI](../../administer-and-observe/using-the-command-line-interface) are specifically designed for monitoring and administration of the Micro Integrator instances. Each of these tools are capable of binding to a single server instance by invoking the [management API](../../administer-and-observe/working-with-management-api) that is exposed by the server. This allow you to view and manage artifacts, logs/log configurations, and users of a server instance.

<img src="../../assets/img/intro/cli-dashboard.png" alt="CLI and dashboard" name="CLI and dashboard" width="700">

## Streaming integration

The [Streaming Integrator](https://ei.docs.wso2.com/en/latest/streaming-integrator/overview/overview/) of WSO2 EI 7.1 is a cloud-native, lightweight component that understands, captures, analyzes, processes, and acts upon streaming data and events in real-time. It utilizes the SQL-like query language ‘Siddhi’ to implement the solution.

<img src="../../assets/img/intro/streaming-integrator.png" alt="Streaming Integrator" name="Streaming Integrator" width="600">

The Streaming Integrator allows you to integrate static data sources with streaming data sources. Thus, it enables various types of applications (e.g., files, cloud based applications, data stores, streaming applications) to access streaming data and also exposes their output in a streaming manner. This is useful for performing ETL (Extract, Transform, Load) operations, capturing change data (i.e., CDC operations), and stream processing.