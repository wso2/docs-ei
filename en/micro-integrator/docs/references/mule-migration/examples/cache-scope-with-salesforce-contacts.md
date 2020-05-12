---
title: Cache Scope With Salesforce Contacts
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example illustrates the basic concept of data caching using Salesforce.

### Assumptions ###

This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/quick-start-guide/). To 
increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case

The very basic concept of caching consists of temporary storing a result of some operations, in our use case it is caching fetched data from Salesforce. The example illustrates caching when we ask for a data once again, the result is returned directly from cache instead of making a new call to Salesforce.

<p align="center">
  <img width="90%" src="../../../assets/img/migration-examples/cache-scope-with-salesforce-contacts-use-case.png">
</p>

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.

3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.

4. Browse and select the file path to the downloaded sample of this Github project (`integration-studio-examples/migration/mule/cache-scope-with-salesforce-contacts/CacheScopeWithSalesforceContactsRegistry`) anf click **finish**.

5. Let's add the Salesforce REST connector into the workspace. Right click on the **CacheScopeWithSalesforceContacts** and select **Add or Remove Connector**. Keep the **Add connector** option selected and click **Next>**. Search for 'salesforce' using the search bar and click the download button located at the bottom right corner of the Salesforce REST connector. Click **Finish**.

6. Follow these [steps](https://ei.docs.wso2.com/en/latest/micro-integrator/references/connectors/salesforce-rest-connector/sf-access-token-generation/) to generate the Access Tokens for Salesforce and obtain the Access Token, and Refresh Token.

7. Open the **ContactsAPI.xml** under **cache-scope-with-salesforce-contacts/CacheScopeWithSalesforceContacts/src/main/synapse-config/api/** directory. 
    Configure the following properties with the previously obtained values.
    - Access Token
    - Refresh Token
    - API URL (e.g.: https://<INSTANCE>.salesforce.com)

    <img width="100%" src="../../../assets/img/migration-examples/cache-scope-with-salesforce-contacts.png">

8. Run the sample by right click on the **CacheScopeWithSalesforceContactsCompositeApplication** under the main 
**cache-scope-with-salesforce-contacts** project and selecting **Export Project Artifacts and Run**.

9. Open HTTP Client in Integration Studio. Follow [HTTP Client Guidelines](../../../docs/common/adding-http-client-to-integration-studio.md) to open HTTP Client if the window is not visible in the interface.

9. Make a GET request to `http://localhost:8290/contacts`.

10. Following logs can be observed in the console log. The first time, the request is made, Salesforce will be invoked to retrieve the data as it is not yet cached. 
    ```
    [2020-04-29 16:39:28,351]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - Fetch Contacts = Querying from Salesforce...
    ```

11. If you make another request to the same endpoint, the following log is observed. Since the data was cached in the previous call, this time the response is returned from the cache instead of fetching from Salesforce again.
    ```
    [2020-04-29 16:39:34,892]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - Fetch Contacts = Querying from cache...
    ```

### How it Works

This application queries Salesforce for contacts and responds them back to the client. From the console log you can simply check when caching is performed.

Salesforce Connector performs a query in your Salesforce Account: to get Id, FirstName, LastName, Email, Phone attributes of Contact objects with limit set to 200 records. To keep the example simple, the caching is illustrated via logs in console and Contacts are displayed in JSON format in the response.

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/cache-scope-with-salesforce-contacts.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about [Salesforce REST connector](https://docs.wso2.com/display/ESBCONNECTORS/Salesforce+REST+Connector).
* Read more on [WSO2 connectors](https://docs.wso2.com/display/ESBCONNECTORS/WSO2+ESB+Connectors+Documentation)
