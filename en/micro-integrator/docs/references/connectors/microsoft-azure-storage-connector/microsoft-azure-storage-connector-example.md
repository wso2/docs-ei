# Microsoft Azure Storage Connector

The Microsoft Azure Storage Connector allows you to access the Azure Storage services using Microsoft Azure Storage Java SDK through WSO2 EI. [Azure Storage](https://azure.microsoft.com/en-us/) is a Microsoft-managed cloud service that provides storage that is highly available, secure, durable, scalable and redundant. The Azure Storage consists of four primary Azure Storage types. They are blob storage, table storage, file storage, queue storage.

Given below is a sample scenario that demonstrates how working with containers and blobs operations using the WSO2 Microsoft Azure Storage Connector.

## What you'll build

This example demonstrates how to use Microsoft Azure Storage connector to:

1. Create a container (a location for storing employee details) in Microsoft Azure Storage account.
2. Retrieve information about the created containers.
3. Upload text or binary employee details (blob) in to the container.
4. Retrieve information about the uploaded employee details(blob).
5. Remove uploaded employee details (blob).
6. Remove created container.

All six operations are exposed via an API. The API with the context `/resources` has six resources  

* `/createcontainer` : Creates a new container in the Microsoft Azure Storage account with the specified container name for store employee details.
* `/listcontainer` : Retrieve information about the created containers from the Microsoft Azure Storage account.
* `/adddetails`: Upload text or binary employee data (blob) and stored into the specified container.
* `/listdetails` : Retrieve information about the added employee deta (blobs).
* `/deletedetails` : Remove added employee data from the specified text or binary employee data (blob).
* `/deletecontainer` : Remove created container in the Microsoft Azure Storage account.

To know the further information about these operations please refer the [Microsoft Azure Storage connector reference guide](microsoft-azure-storage-connector-reference.md).

> **Note**: Before invoke the API you need to create a **Storage Account** in **Microsoft Azure Storage account**, see [Azure Storage Configuration](microsoft-azure-storage-configuration.md) documentation.

Following diagram shows the overall solution. User creates a container, stores some text or binary employee data (blob) into the container and then he receives it back. To invoke each operation user uses the same API. 

<img src="/assets/img/connectors/MS-azure-storage-connector.png" title="Microsoft Azure Storage Connector" width="800" alt="Microsoft Azure Storage Connector"/>

If you do not want to build this yourself, you can simply [get the project](#get-the-project) and run it.

## Configure the connector in WSO2 Integration Studio

Follow these steps to set up the ESB Solution Project and the Connector Exporter Project.

{!references/connectors/importing-connector-to-integration-studio.md!}

1. Right click on the created ESB Solution Project and select, -> **New** -> **Rest API** to create the REST API.

2. Specify the API name as `MSAzureStorage` and API context as `/resources`. You can go to the source view of the XML configuration file of the API and copy the following configuration (source view).

    ```
   <?xml version="1.0" encoding="UTF-8"?>
   <api context="/resources" name="MSAzureStorage" xmlns="http://ws.apache.org/ns/synapse">
       <resource methods="POST" url-mapping="/createcontainer">
           <inSequence>
               <property expression="json-eval($.accountName)" name="accountName" scope="default" type="STRING"/>
               <property expression="json-eval($.accountKey)" name="accountKey" scope="default" type="STRING"/>
               <property expression="json-eval($.containerName)" name="containerName" scope="default" type="STRING"/>
               <msazurestorage.init>
                   <accountName>eiconnectortest</accountName>
                   <accountKey>bWt69gFpheoD6lwVsMgeV5io2/KxlXK1KUcod68PhzuV1xHxje0LBD4Bd+y+ESAOlH5BTAfvdDG5q4Hhg==</accountKey>
               </msazurestorage.init>
               <msazurestorage.createContainer>
                   <containerName>{$ctx:containerName}</containerName>
               </msazurestorage.createContainer>
               <log level="full">
                   <property name="Container created" value="Container created"/>
               </log>
               <respond/>
           </inSequence>
           <outSequence/>
           <faultSequence/>
       </resource>
       <resource methods="POST" url-mapping="/listcontainer">
           <inSequence>
               <property expression="json-eval($.accountName)" name="accountName" scope="default" type="STRING"/>
               <property expression="json-eval($.accountKey)" name="accountKey" scope="default" type="STRING"/>
               <msazurestorage.init>
                   <accountName>eiconnectortest</accountName>
                   <accountKey>bWt69gFpheoD6lwVsMgeV5io2/KxlXK1KUcod68PhzuV1xHxje0LBD4Bd+y+ESAOlH5BTAfvdDG5q4Hhg==</accountKey>
               </msazurestorage.init>
               <msazurestorage.listContainers/>
               <log level="full">
                   <property name="List containers" value="List containers"/>
               </log>
               <respond/>
           </inSequence>
           <outSequence/>
           <faultSequence/>
       </resource>
       <resource methods="POST" url-mapping="/adddetails">
           <inSequence>
               <property expression="json-eval($.accountName)" name="accountName" scope="default" type="STRING"/>
               <property expression="json-eval($.accountKey)" name="accountKey" scope="default" type="STRING"/>
               <property expression="json-eval($.containerName)" name="containerName" scope="default" type="STRING"/>
               <property expression="json-eval($.fileName)" name="fileName" scope="default" type="STRING"/>
               <property expression="json-eval($.filePath)" name="filePath" scope="default" type="STRING"/>
               <msazurestorage.init>
                   <accountName>eiconnectortest</accountName>
                   <accountKey>bWt69gFpheoD6lwVsMgeV5io2/KxlXK1KUcod68PhzuV1xHxje0LBD4Bd+y+ESAOlH5BTAfvdDG5q4Hhg==</accountKey>
               </msazurestorage.init>
               <msazurestorage.uploadBlob>
                   <containerName>{$ctx:containerName}</containerName>
                   <filePath>{$ctx:filePath}</filePath>
                   <fileName>{$ctx:fileName}</fileName>
               </msazurestorage.uploadBlob>
               <log level="full">
                   <property name="Uploaded emplyee details" value="Uploaded emplyee details"/>
               </log>
               <respond/>
           </inSequence>
           <outSequence/>
           <faultSequence/>
       </resource>
       <resource methods="POST" url-mapping="/listdetails">
           <inSequence>
               <property expression="json-eval($.accountName)" name="accountName" scope="default" type="STRING"/>
               <property expression="json-eval($.accountKey)" name="accountKey" scope="default" type="STRING"/>
               <property expression="json-eval($.containerName)" name="containerName" scope="default" type="STRING"/>
               <msazurestorage.init>
                   <accountName>eiconnectortest</accountName>
                   <accountKey>bWt69gFpheoD6lwVsMgeV5io2/KxlXK1KUcod68PhzuV1xHxje0LBD4Bd+y+ESAOlH5BTAfvdDG5q4Hhg==</accountKey>
               </msazurestorage.init>
               <msazurestorage.listBlobs>
                   <containerName>{$ctx:containerName}</containerName>
               </msazurestorage.listBlobs>
               <log level="full">
                   <property name="List uploaded emplyee details" value="List uploaded emplyee details"/>
               </log>
               <respond/>
           </inSequence>
           <outSequence/>
           <faultSequence/>
       </resource>
       <resource methods="POST" url-mapping="/deletedetails">
           <inSequence>
               <property expression="json-eval($.accountName)" name="accountName" scope="default" type="STRING"/>
               <property expression="json-eval($.accountKey)" name="accountKey" scope="default" type="STRING"/>
               <property expression="json-eval($.containerName)" name="containerName" scope="default" type="STRING"/>
               <property expression="json-eval($.fileName)" name="fileName" scope="default" type="STRING"/>
               <msazurestorage.init>
                   <accountName>eiconnectortest</accountName>
                   <accountKey>bWt69gFpheoD6lwVsMgeV5io2/KxlXK1KUcod68PhzuV1xHxje0LBD4Bd+y+ESAOlH5BTAfvdDG5q4Hhg==</accountKey>
               </msazurestorage.init>
               <msazurestorage.deleteBlob>
                   <containerName>{$ctx:containerName}</containerName>
                   <fileName>{$ctx:fileName}</fileName>
               </msazurestorage.deleteBlob>
               <log level="full">
                   <property name="Delete selected employee details" value="Delete selected employee details"/>
               </log>
               <respond/>
           </inSequence>
           <outSequence/>
           <faultSequence/>
       </resource>
       <resource methods="POST" url-mapping="/deletecontainer">
           <inSequence>
               <property expression="json-eval($.accountName)" name="accountName" scope="default" type="STRING"/>
               <property expression="json-eval($.accountKey)" name="accountKey" scope="default" type="STRING"/>
               <property expression="json-eval($.containerName)" name="containerName" scope="default" type="STRING"/>
               <msazurestorage.init>
                   <accountName>eiconnectortest</accountName>
                   <accountKey>bWt69gFpheoD6lwVsMgeV5io2/KxlXK1KUcod68PhzuV1xHxje0LBD4Bd+y+ESAOlH5BTAfvdDG5q4Hhg==</accountKey>
               </msazurestorage.init>
               <msazurestorage.deleteContainer>
                   <containerName>{$ctx:containerName}</containerName>
               </msazurestorage.deleteContainer>
               <log level="full">
                   <property name="Delete selected container" value="Delete selected container"/>
               </log>
               <respond/>
           </inSequence>
           <outSequence/>
           <faultSequence/>
       </resource>
   </api>
   ```
**Note**

* As `accountKey` use the access key obtained from setting up the Microsoft Azure Storage account.
* As `accountName` get the name of created **Storage Account** inside the Microsoft Azure Storage account.
    
Now we can export the imported connector and the API into a single CAR application. CAR application is the one we are going to deploy to server runtime. 
   
{!/references/connectors/exporting-artifacts.md!}

## Get the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../../assets/attach/connectors/ms-azure-connector.zip">
    <img src="../../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

## Deployment

Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}

## Testing

Invoke the API as shown below using the curl command. Curl Application can be downloaded from [here] (https://curl.haxx.se/download.html).

1. Creating a new container in Microsoft Azure Storage for store employee details.
 
   **Sample request**

   `curl -v POST -d {"containerName":"employeedetails"} "http://localhost:8290/resources/createcontainer" -H "Content-Type:application/json"`

   **Expected Response**
   `{
       "success": true
    }`
    
2. Retrieve information about the created containers.    
   
   **Sample request**
   
   `curl -v POST -d {} "http://localhost:8290/resources/listcontainer" -H "Content-Type:application/json"`
   
   **Expected Response**
   
     It will retrieve all the existing container names.
     
     `{
         "result":{
            "container":[
               "employeedetails",
               "employeefinancedetails"
            ]
         }
      }`

3. Upload text or binary employee data (blob).

   **Sample request**
   
     `curl -v POST -d {"containerName": "employeedetails","fileName": "sample.txt","filePath": "/home/kasun/Documents/MSAZURESTORAGE/sample.txt"} "http://localhost:8290/resources/adddetails" -H "Content-Type:application/json"`
    
   **Expected Response**
     
     `{
          "success": true
      }`
 
4. Retrieve information about the uploaded text or binary employee data (blob).

   **Sample request**
     
     `curl -v POST -d {"containerName": "employeedetails"} "http://localhost:8290/resources/listdetails" -H "Content-Type:application/json"`

   **Expected Response**
   
     It will retrieve the uploaded text or binary name and the file path.
     
     `{
          "result": {
              "blob": "http://eiconnectortest.blob.core.windows.net/employeedetails/sample.txt"
          }
      }`

5. Remove uploaded employee details (blob).

   **Sample request**
   
     `curl -v POST -d {"containerName": "employeedetails","fileName": "sample.txt"} "http://localhost:8290/resources/deletedetails" -H "Content-Type:application/json"`
 
   **Expected Response**
    
     `{
          "success": true
      }`

6. Remove created container.

   **Sample request**
     
     `curl -v POST -d {"containerName": "employeedetails"} "http://localhost:8290/resources/deletecontainer" -H "Content-Type:application/json"`
 
   **Expected Response**
     
     `{
          "success": true
      }`

## What's next

* You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).