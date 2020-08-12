# Introduction

The **Micro Integrator** of **WSO2 Enterprise Integrator 7.1.0** is a powerful configuration-driven approach to integration, which allows developers to build integration solutions graphically. 

## Building centralized, monolithic integrations

The heart of WSO2 Micro Integrator is an event-driven, standards-based messaging engine (the **Bus**), which supports message routing, message transformations, and other types of messaging use cases. If your organization uses an API-driven, centralized, integration architecture, WSO2 Micro Integrator can be used as the central integration layer that implements the message mediation logic connecting all the systems, data, events, APIs, etc. in your integration ecosystem.

![Centralized Integration](../assets/img/intro/centralized-integration.png)

## Building decentralized, cloud-native integrations

WSO2 Micro Integrator is also light-weight and container friendly. This allows you to leverage the comprehensive enterprise messaging capabilities of WSO2 Micro Integrator in your decentralized, cloud-native integrations. 

![Centralized Integration](../assets/img/intro/cloud-native-microservices.png)

As shown above, if your organization is running on a decentralized, cloud-native, integration architecture where micro services are used for integrating the various APIs, events, and systems, WSO2 Micro Integrator can easily function as your **Integration** (micro) services and **API** (micro) services.

## Graphical integration desigining

WSO2 Micro Integrator is coupled with [WSO2 Integration Studio](../../develop/WSO2-Integration-Studio); a comprehensive graphical integration flow designer for building integrations using a simple drag-and-drop functionality.

## Administration

WSO2 [Micro Integrator Dashboard](../../administer-and-observe/working-with-monitoring-dashboard) and [CLI](../../administer-and-observe/using-the-command-line-interface) are specifically designed for monitoring and administration of the Micro Integrator instances. Each of these tools are capable of binding 
to a single runtime at a given instance by invoking the [management API](../../administer-and-observe/working-with-management-api) that is exposed by the server to allow users to view and manage artifacts, logs/log configurations and users of a server instance.

## Streaming integration

The [Streaming Integrator](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/overview/overview/) of WSO2 Enterprise Integrator allows you to integrate static data sources with streaming data sources. Thus, it enables various types of applications (e.g., files, cloud based applications, data stores, streaming applications) to access streaming data and also exposes their output in a streaming manner. This is useful for performing ETL (Extract, Transform, Load) operations, capturing change data (i.e., CDC operations), and stream processing.