# Authenticating Salesforce using OAuth 2.0

This example shows you how to utilize the [Salesforce REST Connector](https://store.wso2.com/store/assets/esbconnector/details/43e44763-0d73-4ab3-8ae9-d6f73532d164) to connect to Salesforce using OAuth as the security protocol.

### Assumptions ###

This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/quick-start-guide/). To 
increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

In this example, Contacts in Salesforce are retrieved using OAuth 2.0 as the authentication mechanism. The retrieved records are then logged for simplicity.   

<img src="../../../../assets/img/migration-examples/authenticating-salesforce-using-oauth2-use-case.png" title="Authenticating Salesforce Using OAuth2 Use Case" width="800" alt="Authenticating Salesforce Using OAuth2 Use Case"/>

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).
2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.
3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.
4. Browse and select the file path to the downloaded sample of this Github project 
(`integration-studio-examples/migration/mule/authenticating-salesforce-using-oauth2/AuthenticatingSalesforceUsingOauth2Registry`) anf click **finish**.
5. Let's add the Salesforce REST connector into the workspace. Right click on the **AuthenticatingSalesforceUsingOauth2** and select **Add or Remove Connector**. Keep the **Add connector** option selected and click **Next>**. Search for 'file' using the search bar and click the download button located at the bottom right corner of the Salesforce REST connector. Click **Finish**.
6. Follow these [steps](https://ei.docs.wso2.com/en/latest/micro-integrator/references/connectors/salesforce-rest-connector/sf-access-token-generation/) to generate the Access Tokens for Salesforce and obtain the Access Token, and Refresh Token.
7. Open the **ContactsAPI.xml** under **authenticating-salesforce-using-oauth2/AuthenticatingSalesforceUsingOauth2/src/main/synapse-config/api/** directory.<br> 
    <img src="../../../../assets/img/migration-examples/authenticating-salesforce-using-oauth2.png" title="Authenticating Salesforce Using OAuth2" width="800" alt="Authenticating Salesforce Using OAuth2"/>
    Configure the following properties with the previously obtained values.
    - Access Token
    - Refresh Token
    - API URL (e.g.: https://<INSTANCE>.salesforce.com)
8. Run the sample by right click on the **AuthenticatingSalesforceUsingOauth2CompositeApplication** under the main **authenticating-salesforce-using-oauth2** project and selecting **Export Project Artifacts and Run**.
9. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.
9. Make a GET request to `http://localhost:8290/contacts`.
10. Following logs can be observed in the console log.
    ```
    [2020-04-24 16:28:31,381]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00003JxgH6AAJ", "LastName": "Pattinson", "LastModifiedDate": "2019-12-09T11:59:19.000+0000"}
    [2020-04-24 16:28:31,382]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00003JxgH6AAJ", "LastName": "Pattinson", "LastModifiedDate": "2019-12-09T11:59:19.000+0000"}
    [2020-04-24 16:28:31,383]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00003JyBJaAAN", "LastName": "Smith", "LastModifiedDate": "2019-12-10T10:41:36.000+0000"}
    [2020-04-24 16:28:31,383]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00003JyBJaAAN", "LastName": "Smith", "LastModifiedDate": "2019-12-10T10:41:36.000+0000"}
    [2020-04-24 16:28:31,384]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00003JyBJbAAN", "LastName": "Foe", "LastModifiedDate": "2019-12-10T10:41:36.000+0000"}
    [2020-04-24 16:28:31,384]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00003JyBJbAAN", "LastName": "Foe", "LastModifiedDate": "2019-12-10T10:41:36.000+0000"}
    [2020-04-24 16:28:31,385]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hSAAQ", "LastName": "Gonzalez", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,385]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hSAAQ", "LastName": "Gonzalez", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,386]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hTAAQ", "LastName": "Forbes", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,386]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hTAAQ", "LastName": "Forbes", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,387]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hUAAQ", "LastName": "Rogers", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,387]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hUAAQ", "LastName": "Rogers", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,388]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hVAAQ", "LastName": "Stumuller", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,388]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hVAAQ", "LastName": "Stumuller", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,390]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hWAAQ", "LastName": "Young", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,390]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hWAAQ", "LastName": "Young", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,391]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hXAAQ", "LastName": "Barr", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,391]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hXAAQ", "LastName": "Barr", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,392]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hYAAQ", "LastName": "Bond", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    [2020-04-24 16:28:31,392]  INFO {API_LOGGER.ContactsAPI} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:2280a0c3-4ffe-47ca-a727-21424e531b0f, Direction: request, Payload: {"Id": "0032v00002m23hYAAQ", "LastName": "Bond", "LastModifiedDate": "2019-06-19T08:44:05.000+0000"}
    ```

### Download the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../../assets/attach/connectors/FileConnector.zip">
    <img src="../../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about [Salesforce REST connector](https://docs.wso2.com/display/ESBCONNECTORS/Salesforce+REST+Connector).
* Read more on [WSO2 connectors](https://docs.wso2.com/display/ESBCONNECTORS/WSO2+ESB+Connectors+Documentation)
