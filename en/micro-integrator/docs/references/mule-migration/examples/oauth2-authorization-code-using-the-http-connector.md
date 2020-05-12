---
title: OAuth2 Authorization Code Example
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

Often you are faced with a requirement to handle authorization to the third party service. This example application illustrates how to execute it using WSO2 EI.

### Assumptions

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical developer tool. This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example, by hitting an HTTPS endpoint a user will attempt to grant the access to his data at Box Service. For this purpose, OAuth authorization will be triggered. A user will be asked to enter his user name and password. If successful, he can click a button in order to grant the access.

<img width="90%" src="../../../assets/img/migration-examples/oauth2-authorization-code-using-the-http-connector-use-case.png">

### Set Up and Run the Example ###

To follow along with the steps in this example, you must have a [box.com](https://app.box.com/files) account, which you can create for free if you do not already have one.

#### Registering an App in the Box Developer Portal ####

The steps below are only needed in this particular example so that you can test your finished proxy for the Box API by simulating an API call from an application. They do not necessarily match the steps you need to carry out to test your own API.

1. Go to Box's developer portal: [developers.box.com](https://developers.box.com/). If you do not have an account, you need to create one [here](https://app.box.com/signup/personal). If you have one, click **My apps** in the upper-right corner of the [page](https://developers.box.com/).

2. Click **Create New App** button. Select **Custom App** and click **Next**. For the Authentication Method, select **Standard OAuth 2.0 (User Authentication)** and click **Next**. Give it any name, such as MyProxy, then click **Create App**. 

1. Click **View Your App**.

1. Look for the *client_id* and the *client_secret*. Copy these to a safe place, as you will need them later.

1. Add a *redirect_url*. For this example, add **https://localhost:8253/oauth/callback** and click **Save Changes**.

Leave the box developer portal open for now, as you will return here later to request an OAuth token. Because the OAuth token expires very quickly, it's best to build the flow before you request it.

**Optional Step**: If you're using HTTPS, as the Box API requires, you should create a keystore file to certify the communication. However, the EI runtime already has a keystore configured so you may skip this step. If you wish to use you own keystore, it can be done using the keytool provided by Java, found in the bin directory of your Java installation. Navigate to this directory on your machine using the command line, then execute the following command to create a keystore file:

		keytool -genkey -alias wso2 -keyalg RSA -keystore keystore.jks

You can find instructions to configure the WSO2 Micro Integrator in the [official documentation](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/security/configuring_keystores/)

#### Building the example in Studio ####

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this project (``integration-studio-examples/migration/mule/oauth2-authorization-code-using-the-http-connector``) and click **Finish**.

5. Open the **OAuth2AuthorizationCodeAPI.xml** under **oauth2-authorization-code-using-the-http-connector/OAuth2AuthorizationCode/src/main/synapse-config/api/OAuth2AuthorizationCodeAPI.xml** directory.<br>
  	<img width="60%" src="../../../assets/img/migration-examples/oauth2-authorization-code-using-the-http-connector.png">


6. The **OAuth2AuthorizationCodeAPI.xml** is the graphical view of the simple hello world service.

7. Open the **config.xml** in the **oauth2-authorization-code-using-the-http-connector/resources-oauth** directory and replace **client_id** and **client_secret** with the credentials you obtained from Box in the previous section.

8. Run the sample by right click on the **OAuth2AuthorizationCode** under the main **oauth2-authorization-code-using-the-http-connector** project and selecting **Export Project Artifacts and Run**.

9. In any Web browser, enter the following URL: 

		https://localhost:8253/web

10. Box will prompt you to log in with your username and password. Click **Authorize**. You can use your personal credentials or create a new test account.

11. Clicking **Grant access to Box** (or *Deny access to Box as well*) will redirect you to **http://localhost:8081/callback**

12. Then try the following to perform search call to the Box: 

		https://localhost:8253/web/loginDone

13. Then the example will try consume a resource using the recently obtained token (in this case, search for items containing the term "wso2") and display the result.
  
### How it works

#### oauth2AuthorizationFlow

This example demonstrated two key flows of OAuth2 and its usage. There are two main API resources defined in this example. One flow to retrieve the token
after logging the user in and the other flow to invoke the Box search API using the retrieved token.

When a request pointing to local authorization URL https://localhost:8253/web arrives, it redirects the processing to Box that basically triggers the OAuth authorization. After the successful operation, the client is redirected back defined as external callback URL which is in this case, https://localhost:8253/oauth/callback. This is another API we have defined to handle the callback.  

To start an OAuth operation you will need a *clientId*, a *clientSecret* issued by the third party service and a redirect URL that is used after the finished authorization process. To demonstrate some capabilities of this component, custom URL parameters (i.e. *box_device_id* and *box_device_name*).

Once the user is authorized, it is possible to read resources from Box using the recently obtained token, showing the result in the browser.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/oauth2-authorization-code-using-the-http-connector.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Integration Studio.
* Learn more about configuring [Endpoints](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/endpoint-properties/) in Integration Studio.
