---
title: Hello World
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This application demonstrates a simple HTTP request-response activity. WSO2 EI responds to end user calls submitted via a Web browser with a message that reads, "Hello World". This example was designed to demonstrate the ability of a WSO2 Integration applications to interact with an end user via an HTTP request. The goal is to introduce users to Integration Studio by illustrating very simple functionality.

### Assumption

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical developer tool. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example a simple Hello World payload will be created using [PayloadFactory Mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/payloadFactory-Mediator/).

<img width="50%" src="../../../assets/img/migration-examples/hello-world-use-case.png">

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/hello-world``) and click **Finish**.

5. Open the **HelloWorld.xml** under **hello-world/HelloWorld/src/main/synapse-config/api/HelloWorld.xml** directory.<br>
    <img width="60%" src="../../../assets/img/migration-examples/hello-world.png">

6. The **HelloWorld.xml** is the graphical view of the simple hello world service.

7. Run the sample by right click on the **HelloWorldCompositeApplication** under the main **hello-world** project and selecting **Export Project Artifacts and Run**.

8. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

9. Make a GET request to `http://localhost:8290/helloworld`.

### How it Works

The Hello World example consists of one simple [Synapse API](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/creating-an-api/). This Synapse API accepts an HTTP request, sets a static payload on the message, then returns a response to the end user.

As the name suggests, the [Payload Factory](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/payloadFactory-Mediator/) sets a value in the message payload. In this example, the value utilizes a WSO2 expression to set a static string on the payload. 

After opening the Integration project, there are two projects under the hello-world main project. First project, "HelloWorld" is the WSO2 Integration Project where all the integration use-case related files are stored. The second project is the "HelloWorldCompositeApplication", which is used as the packaging to export the artifacts to the server runtime. 

You can run **Composite Application** using WSO2 Enterprise Integrator server runtime.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/hello-world.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Message Transforming Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.
