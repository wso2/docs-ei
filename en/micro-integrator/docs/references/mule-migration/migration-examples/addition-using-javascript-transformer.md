# Addition using JavaScript Transformer

This application demonstrates how to parse an incoming JSON message and process it using the Script mediator with JavaScript code. This example was designed to demonstrate the ability of a WSO2 Integration applications to interact with JSON payload. The goal is to introduce users to Integration Studio by illustrating very simple functionality.

### Assumption

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical 
developer tool. This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To 
increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Set Up and Run the Example

Follow the steps in this procedure to create and run this example in your own instance of Integration Studio. You can create template applications straight out of the box in Integration Studio and tweak the configurations of the use case-based templates to create your own customized applications in WSO2 Integrator.

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).
2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.
3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.
4. Browse and select the file path to the downloaded sample of this github project ("addition-using-javascript-transformer" folder of the downloaded github repository).
5. Open the **AdditionUsingJavascriptTransformer.xml** under **addition-using-javascript-transformer/AdditionUsingJavascriptTransformer/src/main/synapse-config/proxy-services** directory. 
6. The **AdditionUsingJavascriptTransformer.xml** is the graphical view of the javascript transform mediator.
7. Run the sample by right click on the **AdditionUsingJavascriptTransformerCompositeApplication** under the main **addition-using-javascript-transformer** project and selecting **Export Project Artifacts and Run**.
8. Send a curl request to the server with following command.
``` 
curl -X POST \
  http://localhost:8290/services/AdditionUsingJavascriptTransformer \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: c977f8ea-c493-a548-e63b-191edce3dba1' \
  -d '{ "a": 3, "b": 4 }' 
```
9. You will receive the response as "Sum is: 7"

### How it Works

The Addition Using Javascript Transformer example consists of one simple proxy service which transform given JSON payload into a value.

Here, [script mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/script-Mediator/) work as message transformer that sum up the given JSON object values. Below Javascript code read the message from the input message context and do the calculation. Final output set into the message context. Payload factory mediator read this property and send it back to the request sender through the respond mediator.
```
var list = eval(mc.getPayloadJSON());
var sum = 0;
for (var i in list){
 sum += list[i];
}
 mc.setProperty('result_str', sum);
```

After opening the Integration project, there are two projects under the addition-using-javascript-transformer main project. First project (named as "AdditionUsingJavascriptTransformer") is the WSO2 Integration Project where all the integration use-case related file are stored. Second one is the "AdditionUsingJavascriptTransformerCompositeApplication", which is used as the Packaging and exporting artifact to the server runtime. 

You can run **Composite Application** using WSO2 Enterprise Integrator server runtime.

### Download the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../../assets/attach/connectors/FileConnector.zip">
    <img src="../../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Read about the concept of [WSO2 Script Mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.