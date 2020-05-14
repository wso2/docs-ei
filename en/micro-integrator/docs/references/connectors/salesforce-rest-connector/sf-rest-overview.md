# Salesforce REST Connector Overview

This documentation provides overall mapping with each sections in Salesforce connector and a inbound endpoint documentation. 

The **Salesforce Connector** uses the [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm) to interact with Salesforce and **Salesforce Inbound Endpoint**  uses the [Salesforce streaming API](https://developer.salesforce.com/docs/atlas.en-us.api_streaming.meta/api_streaming/intro_stream.htm) to receives notifications. 

Click the links of the shown below to view the documentation for Salesforce REST Connector and Salesforce Inbound Endpoint. In these examples describe how to use each connector and inbound endpoint operations with real world scenarios. 

## Salesforce REST Connector

The Salesforce REST Connector allows you to work with records in Salesforce, a web-based service that allows organizations to manage contact relationship management (CRM) data. You can use the Salesforce connector to create, query, retrieve, update, and delete records in your organization's Salesforce data.

* **[Salesforce Access Token Generation](sf-access-token-generation.md)**

  This section includes how to obtain the OAuth2 tokens from Salesforce REST API.

* **[Salesforce Rest API Connector Example](sf-rest-connector-example.md)**

  This example explains how to use the Salesforce client to connect with the Salesforce instance and perform the **create** and **retrive** operations.

* **[Salesforce Rest API Connector Configuration](sf-rest-connector-config.md)**

  This documentation provides a reference guide for the Salesforce Rest API operations.
  
## Salesforce Inbound Endpoint  

The Salesforce Inbound Endpoint receives notifications based on the changes that happen to Salesforce data with respect to an SQQL (Salesforce Object Query Language) query you define, in a secured and scalable way.

* **[Setting up the PushTopic in Salesforce](sf-rest-inbound-endpoint-configuration.md)**

  This documentation explains how to set up the Salesforce environment to connect with WSO2 Salesforce Inbound Endpoint. 

* **[Salesforce Inbound Endpoint Example](sf-rest-inbound-endpoint-example.md)**

  This example explains how to Salesforce Inbound Endpoint act as a message consumer. WSO2 EI is a listening inbound endpoint that can consume messages from Salesforce. 

* **[Salesforce Inbound Endpoint Configuration](sf-rest-inbound-endpoint-reference-configuration.md)**

  This documentation provides a reference guide for the Salesforce Inbound Endpoint.
