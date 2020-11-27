# File Connector Example

File Connector can be used to perform operations in the local file system as well as in a remote server such as FTP and SFTP. 

## What you'll build

This example describes how to use the File Connector to write messages to local files and read the files. Similarly, the same example can be configured to communicate with a remote file system (i.e FTP server) easily. The example also uses some other mediators coming with WSO2 EI to manipulate messages. 

<!--
Following diagram shows the overall solution.

<img src="../../../../assets/img/connectors/FileConnector-01.png" title="Adding a Rest API" width="800" alt="Adding a Rest API"/>
-->

An API is exposed to accept XML messages (employee information).
Once a message comes in, it is converted to a CSV message  and then stored to a property. 
A check is done to see if the CSV file with information exists in the file system. If it does not exist, the connector creates a CSV file with CSV headers included. Then the connector appends the new CSV entries in the current message to the CSV file. 
Then the connector reads the same CSV file and converts the information back to XML and responds to the client.

If you do not want to configure this yourself, you can simply [get the project](#get-the-project) and run it.

## Setting up the environment

Create a folder in your local file system where you have read and write access. Let’s consider this as the working directory. In this example it is /Users/hasitha/temp/file-connector-test/dataCollection. 

!!! Note
    If you set up a FTP server, SFTP server or Samba server, make required configurations and select a working directory. Keep host, port and security related information saved for future use. 

## Configure the connector in WSO2 Integration Studio

Follow these steps to set up the Integration Project and the Connector Exporter Project. 

{!references/connectors/importing-connector-to-integration-studio.md!} 

## Creating the Integration Logic

1.  Create a new integration project. Make sure to enable the Connector Exporter project. 
    
    <img src="../../../../assets/img/connectors/filecon1.png" title="create project" width="800" alt="create project"/>


2.  Create an API to accept employee information called TestAPI with the context `/fileTest`. 

    <img src="../../../../assets/img/connectors/filecon2.png" title="create API" width="800" alt="create API"/>

3.  In the default resource created, enable post requests. 

    <img src="../../../../assets/img/connectors/filecon3.png" title="select post method" width="800" alt="select post method"/>

4.  Drag and drop the log mediator to the design palette and add a custom log to indicate that the API received a message.  
5.  Then transform the incoming XML message to a CSV message using the DataMapper mediator. To do that, drag and drop it to the design palette. Double click on the Datamapper mediator icon and add a new transform configuration called “xmlToCsv”.

    <img src="../../../../assets/img/connectors/filecon4.png" title="new datamapper config" width="800" alt="new datamapper config"/>

6.  Load input file into the Datamapper config view. Save the following content into a file and load it as the input. 

    ```xml
    <test>
    <information>
        <people>
            <person>
                <name>Hasitha</name>
                <age>34</age>
                <company>wso2</company>
            </person>
            <person>
                <name>Johan</name>
                <age>32</age>
                <company>IBM</company>
            </person>
        </people>
    </information>
    </test>
    ```

    <img src="../../../../assets/img/connectors/filecon5.png" title="load input" width="800" alt="load input"/>

7.  Load the output CSV file into the datamapper config view. Save the following content as a CSV file and load it as the output file. 

    Name,Age,Company
    Hasitha,34,wso2
    Johan,32,IBM

    <img src="../../../../assets/img/connectors/filecon6.png" title="load output csv" width="800" alt="load output csv"/>

8.  Configure mapping as below. Each element in the input should be mapped to the respective element in the output. 

    <img src="../../../../assets/img/connectors/filecon7.png" title="data mapping" width="800" alt="data mapping"/>

9.  Configure input as XML and output as CSV in the datamapper as below. 

    <img src="../../../../assets/img/connectors/filecon8.png" title="datamapper input output config" width="800" alt="datamapper input output config"/>

10.  Drag and drop the enrich mediator to the palette and save the generated output of the datamper to a property called CONTENT configuring the mediator as below. 

    <img src="../../../../assets/img/connectors/filecon9.png" title="enrich - save payload" width="800" alt="enrich - save payload"/>

11. Then use the file connector to check if the file containing employee information exists in the file system. Drag and drop “checkExist” operation of the file connector and configure it. 

    -   Create a new File connection pointing to the working directory we have set up and keep it as the File Connection for the operation.  

        <img src="../../../../assets/img/connectors/filecon10.png" title="working directory" width="800" alt="working directory"/>

    -   Configure file path as /dataCollection/employees/employees.csv. This is the file we keep employees' information. 

        <img src="../../../../assets/img/connectors/filecon11.png" title="checkExist operation" width="800" alt="checkExist operation"/>

12. Then use the Filter Mediator to branch the logic depending on the result of File Connector’s checkExist operation. If the file does not exist, we will again use the file connector’s write operation to create the file.

    -   Click on the Filter mediator and define filter condition as below. 

        <img src="../../../../assets/img/connectors/filecon12.png" title="filter mediator" width="800" alt="filter mediator"/>

    -   Inside “else” block, drag and drop File Connector write operation and configure it to write static content of CSV file headers - “Name,Age,Company”. Make sure to append a new line at EOF. Write mode needs to be “Create New”. 

        <img src="../../../../assets/img/connectors/filecon13.png" title="create new" width="800" alt="create new"/>

13. Then after the filter mediator, use enrich mediator again to put back saved payload into the message payload again. 

    <img src="../../../../assets/img/connectors/filecon14.png" title="enrich - put back payload" width="800" alt="enrich - put back payload"/>

14. Now it is time to append the CSV message to the existing file. Drag and drop File Connector’s Write operation again and append the content. Thus the write mode needs to be “Append”. As we need the newest message on the top, always append to line number 2. 

    <img src="../../../../assets/img/connectors/filecon15.png" title="append to file" width="800" alt="append to file"/>

15. Then Read the same CSV file using File connector read operation. Drag and drop it to the palette and configure it as below. Note that we read the file from line number 2. We read it as a text message. 

    <img src="../../../../assets/img/connectors/filecon16.png" title="read csv file" width="800" alt="read csv file"/>

16. Now we will convert the read CSV message back to XML using datamapper again. Drag and drop the Datamapper mediator again to the palette, double click and add a new configuration called “csvToXml”. This time input and output should be flipped w.r.t earlier configuration. Mapping is from CSV to XML.

    <img src="../../../../assets/img/connectors/filecon17.png" title="output datamapper config" width="800" alt="output datamapper config"/>

    <img src="../../../../assets/img/connectors/filecon18.png" title="output datamapper dialog" width="800" alt="output datamapper dialog"/>

17. As the last mediator on the message flow, use Respond Mediator to send the transformed message to the caller of the API. 
18. Define a fault sequence with an error log and Respond Mediator and add it as the fault sequence of the API resource. Then, if some error happened during the configured message flow, the fault sequence will get hit. 

    <img src="../../../../assets/img/connectors/filecon19.png" title="fault sequence" width="800" alt="fault sequence"/>

    <img src="../../../../assets/img/connectors/filecon20.png" title="error log" width="800" alt="error log"/>

{!references/connectors/exporting-artifacts.md!}


## Get the project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../../assets/attach/connectors/FileConnector.zip">
    <img src="../../../../assets/img/connectors/download-zip.png" width="200" alt="Download ZIP">
</a>

## Deployment

Follow these steps to deploy the exported CApp in the Enterprise Integrator Runtime. 

{!references/connectors/deploy-capp.md!}

## Testing

1. Create a file called data.json with the following payload. 
    ```xml
    <test>
    <information>
        <people>
            <person>
                <name>Hasitha</name>
                <age>34</age>
                <company>wso2</company>
            </person>
            <person>
                <name>Johan</name>
                <age>32</age>
                <company>IBM</company>
            </person>
            <person>
                <name>Bob</name>
                <age>30</age>
                <company>Oracle</company>
            </person>
            <person>
                <name>Alice</name>
                <age>28</age>
                <company>Microsoft</company>
            </person>
            <person>
                <name>Anne</name>
                <age>30</age>
                <company>Google</company>
            </person>
        </people>
    </information>
    </test>
    ```
    **Note**: When you configuring this `source` parameter in Windows operating system you need to set this property shown as `<source>C:\\Users\Kasun\Desktop\Salesforcebulk-connector\create.txt</source>`.
    
2. Invoke the API as shown below using the curl command. Curl Application can be downloaded from [here](https://curl.haxx.se/download.html).

    ```bash
    curl -H "Content-Type: application/xml" --request POST --data @body.json http://10.100.5.136:8290/fileconnector/create
    ```

3.  Note that you will get the same message at the first invocation as the response. Check the file system to verify that the CSV file has been created. 

    <img src="../../../../assets/img/connectors/filecon21.png" title="file creation result" width="800" alt="file creation result"/>

4.  If you invoke again with a different set of employees, they will get appended to the same file, and as the response you will get all the employees (with employees that were added using the previous message).

In this example file connector was to create a file, write to a file and read a file. Blending these capabilities together with other powerful message manipulation features of WSO2 EI, we could come up with a working scenario in minutes. File connector has many more functionalities. Please read the File Connector reference guide for more information. 

## What's Next

* You can deploy and run your project on Docker or Kubernetes. See the instructions in [Running the Micro Integrator on Containers](../../../../setup/installation/run_in_containers).
* To customize this example for your own scenario, see [File Connector Configuration](file-connector-config.md) documentation for all operation details of the connector.
