# Salesforce Connector Example

The Salesforce REST Connector allows you to work with records in Salesforce, a web-based service that allows organizations to manage contact relationship management (CRM) data. You can use the Salesforce connector to create, query, retrieve, update, and delete records in your organization's Salesforce data. The connector uses the [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm) to interact with Salesforce.

## What you'll build

This example explains how to use the Salesforce client to connect with the Salesforce instance and perform the 
following operations:

* Create Account with Contacts and Opportunities
* Execute a SOQL query

The following diagram illustrates all the required functionality of the Salesforce Service that you are going to build.

<p><img src="/assets/img/connectors/working-with-sf-client.png" title="Working with Salesforce Client" width="800" alt="Working with Salesforce Client" /></p>

To implement this, you need to do the following.

1. [Generate Salesforce access tokens](../sf-access-token-generation.md).
2. Configure the connector in WSO2 Integration Studio.

## Configure the connector in WSO2 Integration Studio

1. Open WSO2 Integration Studio and create an ESB Solution Project.
2. Our project would look similar to the following (source view).

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <api context="/salesforcerest" name="salesforcerest" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="POST" uri-template="/themecolor">
            <inSequence>
                <property expression="json-eval($.accessToken)" name="accessToken" scope="default" type="STRING"/>
                <property expression="json-eval($.apiUrl)" name="apiUrl" scope="default" type="STRING"/>
                <property expression="json-eval($.clientId)" name="clientId" scope="default" type="STRING"/>
                <property expression="json-eval($.refreshToken)" name="refreshToken" scope="default" type="STRING"/>
                <property expression="json-eval($.clientSecret)" name="clientSecret" scope="default" type="STRING"/>
                <property expression="json-eval($.hostName)" name="hostName" scope="default" type="STRING"/>
                <property expression="json-eval($.apiVersion)" name="apiVersion" scope="default" type="STRING"/>
                <property expression="json-eval($.registryPath)" name="registryPath" scope="default" type="STRING"/>
                <property expression="json-eval($.intervalTime)" name="intervalTime" scope="default" type="STRING"/>
                <salesforcerest.init>
                    <accessToken>{$ctx:accessToken}</accessToken>
                    <apiVersion>{$ctx:apiVersion}</apiVersion>
                    <hostName>{$ctx:hostName}</hostName>
                    <refreshToken>{$ctx:refreshToken}</refreshToken>
                    <clientSecret>{$ctx:clientSecret}</clientSecret>
                    <clientId>{$ctx:clientId}</clientId>
                    <apiUrl>{$ctx:apiUrl}</apiUrl>
                    <registryPath>{$ctx:registryPath}</registryPath>
                    <intervalTime>{$ctx:intervalTime}</intervalTime>
                </salesforcerest.init>
                <log level="full"/>
                <salesforcerest.themes/>
                <respond/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>

    ```

3. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.
4. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```

## Testing

1. Login.

    ```
    ./mi remote login
    ```

2. Provide default credentials admin for both username and password.
3. In order to view the APIs deployed, execute the following command.

    ```
    ./mi api show
    ```

## Deployment

You can deploy your project on a container-based environment like [Kubernetes](../../setup/deployment/kubernetes_deployment.md).
