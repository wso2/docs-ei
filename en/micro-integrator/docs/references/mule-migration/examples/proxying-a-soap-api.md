---
title: Proxying a SOAP API #
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example shows how to proxy your API. Applications send service requests to your proxy, which in turn calls the real API.

### Assumptions ###

This document assumes that you are familiar with the [Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Studio, consider completing one or more [Integration Studio Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/). Further, this example assumes that you have a basic understanding of [EI flows](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/key-concepts/).

Furthermore, this document assumes that you have a SOAP API that has not been built to run on WSO2 EI.

This document describes the details of the example within the context of Integration Studio, WSO2 EI’s graphical user interface, and includes configuration details for the Source Editor where relevant.

### Example Use Case ###

In this simple example, an application serves as a minimum proxy that can routes request and response to and from a SOAP API. After receiving request, it calls SOAP API of service described in wsdl (resources/tshirt.wsdl) and lists inventories.

<img width="60%" src="../../../assets/img/migration-examples/proxying-a-soap-api-use-case.png">

### Set Up and Run the Example ###

Complete the following procedure to create, then run this example in your own instance of Integration Studio. Skip ahead to the next section if you prefer to simply examine this example via code snippets.

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this github project ('proxying-a-soap-api' folder of the downloaded github repository).

5. Open the **proxyingASoapApiAPI.xml** under **proxying-a-soap-api/proxyingASoapApi/src/main/synapse-config/api/** directory. 

6. The **proxyingASoapApiAPI.xml** is the graphical view of the content based routing sample.<br>
    <img width="60%" src="../../../assets/img/migration-examples/proxying-a-soap-api.png">

7. Run the sample by right click on the **proxyingASoapApiCompositeApplication** under the main **proxying-a-soap-api** project and selecting **Export Project Artifacts and Run**.

5. Send the following POST request with xml body to your API via the proxy URL: http://localhost:8290/listInventory.

		    <ns1:ListInventory xmlns:ns1='http://wso2.org/tshirt-service' />

You can use a browser extension such as Postman or the curl command line utility. As a response to this request, you should receive a list of inventories.

### How it Works ###

Follow the anatomy described here to build a proxy application in Integration Studio that abstracts your API to a new layer. Your proxy application needs to:

1. Accept incoming service call from applications and route them to the URI of your target SOAP API.
2. Return the response back to the application that made the service call.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/proxying-a-soap-api.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further 

* Learn more about configuring a [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Learn more about configuring [Endpoints](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/endpoint-properties/) in Studio.



