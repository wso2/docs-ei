# Amazon S3 Connector Example 

Google firebase is a rich modern platform to create quick mobile app back-ends, with a ton of built-in and ready-to-integrate features. The most used feature of Firebase is as a back-end. But along with this back-end, one of the popular features is **Push notifications**. We can register Android, IOS and Web based backend to Google Firebase applications and push notifications to them. Firebase being a Google product a lot of people use it for reliable push notifications. In mobile world push notifications are very popular. 

You can use firebase console itself it trigger out messages to the registered devices or you can even schedule a CRON job. Firebase provides a `Messaging Console`, which you can use to send all kinds of push messages, filter target users, schedule messages and much more. Needlessly to state, it provides notification history and reports as well. However, when it come to integration scenarios we should be able to generate a notification externally and send it to Google Firebase. 

**Google Firebase Connector** is useful for integrating Google Firebase with other enterprise applications, on-premise or cloud. You can generate notifications and send them to Firebase so that they will be triggered to all the registered devices on that topic.

Please refer below for more use cases. 

https://firebase.google.com/docs/cloud-messaging/android/topic-messaging
https://inducesmile.com/android/android-firebase-push-notification-with-topic-message-subscription/
https://www.youtube.com/watch?v=aG2JC8c9EK0


## What you'll build

In this example let us see how we can use Google Firebase Connector to generate a push notification based on an HTTP API invocation. The integration logic will extract information from HTTP message to the API to generate the push notification. 

**Note**
The connector can also be used to register a device to a particular topic on Firebase. We will not cover it here. We will also not cover how the notifications can be received using Android or IOS apps. Please refer to online resources to get know about them. 

Overall integration scenario would look like below. 

<image> 

## Setting up the environment

You need to create an application at Google Firebase and get the credentials required. Please follow [Setting up Google Firebase](google-firebase-setup.md) on how to do that. 

## Configure the connector in WSO2 Integration Studio

Follow these steps to set up the Integration Project and import Google Firebase connector into it.

{!references/connectors/importing-connector-to-integration-studio.md!} 

1. Right click on the created Integration Project and select, -> **New** -> **Rest API** to create the REST API.
2. Specify the API name as `FirebaseNotify` and API context as `/firebasenotify`. You can go to the source view of the XML configuration file of the API and copy the following configuration. 
   ```
   ```
3. Right click on the created Integration Project and select, -> **New** -> **Sequence** to create a sequence. Here we will define the logic how the push notification should be constructed. Note how this sequence is called by the API. 
   ```
   ```

**NOTE:**
The parameters under `<init>` section of the configuration above are referring to the credentials we obtained from Google Firebase in above steps. The parameters are mapped to the keys of the Json file that you have downloaded as below. 

```
accountType --> type
projectId --> project_id
privateKeyId --> private_key_id
privateKey --> private_key
clientEmail --> client_email
clientId --> client_id
authUri --> auth_uri
tokenUri --> token_uri
authProviderCertUrl --> auth_provider_x509_cert_url
clientCertUrl --> client_x509_cert_url
```

Now we can export the imported connector, sequence and the API into a single CAR application. CAR application is the one we are going to deploy to server runtime.


{!references/connectors/exporting-artifacts.md!}


Now the exported CApp can be deployed in Enterprise Integrator Runtime so that we can run it and test. 
**NOTE**
Download following jars [firebase-admin-6.5.0.jar](https://mvnrepository.com/artifact/com.google.firebase/firebase-admin/6.5.0), [google-auth-library-credentials-0.11.0.jar](https://mvnrepository.com/artifact/com.google.auth/google-auth-library-credentials/0.11.0), [google-auth-library-oauth2-http-0.11.0.jar](https://mvnrepository.com/artifact/com.google.auth/google-auth-library-oauth2-http/0.11.0), [api-common-1.7.0.jar](https://mvnrepository.com/artifact/com.google.api/api-common/1.7.0) and place those into  <Product_HOME>/lib folder.

{!references/connectors/deploy-capp.md!}

## Testing

We can use Curl or Postman to try the API. The testing steps are provided for curl. Steps for Postman should be straightforward and can be derived from the curl requests.

1. Create a file called data.xml with the following content. 
    ```
    ```
2. Invoke the API as shown below using the curl command. Curl Application can be downloaded from [here] (https://curl.haxx.se/download.html).
    ```
    curl -H "Content-Type: application/xml" --request PUT --data @data.xml http://127.0.0.1:8290/s3connector/createbucket
    ```
**Expected Response**:
    ```
    {
        "Result": {
            "MessageID": "projects/wso2-42209/messages/0:1542190904524195%2fd9afcdf9fd7ecd"
        }
    }
    ```
If you have registered some devices to your application, the notification will appear on that device. 

## What's Next

* You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).
* Please read the [Google Firebase Connector reference guide](google-firebase-configuration.md) to learn more about the operations you can perform with the connector.