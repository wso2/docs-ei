---
title: Sending JSON Data to a JMS Queue
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

WSO2 offers components that are easy to use for connecting to JMS queues and topics. This example shows how to use Apache 
ActiveMQ, a leading open-source JMS implementation from Apache that supports the JMS 1.1 specification.

### Assumptions

This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/), WSO2 EI’s graphical developer tool. To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Sample Use Case
In this example, an HTTP request holding JSON sales data reaches an HTTP endpoint. A success message is logged and the data is added to a JMS queue, where you can view it in the ActiveMQ admin console.

<p align="center">
  <img width="70%" src="../../../assets/img/migration-examples/sending-json-data-to-a-jms-queue-use-case.png">
</p>

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/sending-json-data-to-a-jms-queue``) and click **Finish**.

5. Open the **SendingJsonDataToAJmsQueue.xml** under **sending-json-data-to-a-jms-queue/SendingJsonDataToAJmsQueue/src/main/synapse-config/api/SendingJsonDataToAJmsQueue.xml** directory.<br>
    <img width="70%" src="../../../assets/img/migration-examples/sending-json-data-to-a-jms-queue.png">

6. Configure WSO2 Micro Integrator to connect with [ActiveMQ](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/brokers/configure-with-ActiveMQ/).

7. Lets [export composite application](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/exporting-artifacts/) in Micro Integrator. Right click on the **SendingJsonDataToAJmsQueueCompositeApplication** under the main **sending-json-data-to-a-jms-queue** project and select **Export Composite Application Project**. Browse and provide carbonapps location of Micro Integrator(`[MI_HOME]/repository/deployment/server/carbonapps` where `MI_HOME` is the home directory of the distribution you downloaded). 

8. Open a terminal and navigate to the `MI_HOME/bin/` directory and execute the relevant command:
    * On MacOS/Linux/CentOS:<br/>
    ``sh micro-integrator.sh``
    * On Windows:<br/>
    ``micro-integrator.bat``

9. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

10. Make a POST request to *http://localhost:8290/sales* with following JSON message body.
    ```json
    {"ITEM_ID" : 001, "ITEM_NAME" : "Shirt", "QTY" : 1, "PRICE" : 20}
    ```

11. Log in to the ActiveMQ admin page at [http://localhost:8161/admin/queues.jsp](http://localhost:8161/admin/queues.jsp) with the default username and password “admin”. Check whether the message was added to the queue. In the ActiveMQ queue, click the Sales link and then the link under the Message ID column. You'll see the details of the message that was added to the JMS queue.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/sending-json-data-to-a-jms-queue.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Message Transforming Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.
