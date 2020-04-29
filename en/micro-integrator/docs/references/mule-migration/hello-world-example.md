# Hello World

This application demonstrates a simple HTTP request-response activity. WSO2 EI responds to end user calls submitted via a Web browser with a message that reads, "Hello World". This example was designed to demonstrate the ability of WSO2 Integration applications to interact with an end user via an HTTP request. The goal is to introduce users to Integration Studio by illustrating very simple functionality.

### Assumption

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s developer tool. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Set up and run the example

Follow the steps in this procedure to create and run this example in your own instance of Integration Studio. You can create template applications by default in Integration Studio and tweak the configurations of the use case-based templates to create your own customized applications.

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).
2. Click the **File** menu and select the **Import...** item.
3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.
4. Browse and select the file path to the downloaded sample of this github project ("hello-world" folder of the downloaded github repository).
5. Open the **HelloWorld.xml** under **hello-world/HelloWorld/src/main/synapse-config/api/HelloWorld.xml** directory. 
6. The **HelloWorld.xml** is the graphical view of the simple hello world service.
7. Run the sample by right click on the **HelloWorldCompositeApplication** under the main **hello-world** project and selecting **Export Project Artifacts and Run**.
8. In the address bar, type the following URL: `http://localhost:8290/helloworld`
9. Press Enter to elicit a response from the application.

### How it works

The Hello World example consists of one simple [Synapse API](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/creating-an-api/). This Synapse API accepts an HTTP request, sets a static payload on the message, then returns a response to the end user.

As its name suggests, the [Payload Factory](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/payloadFactory-Mediator/) sets a value in the message payload. In this example, the value utilizes a WSO2 expression to set a static string on the payload. Like the HTTP endpoint, the configuration contains descriptive notes to help you understand what the component does.

After opening the Integration project, there are two projects under the hello-world main project. First project (named as "HelloWorld") is the WSO2 Integration Project where all the integration use case related files are stored. Second one is the "HelloWorldCompositeApplication", which is used as the Packaging and exporting artifact to the server runtime. 

You can run **Composite Application** using WSO2 Enterprise Integrator server runtime.

### Download the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/attach/connectors/FileConnector.zip">
    <img src="../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Message Transforming Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.