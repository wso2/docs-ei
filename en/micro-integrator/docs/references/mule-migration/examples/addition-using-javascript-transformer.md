---
title: Addition using JavaScript Transformer
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This application demonstrates how to parse an incoming JSON message and process it using the [Script Mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/script-Mediator/)  with JavaScript code. This example was designed to demonstrate the ability of a WSO2 Integration applications to interact with JSON payload.

### Assumption

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical developer tool. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example, we send a JSON message containing two numbers to an HTTP endpoint. The [Script Mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/script-Mediator/) 
then process the data and add two numbers. 

<img width="70%" src="../../../assets/img/migration-examples/addition-using-javascript-transformer-use-case.png">


### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (``integration-studio-examples/migration/mule/addition-using-javascript-transformer``) and click **Finish**.

5. Open the **AdditionUsingJavascriptTransformer.xml** under **addition-using-javascript-transformer/AdditionUsingJavascriptTransformer/src/main/synapse-config/api/AdditionUsingJavascriptTransformer.xml** directory.<br>
    <img width="65%" src="../../../assets/img/migration-examples/addition-using-javascript-transformer.png">


6. The **AdditionUsingJavascriptTransformer.xml** is the graphical view of the JavaScript transform mediator.

7. Run the sample by right click on the **AdditionUsingJavascriptTransformerCompositeApplication** under the main **addition-using-javascript-transformer** project and selecting **Export Project Artifacts and Run**.

8. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

9. Send a POST request to *http://localhost:8290/add* attaching a JSON to the body of the message. A sample JSON is 
provided below.
  ```json
  { "a": 3, "b": 4 }
  ```

10. You will receive the response as `Sum is: 7`

### How it Works

Here, [Script Mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/script-Mediator/) works as message transformer that add the given JSON object values. Below Javascript code read the message from the input message context and do the calculation. Final output is set into the message context. Payload factory mediator read this property and respond it back to the request sender through the respond mediator.

```
var list = eval(mc.getPayloadJSON());
var sum = 0;
for (var i in list){
 sum += list[i];
}
mc.setProperty('result_str', sum);
```

After opening the Integration project, there are two projects under the addition-using-javascript-transformer main project. 

First project (named as "AdditionUsingJavascriptTransformer") is the WSO2 Integration Project where all the integration use-case related file are stored. Second one is the "AdditionUsingJavascriptTransformerCompositeApplication", which is used as the Packaging and exporting artifact to the server runtime. 

You can run **Composite Application** using WSO2 Enterprise Integrator server runtime.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/addition-using-javascript-transformer.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Script Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.
