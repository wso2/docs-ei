---
title: HTTP Request-Response with Logger Example
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example application demonstrates how to use WSO2 EI to build a simple HTTP request-response application. This example was designed to demonstrate the ability of a WSO2 Integration applications to interact with HTTP request and read 
parameters from the URL. 

### Assumptions

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical developer tool. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

## Example Use Case

In this example, a user calls the EI application by submitting a request. (i.e., entering a specific URL: http://localhost:8084/echo). 

The application receives the request, sets the request path as the payload, and returns the payload to the end user. 

In other words, when a user types `http://localhost:8084/echo` in address bar of a browser, application returns with `/echo` and if the user changes the url to `http://localhost:8084/moon`, application responds with `/moon`.

There are two functions that the HTTP Request-Response with Logger example application illustrates:
1. Receiving HTTP requests and returning HTTP responses
2. Logging the payload

<img width="60%" src="../../../assets/img/migration-examples/http-request-response-with-logger-use-case.png">

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/http-request-response-with-logger``) and click **Finish**.

5. Open the **HttpRequestResponseWithLogger.xml** under **http-request-response-with-logger/HttpRequestResponseWithLogger/src/main/synapse-config/api/HttpRequestResponseWithLoggerAPI.xml** directory.<br>
    <img width="60%" src="../../../assets/img/migration-examples/http-request-response-with-logger.png">

6. The **HttpRequestResponseWithLoggerAPI.xml** is the graphical view of the simple http-request-response-with-logger service.

7. Run the sample by right click on the **HttpRequestResponseWithLoggerCompositeApplication** under the main **http-request-response-with-logger** project and selecting **Export Project Artifacts and Run**.

8. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

9. Make a GET request to *http://localhost:8290/api/echo*. Application presents a message that reads `/echo`.

10. Now replace the word `echo` with the word `moon`, and execute. The application responds with `/moon`.

### How it Works

This example consists a simple [Synapse API](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/creating-an-api/) that accept a HTTP request and read the postfix of the URL. Log mediator used to log the URL prefix and [Payload factory](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/payloadFactory-Mediator/) mediator to set the payload.

After opening the Integration project, there are two projects under the http-request-response-with-logger main project. First project (named as "HttpRequestResponseWithLogger") is the WSO2 Integration Project where all the integration use-case related files are stored. Second one is the "HttpRequestResponseWithLoggerCompositeApplication", which is used as the Packaging and exporting artifact to the server runtime.

You can run **Composite Application** using WSO2 Enterprise Integrator server runtime.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/http-request-response-with-logger.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Message Transforming Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.
