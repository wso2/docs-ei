# File Connector Example

The File Connector allows you to connect to different file systems and perform various operations with the file systems. The file connector uses the Apache Commons VFS I/O functionalities to execute operations.

WSO2 EI File connector introduces the atomic operation related to the file system and allows you to easily manipulate files based on your requirement. The file streaming functionality using Apache Commons I/O lets you copy large files and reduces the file transfer time between two file systems resulting in a significant improvement in performance that can be utilized in file operations.

## What you'll build

This example explains how to use File Connector to create a file in the local file system and read the particular file. 

It will have two api resources which are /create and /read. 

* /create : It will create a file with the content that the user specifies in the payload. 

* /read : It will first check if the file exists. If so it will read the content of the file. 

## Configure the connector in WSO2 Integration Studio

1. Open WSO2 Integration Studio and create an ESB Solution Project.
2. Right click on the project that you created and click on 'Add or Remove Connector' -> Add Connector. 
3. It will be navigated to the WSO2 Connector Store. 
4. Search for File Connector and download it. 
5. Now the File Connector is added to the left side palette when you go to the design view of a synapse configuration. 
6. Create the following REST API with two api resources. 
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
7. Now it needs to add a Connector Exporter Project. 
8. Go to File -> New -> Other -> WSO2 -> Extensions -> Project Types -> Connector Exporter Project
9. Provide a name for Connector Exporter Project and in the next screen tick on 'Specify the parent from workspace' and select the File Connector project. 
10. Now it needs to add the Connector to Connector Exporter project that we just created. Right Click on the Connector Exporter project -> New -> Add Remove Connectors -> Add Connector -> Add from Workspace -> File Connector
11. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.
12. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

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

4. Invoke the API with the following payload. 
URL: http://localhost:8290/fileconnector/create
```
{
     "source":"../createfile/create.txt",
     "inputContent": "This is a test file"
}
```

URL: http://localhost:8290/fileconnector/read
```
{
     "source":"../createfile/create.txt"
}
```