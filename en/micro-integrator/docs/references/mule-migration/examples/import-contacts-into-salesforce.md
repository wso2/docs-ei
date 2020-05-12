---
title: Import Contacts Into Salesforce
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This example shows you how to utilize the [File Connector](https://store.wso2.com/store/assets/esbconnector/details/5d6de1a4-1fa7-434e-863f-95c8533d3df2) and [Salesforce REST Connector](https://store.wso2.com/store/assets/esbconnector/details/43e44763-0d73-4ab3-8ae9-d6f73532d164) to import data in a file to Salesforce.

### Assumptions ###

This document assumes that you are familiar with WSO2 EI and the 
[Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/quick-start-guide/). To 
increase your familiarity with Integration Studio, consider completing one or more 
[WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

### Example Use Case
In this example, Contacts in a csv file is being imported to Salesforce using the File Connector and the Salesforce Connector. The response from Salesforce is then logged to track the status of creating each record.   

### Set Up and Run the Example

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).
2. In your menu in Studio, click the **File** menu. In the File menu select the **Import...** item.
3. In the Import window select the **Existing WSO2 Projects into workspace** under **WSO2** folder.
4. Browse and select the file path to the downloaded sample of this github project 
("import-contacts-into-salesforce" folder of the downloaded github repository).
5. Lets add the file connector into the workspace. Right click on the **ImportContactsIntoSalesforce** and select 
**Add or Remove Connector**. Keep the **Add connector** option selected and click **Next>**. Search for 'file' using the 
search bar and click the download button located at the bottom right corner of the file connector. Click **Finish**.
6. Similarly, add the Salesforce REST Connector to the workspace.
7. Follow these [steps](https://ei.docs.wso2.com/en/latest/micro-integrator/references/connectors/salesforce-rest-connector/sf-access-token-generation/) to generate the Access Tokens for Salesforce and obtain the Access Token, and Refresh Token.
8. Open the **Sequence.xml** under 
**import-contacts-into-salesforce/ImportContactsIntoSalesforce/src/main/synapse-config/sequences/** directory. 
Configure the following properties with the previously obtained values under **<salesforcerest.init>**.
    - Access Token
    - Refresh Token
    - API URL (e.g.: https://<INSTANCE>.salesforce.com)
9. Copy the **contacts.csv** file in **import-contacts-into-salesforce/ImportContactsIntoSalesforce/src/main/resources/** to a location on your computer. 
Provide the path to this as the source under **<fileconnector.read>**
E.g.:
```xml
<source>file:///Users/testuser/Desktop/contacts.csv</source>
```
10. Run the sample by right clicking on the **import-contacts-into-salesforce** project and selecting **Run as -> Run On Micro Integrator**.
11. Following logs can be observed in the console log.
```
[2020-04-28 11:18:16,994]  INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: http://www.w3.org/2005/08/addressing/anonymous, WSAction: , SOAPAction: , MessageID: urn:uuid:9f36fa4b-a851-45a1-945a-afdbd9f004ad, Direction: request, Payload: {"hasErrors":false,"results":[{"referenceId":"1","id":"0032v00003Un7V6AAJ"},{"referenceId":"2","id":"0032v00003Un7V7AAJ"}]}

```
12. You may verify whether the records were successfully added to salesforce by visiting Salesforce. Then, navigate to **Contacts** tab and use the drop-down menu to display All Contacts.
Scan your contacts for two new entries: John Doe, Jane Doe
                                                                                                                                                                                                                                        
### How It Works

This application accepts CSV files which contain contact information, then uploads the contacts to Salesforce.

A scheduler task will run for every ten seconds and execute the sequence configured in Sequence.xml. 

In this sequence, initially, file connector will read the file specified as the source. A datamapper is then used to map the read CSV content to the JSON structure Salesforce REST connector expects. 
Then the mapped content is passed to the Salesforce connector to create multiple records.

### Go Further

* Learn more about [File Connector](https://docs.wso2.com/display/ESBCONNECTORS/Working+with+the+File+Connector#WorkingwiththeFileConnector-append).
* Learn more about [Salesforce REST connector](https://docs.wso2.com/display/ESBCONNECTORS/Salesforce+REST+Connector).
* Read more on [WSO2 connectors](https://docs.wso2.com/display/ESBCONNECTORS/WSO2+ESB+Connectors+Documentation)
