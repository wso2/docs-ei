---
title: Proxying a REST API #
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example shows how to proxy your API. Applications send service requests to your proxy, which in turn calls the real API.

### Assumptions ###

This document assumes that you are familiar with the [Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Studio, consider completing one or more [Integration Studio Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/). Further, this example assumes that you have a basic understanding of [EI flows](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/key-concepts/).
 

### Example Use Case ###

To demonstrate the basic procedure of creating a proxy application, this document uses the public Box API as an example REST API to stand in for any REST API that you have that you might want to proxy through an EI application. The specific configuration for Box is summarized here, but you will need to replace this with the corresponding information for your own RESTful services that you wish to proxy.

<img width="60%" src="../../../assets/img/migration-examples/proxying-a-rest-api-use-case.png">

### Set Up and Run the Example ###

To follow along with the steps in this example, you must have a [box.com](https://app.box.com/files) account, which you can create for free if you don't already have one.

#### Registering an App in the Box Developer Portal ####

The steps below are only needed in this particular example so that you can test your finished proxy for the Box API by simulating an API call from an application. They don't necessarily match the steps you need to carry out to test your own API.

1. Go to Box's developer portal: [developers.box.com](https://developers.box.com/). If you do not have an account, you need to create one [here](https://app.box.com/signup/personal). If you have one, click **My apps** in the upper-right corner of the [page](https://developers.box.com/).

2. Click **Create New App** button. Select **Custom App** and click **Next**. For the Authentication Method, select **Standard OAuth 2.0 (User Authentication)** and click **Next**. Give it any name, such as MyProxy, then click **Create App**. 

1. Click **View Your App**.

1. Look for the *client_id* and the *client_secret*. Copy these to a safe place, as you will need them later.

1. Add a *redirect_url*. For the purpose of this exercise, any https URL will do, even https://www.google.com.

Leave the box developer portal open for now, as you will return here later to request an OAuth token. Because the OAuth token expires very quickly, it's best to build the flow before you request it.

**Optional Step**: If you're using HTTPS, as the Box API requires, you should create a keystore file to certify the communication. However, the EI runtime already has a keystore configured so you may skip this step. If you wish to use you own keystore, it can be done using the keytool provided by Java, found in the bin directory of your Java installation. Navigate to this directory on your machine using the command line, then execute the following command to create a keystore file:

		keytool -genkey -alias wso2 -keyalg RSA -keystore keystore.jks

You can find instructions to configure the WSO2 Micro Integrator in the [official documentation](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/security/configuring_keystores/)

#### Building the Proxy in Studio ####

Next, build your proxy application in Integration Studio. Your proxy application needs to:

1. Accept incoming service calls from applications and route them to the Box API.

2. Copy any message headers from the service call and pass them along to the Box API.

3. Disable the default status code exception check to allow any error messages that the Box API returns to be passed on to the application. 

4. Capture message headers from the Box API's response and attach them to the response message.

5. Route the response to the application that made the service call.

The following steps describe how to obtain a token for the Box API and use it to test the proxy you have just built by simulating an API call from an application.

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this github project ('proxying-a-rest-api' folder of the downloaded github repository).

5. Open the **proxyingARestApiAPI.xml** under **proxying-a-rest-api/proxyingARestApi/src/main/synapse-config/api/** directory. 

6. The **proxyingARestApiAPI.xml** is the graphical view of the content based routing sample.<br>
  	<img width="60%" src="../../../assets/img/migration-examples/proxying-a-rest-api.png">

7. Run the sample by right click on the **proxyingARestApiCompositeApplication** under the main **proxying-a-rest-api** project and selecting **Export Project Artifacts and Run**.

8. In any Web browser, enter the following URL: 

		http://localhost:8290/box/oauth2/authorize?response_type=code&client_id=<CLIENT_ID>

	Replace <CLIENT_ID> in the URL above with the client_id provided by Box when you registered your new app.

9. Box will prompt you to log in with your username and password. You can use your personal credentials or create a new test account.

10. Before you click **Grant access to Box**, you should be ready for the following steps, as the token code you will obtain will expire in only 30 seconds. Be ready to send **http://localhost:8290/box/oauth2/token** as an HTTP **POST** request that includes a body with the properties below:

		Attribute		Value
		grant_type		authorization_code
		code			<fill this in during the next step>
		client_id		<client_id provided by Box when you registered your app>
		client_secret	<client_secret provided by Box when you registered your app>

	To send this request, use a browser extension such as [Advanced Rest Client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo) (Google Chrome), or the [curl](http://curl.haxx.se/) command line utility. 

7. Once you have prepared for the next step, go back to the browser page where you entered your Box credentials and click **Grant access to Box**.

8. The browser is redirected to the page you set as the redirect on your Box app. For this exercise, the page itself is irrelevant, but the full URL will include an extra parameter named code. For example:

		https://www.google.com/?state=&code=<CODE>

9. Copy the value of &lt;CODE&gt; from the URL and paste it into your POST request so that its properties are the following:
	
		Attribute		Value
		grant_type		authorization_code
		code			<code provided by redirect URL>
		client_id		<client_id provided by Box when you registered your app>
		client_secret	<client_secret provided by Box when you registered your app>

10. Send the request.

11. This POST request returns a JSON object with several fields. Copy the value corresponding to *access_token*, as you will need it soon. The *access_token* lasts for an hour before expiring.

12. Now you can make proper requests to your proxy. You must include *access_token* on every request as a header with the name Authorization.

		Header			Value
		Authorization	Bearer <access_token>

	The value of the header must include the word Bearer followed by a space and then the access_token. For example:
	
		Authorization=Bearer 1234123412341234

Try making a **GET** request to [http://localhost:8290/box/2.0/folders/0](http://localhost:8290/box/2.0/folders/0), remembering to include the Authorization header. 

### How it Works ###

Follow the anatomy described here to build a proxy application in Integration Studio that abstracts your API to a new layer. Your proxy application needs to:

1. Accept incoming service calls from applications and route them to the URI of your target API.
2. Copy any message headers from the service call and pass them along to your API.
3. Avoid passing internal WSO2 headers both to the API and back to the requester.
4. Capture message headers from your API's response and attach them to the response message.
5. Route the response to the application that made the service call.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/proxying-a-rest-api.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further 

* Learn more about configuring a [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Learn more about configuring [Endpoints](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/endpoint-properties/) in Studio.
