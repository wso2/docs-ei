# Salesforce Rest API Connector Example

The Salesforce REST Connector allows you to work with records in Salesforce, a web-based service that allows organizations to manage contact relationship management (CRM) data. You can use the Salesforce connector to create, query, retrieve, update, and delete records in your organization's Salesforce data. The connector uses the [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm) to interact with Salesforce.

## What you'll build

This example explains how to use the Salesforce client to connect with the Salesforce instance and perform the 
following operations:

* Create an Account 
* Execute a SOQL query to retrieve the Account Name and ID in all the existing Accounts. 

<img src="/assets/img/connectors/Salesforce.png" title="Using Salesforce Rest Connector" width="800" alt="Using Salesforce Rest Connector"/>

The user calls the Salesforcerest API. It invoke the **create** sequence and create a new account in Salesforce. Then through the **retrieve** sequence, it displays all the existing account details to the user. 

If you do not want to build this yourself, you can simply [get the project](#get-the-project) and run it.

## Configure the connector in WSO2 Integration Studio
Follow these steps to set up the ESB Solution Project and the Connector Exporter Project. 

{!references/connectors/importing-connector-to-integration-studio.md!} 

1. Right click on the created ESB Solution Project and select, -> **New** -> **Rest API** to create the REST API. 
    <img src="/assets/img/connectors/adding-an-api.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/>

2. Follow these steps to [generate the Access Tokens for Salesforce](../salesforce-connector/sf-access-token-generation.md) and obtain the Client Id, Client Secret, Access Token, and Refresh Token. 

3. First, we will create two defined sequences called `create.xml` and  `retrieve.xml` to create an account and retrieve data. You can go to the source view of the XML configuration file of the API and copy the following configuration. Replace the `clientSecret`, `clientId`, `accessToken`, `refreshToken` with obtained values in step 3. 

    ```
    create.xml

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

    ```
    retrieve.xml

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

4. Now create the API **salesforcerest**. You can go to the source view of the XML configuration file of the API and copy the following configuration. 
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
