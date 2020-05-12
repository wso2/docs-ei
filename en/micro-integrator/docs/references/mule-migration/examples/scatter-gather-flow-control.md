---
title: Scatter Gather flow control to email
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example shows the usage of the scatter-gather control flow to aggregate data in parallel and return the result in JSON. 

The example uses prepared data as input for two resources that should be aggregated. The data represents contacts 
information with the following structure:

Example data structure(contacts-1.csv, contacts-2.csv) in:
		firstname;surname;phone;email
		John;Doe;096548763;john.doe@texasComp.com
		Jane;Doe;091558780;jane.doe@texasComp.com
		...		

Contacts are aggregated to a JSON structure that represents data from both sources (`contacts-1.csv` and `contacts-2.csv`).

[Clone Mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/clone-Mediator/) is used to clone the message and [Aggregate Mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/aggregate-Mediator/) is used to aggregate data.

### Assumptions

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical developer tool. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

This example read two CSV files and aggregate the two contents into one payload. then the payload is transformed to JSON 
format before responding back to the client.

<img width="60%" src="../../../assets/img/migration-examples/scatter-gather-flow-control-use-case.png">

#### Set Up and Run this Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/scatter-gather-flow-control``) and click **Finish**.

5. Lets add the file connector into the workspace. Right click on the **ScatterGatherFlowControl** and select **Add or Remove Connector**. Keep the **Add connector** option selected and click **Next>**. Search for `file` using the search bar and click the download button located at the bottom right corner of the file connector. Click **Finish**.

6. Open the **ScatterGatherFlowControl.xml** under **scatter-gather-flow-control/ScatterGatherFlowControl/src/main/synapse-config/api** directory.<br>
	<img width="60%" src="../../../assets/img/migration-examples/scatter-gather-flow-control.png">

7. Provide file paths of `contacts-1.csv` and `contacts-2.csv` resides in `integration-studio-examples/migration/mule/scatter-gather-flow-control/resources` location.

8. Run the sample by right click on the **ScatterGatherFlowControlCompositeApplication** under the main **scatter-gather-flow-control** project and selecting **Export Project Artifacts and Run**.

9. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

10. Make a GET request to *http://localhost:8290/aggregate*

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/scatter-gather-flow-control.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Read more about the [Scatter Gather Flow Control](https://docs.wso2.com/display/IntegrationPatterns/Scatter-Gather).
* Learn more on [File Connector](https://docs.wso2.com/display/ESBCONNECTORS/File+Connector).
