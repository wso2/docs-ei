# File Connector Example

The File Connector allows you to connect to different file systems and perform various operations. The File Connector uses the [Apache Commons VFS](https://commons.apache.org/proper/commons-vfs/) I/O functionalities to execute operations.

WSO2 EI File Connector introduces the independent operations related to the file system and allows you to easily manipulate files based on your requirement. The file streaming functionality using Apache Commons I/O lets you copy large files and reduces the file transfer time between two file systems resulting in a significant improvement in performance that can be utilized in file operations.

File Connector can be used to perform operations in the local file system as well as in a remote server such as FTP and SFTP. 

## What you'll build

This example explains how to use File Connector to create a file in the local file system and read the particular file. The user sends a payload specifying which content is to be written to the file. Based on that content, a file is created in the specified location. Then the content of the file can be read as an HTTP response by invoking the other API resource upon the existence of the file. 

It will have two HTTP API resources, which are `create` and `read`. 

* `/create `: It will create a file with the content that the user specifies in the payload. 
    <p><img src="/assets/img/connectors/FileConnector-03.png" title="Adding a Rest API" width="800" alt="Adding a Rest API" /></p>

* `/read `: It will first check if the file exists. If so it will read the content of the file. 
    <img src="/assets/img/connectors/FileConnector-02.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/>

## Configure the connector in WSO2 Integration Studio

1. Follow these steps to set up the ESB Solution Project and the Connector Exporter Project. 
[Importing Connector to Integration Studio Solution Project](../importing-connector-to-integration-studio.md)

2. Right click on the created ESB Solution Project and select, -> **New** -> **Rest API** to create the REST API. 
    <img src="/assets/img/connectors/adding-an-api.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/>

3. Provide the API name as File Connector and the API context as `/fileconnector`. You can go to the source view of the xml configuration file of the API and copy the following configuration. 
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <api context="/fileconnector" name="FileConnector" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="POST" uri-template="/create">
            <inSequence>
                <property expression="json-eval($.source)" name="source" scope="default" type="STRING"/>
                <property expression="json-eval($.inputContent)" name="inputContent" scope="default" type="STRING"/>
                <fileconnector.create>
                    <source>{$ctx:source}</source>
                    <inputContent>{$ctx:inputContent}</inputContent>
                </fileconnector.create>
                <respond/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
        <resource methods="POST" uri-template="/read">
            <inSequence>
                <property expression="json-eval($.source)" name="source" scope="default" type="STRING"/>
                <fileconnector.isFileExist>
                    <source>{$ctx:source}</source>
                </fileconnector.isFileExist>
                <property expression="json-eval($.fileExist)" name="response" scope="default" type="STRING"/>
                <log level="custom">
                    <property expression="get-property('response')" name="responselog"/>
                </log>
                <switch source="get-property('response')">
                    <case regex="true">
                        <fileconnector.read>
                            <source>{$ctx:source}</source>
                        </fileconnector.read>
                        <respond/>
                    </case>
                    <default>
                        <log>
                            <property name="notext" value="&quot;File does not exist&quot;"/>
                        </log>
                    </default>
                </switch>
                <drop/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>

    ```

4. Follow these steps to export the artifacts. 
{! /references/connectors/exporting-artifacts.md !}

## Deployment
Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}

## Testing

### File Create Operation

1. Create a file called data.json with the following payload. 
    ```
    {
        "source":"/Users/IsuruUyanage/Desktop/RnD/2020/Q1/ConnectorVerification/create.txt",
        "inputContent": "This is a test file"
    }
    ```
2. Invoke the API as shown below using the curl command. Curl Application can be downloaded from [here] (https://curl.haxx.se/download.html).
    ```
    curl -H "Content-Type: application/json" --request POST --data @body.json http://10.100.5.136:8290/fileconnector/create
    ```
**Expected Response**: 
You should get a 'Success' response, and the file should be created in the specified location in the above payload. 

### File Read Operation

1. Create a file called data.json with the following payload. 
    ```
    {
        "source":"/Users/IsuruUyanage/Desktop/RnD/2020/Q1/ConnectorVerification/create.txt"
    }
    ```
2. Invoke the API as shown below using the curl command. Curl Application can be downloaded from [here] (https://curl.haxx.se/download.html).
    ```
    curl -H "Content-Type: application/json" --request POST --data @body.json http://10.100.5.136:8290/fileconnector/read
    ```

**Expected Response**: 
You should get the following text returned. 

`
This is a test file
`
