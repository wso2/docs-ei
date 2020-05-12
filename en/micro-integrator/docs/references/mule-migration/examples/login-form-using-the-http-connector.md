---
title: Login form using the HTTP connector Example
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example shows you how to use WSO2 EI server to build simple HTTP application with a login form. Here, we are using three HTML pages (login.html, loginSuccessful.html, and loginFailure.html) to illustrate the HTTP login application and those are stored in the Registry project.

### Assumptions ###

This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/quick-start-guide/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example, a user submits a username and a password using the HTML login form provided by the application. Once user submit the username and password values, WSO2 EI server receives the credentials and respond the relevant HTML page with the authentication result.  
* **Get Login Page** (GET `/api/login`). This resource get the index.html page from the Registry project and respond back.

    <img width="60%" src="../../../assets/img/migration-examples/login-form-using-the-http-connector-get-login-page-flow.png">

* **Do Login** (POST `/api/login`). This resource checks the username and password is equals to the `wso2` and respond the relevant HTML page based on the authentication status.

    <img width="70%" src="../../../assets/img/migration-examples/login-form-using-the-http-connector-do-login-flow.png">

* **Requester Login** (GET `/api/requesterLogin`). TThis resource is responsible for filling in the correct credentials and calling Do Login resource in order to make a successful login.

    <img width="70%" src="../../../assets/img/migration-examples/login-form-using-the-http-connector-call-login-flow-using-requester.png">

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (`integration-studio-examples/migration/mule/login-form-using-the-http-connector`) and click **finish**.

5. Run the sample by right click on the **LoginFormUsingHttpConnectorCompositeApplication** under the main **login-form-using-the-http-connector** project and selecting **Export Project Artifacts and Run**.

6. Open your browser and hit [http://localhost:8290/api/login](http://localhost:8290/api/login).<br>
    <img width="40%" src="../../../assets/img/migration-examples/login-form-using-the-http-connector-login-page.png">

7. Enter `wso2` for username and `wso2` for password. Hit submit button.

8. You should receive this response: 
    <img width="40%" src="../../../assets/img/migration-examples/login-form-using-the-http-connector-login-success.png">
    
   Alternately you will receive the following:
    <img width="40%" src="../../../assets/img/migration-examples/login-form-using-the-http-connector-login-error.png">

9. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

10. Make a GET request to `http://localhost:8290/api/requesterLogin`, you should see the successful message as the correct credentials are set by the flow and the login is always successful.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/login-form-using-the-http-connector.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
