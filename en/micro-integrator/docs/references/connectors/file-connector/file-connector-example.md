# File Connector Example

The File Connector allows you to connect to different file systems and perform various operations with the file systems. The file connector uses the Apache Commons VFS I/O functionalities to execute operations.

WSO2 EI File connector introduces the atomic operation related to the file system and allows you to easily manipulate files based on your requirement. The file streaming functionality using Apache Commons I/O lets you copy large files and reduces the file transfer time between two file systems resulting in a significant improvement in performance that can be utilized in file operations.

Using File Connector it can be perform operations in local file system as well as in a remote server such as FTP and SFTP. 

## What you'll build

This example explains how to use File Connector to create a file in the local file system and read the particular file. The user will send a payload specifying which content to be written to the file. Based on that content a file will be created in the specified location. Then it can be read the content of the file as an http response by invoking the other api resource upon the existence of the file. 

It will have two http api resources which are create and read. 

* /create : It will create a file with the content that the user specifies in the payload. 
<p><img src="../../../assets/img/connectors/FileConnector-03.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/></p>

* /read : It will first check if the file exists. If so it will read the content of the file. 
<p><img src="../../../assets/img/connectors/FileConnector-02.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/></p>

## Configure the connector in WSO2 Integration Studio

1. Follow these steps to set up the ESB Solution Project and the Connector Exporter Project. 
[Configure the Connector and the Connector Exporter Project](../configure-connector-exporter-project.md).

2. Right Click on the created ESB Solution Project -> New -> Rest API to create the REST API. 
<p><img src="../../../assets/img/connectors/adding-an-api.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/></p>

3. Provide the API name as File Connector and the api context as /fileconnector. You can go to the source view and copy paste the following api configuration. 
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

4. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Export tha carbon application to your desired location. 

## Deployment
### Micro Integrator

You can copy the carbon application to the <PRODUCT-HOME>/repository/deployment/server/carbonapps folder and start the server. 
Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```

1. Login.

    ```
    ./mi remote login
    ```

2. Provide default credentials admin for both username and password.
3. In order to view the APIs deployed, execute the following command.

    ```
    ./mi api show
    ```

### wso2-ei-6.6.0

1. You can copy the carbon application to the <PRODUCT-HOME>/repository/deployment/server/carbonapps folder and start the server. 

2. WSO2 EI server will be started and you can login to the Management Console https://localhost:9443/carbon/ URL. Provide login credentials. 

3. Under API section it can be seen that the API is deployed. 

## Testing

### File Create Operation

1. Create a file called data.json with the following payload. 
```
{
    "source":"/Users/IsuruUyanage/Desktop/RnD/2020/Q1/ConnectorVerification/create.txt",
    "inputContent": "This is a test file"
}
```
2. Invoke the API as below. 
```
curl -H "Content-Type: application/json" --request POST --data @body.json http://10.100.5.136:8290/fileconnector/create
```

### File Read Operation

1. Create a file called data.json with the following payload. 
```
{
    "source":"/Users/IsuruUyanage/Desktop/RnD/2020/Q1/ConnectorVerification/create.txt"
}
```
2. Invoke the API as below. 
```
curl -H "Content-Type: application/json" --request POST --data @body.json http://10.100.5.136:8290/fileconnector/read
```