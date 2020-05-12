---
title: Retry Mechanism Example
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This application shows on a simple use case how the retry mechanism could be implemented in WSO2 Enterprise Integrator 
message mediation. It receives the HTTP request and simulates the failure in the flow. Once the flow fails the retry mechanism proceed with series of retries based on the configuration.

## Assumptions

This document describes the details of the example within the context of WSO2 Integration Studio, editor to develop WSO2 EI artifacts. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

## Example Use Case

The application consumes HTTP requests and based on the query parameter `fail` either successfully completes the flow or throw an exception inside the message flow causing the retry mechanism to repeat the flow. 

<img width="90%" src="../../../assets/img/migration-examples/retry-mechanism-example-use-case.png">

> **NOTE**: The example demonstrates how to call a message flow recursively to achieve the requirement. 


## Set Up and Run the Example

Follow the steps in this procedure to create and run this example in your own instance of Integration Studio. You can create template applications straight out of the box in Integration Studio and tweak the configurations of the use case-based templates to create your own customized applications in WSO2 Integrator.

1. Start WSO2 Integration Studio. See [Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/) 

2. In your menu in Studio, click the File menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this github project (`retry-mechanism-example` folder of the downloaded github repository)

5. Open the **mediationRetryAPI.xml** file in the **retry-mechanism-example/mediationRetryIntegrationProject/src/main/synapse-config/api/** directory. The **mediationRetryAPI.xml** is the graphical view of the retry mechanism sample.<br>
    <img width="60%" src="../../../assets/img/migration-examples/retry-mechanism-example.png">

6. In the **Package Explorer**, right-click **Composite Application Project** and select **Export Project Artifacts and Run**. Select all the artifacts in the wizard. Studio runs the application on the embedded server.

7. In the REST client e.g. Postman or using embedded HTTP4e client send the following request: 
   ```
   http://localhost:8290/trigger?fail=false
   ```
   In the HTTP response you can see the message *Processing successfully* completed and the application log contains 
   the following log entries:
   ```
    [2020-04-27 08:34:51,198]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - Log processing triggered = Attempt to trigger processing
    [2020-04-27 08:34:51,217]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = Processing successfully completed.
   ```
   The flow did not fail, no retry was needed and the processing was successful.

8. Now, let's send another request: 
   ```
   http://localhost:8290/trigger?fail=true
   ```
   
   The HTTP status is 500 (Internal Server Error) now and the response body contains the error message indicating the flow fails even after predefined number of retries:
   ```
   <Response>
    <message>Processing failed. </message>
    <reason>The script engine returned an error executing the inlined js script function mediate</reason>
   </Response>
   ```
    The server log now contains the following entries:
    ```
    [2020-04-27 08:38:43,691]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - Log processing triggered = Attempt to trigger processing
    [2020-04-27 08:38:43,713] ERROR {org.apache.synapse.mediators.bsf.ScriptMediator} - The script engine returned an error executing the inlined js script function mediate com.sun.phobos.script.util.ExtendedScriptException: org.mozilla.javascript.EcmaError: ReferenceError: "Exception" is not defined. (<Unknown Source>#1) in <Unknown Source> at line number 1
    [2020-04-27 08:38:43,784]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = Processing failed
    [2020-04-27 08:38:43,786]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = 1.0
    [2020-04-27 08:38:48,808]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - Log processing triggered = Attempt to trigger processing
    [2020-04-27 08:38:48,813] ERROR {org.apache.synapse.mediators.bsf.ScriptMediator} - The script engine returned an error executing the inlined js script function mediate com.sun.phobos.script.util.ExtendedScriptException: org.mozilla.javascript.EcmaError: ReferenceError: "Exception" is not defined. (<Unknown Source>#1) in <Unknown Source> at line number 1
    [2020-04-27 08:38:48,866]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = Processing failed
    [2020-04-27 08:38:48,866]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = 2.0
    [2020-04-27 08:38:53,873]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - Log processing triggered = Attempt to trigger processing
    [2020-04-27 08:38:53,874]  INFO {API_LOGGER.mediationRetryAPI} - Log processing triggered = Attempt to trigger processing
    [2020-04-27 08:38:53,878] ERROR {org.apache.synapse.mediators.bsf.ScriptMediator} - The script engine returned an error executing the inlined js script function mediate com.sun.phobos.script.util.ExtendedScriptException: org.mozilla.javascript.EcmaError: ReferenceError: "Exception" is not defined. (<Unknown Source>#1) in <Unknown Source> at line number 1

    ...

    [2020-04-27 08:39:04,073]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = Processing failed
    [2020-04-27 08:39:04,073]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - message = 5.0
    ```

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/retry-mechanism-example.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

## Go Further

* Learn more about [configuring sequences](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/sequence-properties/) in Studio.
* Read about the [message mediators](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/about-mediators/) in WSO2 Enterprise Integrator.

