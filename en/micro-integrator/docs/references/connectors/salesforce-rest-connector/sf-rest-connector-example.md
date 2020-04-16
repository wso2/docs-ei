# Salesforce Rest API Connector Example
salesforce
The Salesforce REST Connector allows you to work with records in Salesforce, a web-based service that allows organizations to manage contact relationship management (CRM) data. You can use the Salesforce connector to create, query, retrieve, update, and delete records in your organization's Salesforce data. The connector uses the [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm) to interact with Salesforce.

## What you'll build

This example explains how to use the Salesforce client to connect with the Salesforce instance and perform the 
following operations:

* Create an Account.

  The user sends the request payload which includes with `sObjects` (Any object that can be stored in the Lightning platform database), to create a new Account object in the sales force. This request is sent to WSO2 EI by invoking the Salesforce connector API. 

* Execute a SOQL query to retrieve the Account Name and ID in all the existing Accounts.

  In this example use the Salesforce Object Query Language (SOQL) to search stored Salesforce data for specific information which is created under `sObjects`. 

<img src="../../../../assets/img/connectors/Salesforce.png" title="Using Salesforce Rest Connector" width="800" alt="Using Salesforce Rest Connector"/>

The user calls the Salesforcerest API. It invoke the **create** sequence and create a new account in Salesforce. Then through the **retrieve** sequence, it displays all the existing account details to the user. 

If you do not want to build this yourself, you can simply [get the project](#get-the-project) and run it.

## Configure the connector in WSO2 Integration Studio
Follow these steps to set up the ESB Solution Project and the Connector Exporter Project. 

{!references/connectors/importing-connector-to-integration-studio.md!} 

## Adding integration logic

1.  Creating the API

    Right click on the created Integration Project and select, -> **New** -> **Rest API** to create the REST API. Specify the API name as `salesforcerest` and API context as `/salesforcerest`.
    
    <img src="../../../../assets/img/connectors/adding-an-api.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/>

2. Creating the sequence to create salesforce object.
   
    We will create two defined sequences called `create.xml` and  `retrieve.xml` to create an account and retrieve data. Right click on the created Integration Project and select, -> **New** -> **Sequence** to create the Sequence. 
  
    <img src="../../../../assets/img/connectors/add-sequence.png" title="Adding a Sequnce" width="800" alt="Adding a Sequnce"/>
    
    2.1 Initialize the connector
    
    Follow these steps to [generate the Access Tokens for Salesforce](sf-access-token-generation.md) and obtain the Client Id, Client Secret, Access Token, and Refresh Token.
    
    Navigate into the **Palette** pane and select the graphical operations icons listed under **Salesforce Connector** section. Then drag and drop the `init` operation into the Design pane.
        
    <img src="../../../../assets/img/connectors/salesforce-drag-and-drop-init.png" title="Drag and drop init operation" width="800" alt="Drag and drop init operation"/>
        
    Add the property values into the `init` operation as shown bellow. Replace the `clientSecret`, `clientId`, `accessToken`, `refreshToken` with obtained values from above steps.
      
    - **clientSecret** : Value of your client secret given when you registered your application with Salesforce.
    - **clientId** : Value of your client ID given when you registered your application with Salesforce.
    - **accessToken** : Value of the access token to access the API via request.
    - **refreshToken** : Value of the refresh token.
       
    <img src="../../../../assets/img/connectors/salesforce-api-init-operation-sequnce.png" title="Add values to the init operation" width="800" alt="Add values to the init operation"/>
    
    The above four parameters are saved to the properties section and add the values into the `init` operation.
    
    <img src="../../../../assets/img/connectors/salesforce-api-create-operation-sequnce.png" title="Add values to the create operation" width="800" alt="Add values to the create operation"/>
     
    2.2 Setting up the created operation

    Then we will setup the `create` sequence configurations. In this operation we are going to create a `sObjects` in the Salesforce account. An `SObject` represents a specific table in the database that you can discretely query. It describes the individual metadata for the specified object. Please find the `create` operation parameters listed here.
       
    - **sObjectName** : Name of the sObject which we need to create in the salesforce.
    - **fieldAndValue** : The field and value need to store in the created salesforce sObject.
        
    While invoking the api the above two parameters values are coming as a user input.
    
    Navigate into the **Palette** pane and select the graphical operations icons listed under **Salesforce Connector** section. Then drag and drop the `create` operation into the Design pane.
    
     <img src="../../../../assets/img/connectors/salesforce-drag-and-drop-create.png" title="Drag and drop create operation" width="800" alt="Drag and drop create operations"/>
    
    To get the input values in to the API we can use the [property mediator](https://ei.docs.wso2.com/en/next/micro-integrator/references/mediators/property-Mediator/) .

    Navigate into the **Palette** pane and select the graphical mediators icons listed under **Mediators** section. Then drag and drop the `Property` mediators into the Design pane as shown bellow.
    
    <img src="../../../../assets/img/connectors/salesforce-api-drag-and-drop-property-mediator.png" title="Add property mediators" width="800" alt="Add property mediators"/>

    The parameters available for configuring the Property mediator are as follows:
    
    > **Note**: That the properties should be add to the pallet before create the operation.
    
    Add the property mediator to capture the `sObjectName` value.The sObjectName type can be used to retrieve the metadata for the Account object using the GET method, or create a new Account object using the POST method. In this example we are going to create a new Account object using the POST method.
   
    - **name** : sObjectName
    - **expression** : json-eval($.sObject)
    - **type** : STRING
   
    <img src="../../../../assets/img/connectors/salesforce-api-property-mediator-values1.png" title="Add values to capture sObjectName value" width="800" alt="Add values to capture sObjectName value"/>
    
    Add the property mediator to capture the `fieldAndValue` values. The fieldAndValue contains object fields and values that user need to store.
   
    - **name** : fieldAndValue
    - **expression** : json-eval($.fieldAndValue)
    - **type** : STRING
     
    <img src="../../../../assets/img/connectors/salesforce-api-property-mediator-values2.png" title="Add values to capture fieldAndValue value" width="800" alt="Add values to capture fieldAndValue value"/>  
    
3. Creating the sequence to retrive the salesforce objects created

    3.1 Initialize the connector
    
    You can use the generated tokens to initialize the connector. Please follow the steps  given in 2.1 for setting up the `init` operation to the `retrive.xml` sequence. 
    
    3.2 Setting up the retrieve operation

    For retrieve data from the created objects in the salesforce account,you need to add the `query` operation to the `retrieve` sequence. 
    
    - **queryString** :  This variable contains specified SOQL query. In this sample this SOQL query executes to retrive `id` and `name` from created `Account`. If the query results are too large, the response contains the first batch of results.
                                                                                                                                                                     
    Navigate into the **Palette** pane and select the graphical operations icons listed under **Salesforce Connector** section. Then drag and drop the `query` operations into the Design pane.      
    
    <img src="../../../../assets/img/connectors/salesforce-drag-and-drop-query.png" title="Add query operation to retrive sequnce" width="800" alt="Add query operation to retrive sequnce"/> 
    
    Select the query operation and add `id, name from Account` query to the properties section shown as bellow.
    
    <img src="../../../../assets/img/connectors/salesforce-api-retrive-query-operation-sequnce.png" title="Add query to the query operation in retrive sequnce" width="800" alt="Add query to the query operation in retrive sequnce"/>
    
4. Configure the `salesforcerest API` using the created `create` and `retrive` sequences.
 
    Now you can select the API which we created in step 1. Navigate into the **Palette** pane and select the graphical operations icons listed under **Defined Sequences** section. Then drag and drop the created `create` and `retrive` sequences to the Design pane.
    
    <img src="../../../../assets/img/connectors/salesforce-drag-and-drop-sequencestothe-DesignPane.png" title="Drag and drop sequences to the Design view" width="800" alt="Drag and drop sequences to the Design view"/> 
 
5. Getting response to the user
    
    When you invoking the created API the request of the message is going through the `create` and `retrive` sequences. Finally pass to the the [Respond mediator](https://ei.docs.wso2.com/en/next/micro-integrator/references/mediators/respond-Mediator/). In here the Respond Mediator stops the processing on the current message and sends the message back to the client as a response.            
    
    Drag and drop **respond mediator** to the **Design view**. 
    
    <img src="../../../../assets/img/connectors/salesforce-drag-and-drop-respond-mediator.png" title="Add Respond mediator" width="800" alt="Add Respond mediator"/> 

    Once you have setup the sequences and API, you can see the `salesforcerest` API as following manner.
    
    <img src="../../../../assets/img/connectors/salesforce-api-design-view.png" title="API Design view" width="800" alt="API Design view"/>
       
6.  Now you can switch into the Source view and check the XML configuration files of the created API and sequences. 
   
   **create.xml**
   
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <sequence name="create" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
        <property expression="json-eval($.sObject)" name="sObject" scope="default" type="STRING"/>
        <property expression="json-eval($.fieldAndValue)" name="fieldAndValue" scope="default" type="STRING"/>
        <salesforcerest.init>
            <accessToken></accessToken>
            <apiVersion>v44.0</apiVersion>
            <hostName>https://login.salesforce.com</hostName>
            <refreshToken></refreshToken>
            <clientSecret></clientSecret>
            <clientId></clientId>
            <apiUrl>https://ap16.salesforce.com</apiUrl>
            <registryPath>connectors/SalesforceRest</registryPath>
        </salesforcerest.init>
        <salesforcerest.create>
            <sObjectName>{$ctx:sObject}</sObjectName>
            <fieldAndValue>{$ctx:fieldAndValue}</fieldAndValue>
        </salesforcerest.create>
    </sequence>
    ```
   
   **retrieve.xml**

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <sequence name="retrieve" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
        <salesforcerest.init>
            <accessToken></accessToken>
            <apiVersion>v44.0</apiVersion>
            <hostName>https://login.salesforce.com</hostName>
            <refreshToken></refreshToken>
            <clientSecret></clientSecret>
            <clientId></clientId>
            <apiUrl>https://ap16.salesforce.com</apiUrl>
            <registryPath>connectors/SalesforceRest</registryPath>
        </salesforcerest.init>
        <salesforcerest.query>
            <queryString>select id, name from Account</queryString>
        </salesforcerest.query>
    </sequence>
    ```

   **salesforcerest.xml**

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <api context="/salesforcerest" name="salesforcerest" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="POST">
            <inSequence>
                <sequence key="create"/>
                <sequence key="retrieve"/>
                <respond/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>
    ```
## Get the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../../assets/attach/connectors/salesforceRest.zip">
    <img src="../../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

## Deployment

Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}

## Testing
Save a file called data.json with the following payload. 
```
{
	"sObject":"Account",
	"fieldAndValue": {
    "name": "Engineers",
    "description":"This Account belongs to WSO2"
  }
}
```

Invoke the API as shown below using the curl command. Curl application can be downloaded from [here](https://curl.haxx.se/download.html).


```
curl -X POST -d @data.json http://localhost:8280/salesforcerest --header "Content-Type:application/json"
```

You will get a set of account names and the respective IDs as the output.

## What's Next

* You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).
