---
title: Exposing a RESTful resource using the HTTP connector Example
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example application illustrates how to use the WSO2 EI to expose a RESTful resource using the REST APIs. After reading this document, and creating and running the example in WSO2, you should be able to leverage what you have learned to create a simple HTTP request-response application that can expose a RESTful resource by providing different verbs (HTTP methods) using JSON data.

### Assumptions

This document describes the details of the example within the context of Integration Studio, WSO2 EI’s graphical user interface (GUI). This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/). To increase your familiarity with Integration Studio, consider completing one or more [Integration Studio Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example, a user calls the WSO2 application by submitting a request via a REST client (i.e., entering a specific URL, http://localhost:8081/person/1). The application receives the request and processes it based on the URL. The application is capable of retrieving and inserting person data.  

To send this request, use a browser extension such as [Advanced Rest Client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo) (Google Chrome), or the [curl](http://curl.haxx.se/) command-line utility.  


### Set Up and Run the Example

You can tweak the configurations of these use case-based examples to create your customized applications in WSO2.

Follow the procedure below to create, then run the HTTP with JSON application.

1. Download the GitHub Project and import the project to Integration Studio (File-> Import [WSO2-> Existing WSO2 projects into workspace])
2. In the Package Explorer in Studio, right-click the project name, then select Run As > Run on Micro Integrator. The studio runs the application and WSO2 EI is up and kicking!
3. Send a POST request to **http://localhost:8290/person** with the body equal to:
```json 
{
   "firstname":"John",
   "lastname":"Doe",
   "age":"12",
   "address":{
      "streetAddress":"Lincoln St.",
      "city":"San Francisco",
      "state":"CA",
      "zipCode":"90401"
   }
}
```

 You should receive a response saying a person was created successfully. For example: 
```json
{
   "personId":"1",
   "message":"Person was created"
}
```
4. Send a GET request to **http://localhost:8290/person**.
 You should receive a response with all created persons.
5. Take the value of `personId` from the previous response and send a GET request to **http://localhost:8290/person/{personId}**
 You should recieve a response with the person data:
```json
{
   "personId":"1",
   "firstname":"John",
   "lastname":"Doe",
   "address":{
      "streetAddress":"Lincoln St.",
      "city":"San Francisco",
      "state":"CA",
      "zipCode":"90401"
   },
   "age":12
}
```

### How it Works

The **Exposing a RESTful resource using the HTTP connector** example application consists of three, simple API Resources(https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/creating-an-api/) which receive end-user HTTP requests, set payloads based on the request paths, and send data and return the payloads to end-users as HTTP responses.

The sections below elaborate further on the configurations of the application and how it works to respond to end-user requests.

### POST Request Handling (1st Resource)

When an end user's POST request reaches the application, first it will come to REST API's first resource(Which handles POST requests) [https://ei.docs.wso2.com/en/latest/micro-integrator/develop/creating-artifacts/creating-an-api/), which listens to POST requests at http://localhost:8290/person. 

Next, the PayloadFactory create the following message to send back to the client: 
```json
{ "personId": "1", "message": "Person was created"}
```

Finally, WSO2 EI sends the message back to the client using [respond mediator](https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/respond-Mediator/).

### GET /person/{personId} Resource (2nd Resource)

This resource is responsible for retrieving a specific person's data, based on the provided person ID. It starts with a WSO2 API Resource, which listens at http://localhost:8290/person/{personId}.

Next, the flow will create a mock person detail using the PayloadFactory mediator and send the response using Respond mediator.
**Note** this sample is only sending a mock response with the peronId path parameter as the ID
    

### GET Resource for /person (3rd Resource)

When an end-user request reaches the application for http://localhost:8290/person the 3rd Resource of the API will get invoked.

Next, the resource creates a dummy payload with a set of two-person details using (PayloadFactory mediator)[https://ei.docs.wso2.com/en/latest/micro-integrator/references/mediators/payloadFactory-Mediator/]

Finally, the response is sent back to the client using the Respond Mediator.
**Note** this sample is only sending a mock response with pre-defined person details

  
### Go Further

- Learn more about the [HTTP REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/).



