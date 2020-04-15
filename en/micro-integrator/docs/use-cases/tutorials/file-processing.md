# File Processing

## What you'll build

This tutorial demonstrates how to pick a file from a directory and process it within the Micro Integrator. In this sample scenario the Micro Integrator will pick a file from the local directory, insert the records in the file to a database, send an email with the file content, trace and write the log, and finally move the file to another directory.

When you query to view the records in the `test.info` table, you will see that there is no data in the table.
  
## Let's get started!

### Step 1: Set up the workspace

- Download the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system. The path to the extracted/installed folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
- Optionally, you can set up the **CLI tool** for artifact monitoring. This will later help you get details of the artifacts that you deploy in your Micro Integrator.

    1.  Go to the [WSO2 Micro Integrator website](https://wso2.com/integration/#). 
    2.  Click **Download -> Other Resources** and click **CLI Tooling** to download the tool. 
    3.  Extract the downloaded ZIP file. This will be your `MI_CLI_HOME` directory. 
    4.  Export the `MI_CLI_HOME/bin` directory path as an environment variable. This allows you to run the tool from any location on your computer using the `mi` command. Read more about the [CLI tool](../../../administer-and-observe/using-the-command-line-interface).

Let's setup a MySQL database:

1.  Manually set up the database.
2.  Create a table named `           info          ` in your schema. You
    can run the following commands to do this.

    ```java
    delimiter $$

    CREATE TABLE `info` (
      `name` varchar(45) DEFAULT '',
      `surname` varchar(45) DEFAULT NULL,
      `phone` varchar(45) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8$$
    ```

3.  Make sure the `info` table is created and that it contains the following columns:
    -   **name**
    -   **surname**
    -   **phone**

### Step 2: Develop the integration artifacts 

Follow the instructions given in this section to create and configure the required artifacts.

#### Creating the Config project

To create a solution consisting of an **Config** project and a **Composite Application** project:

1.  Open **WSO2 Integration Studio**.
2.  Go to **Integration** and click **Create Integration Project**.
    ![](../../../assets/img/create_project/create-integration-project.png)

3.  Enter `FileProcessingService` as the project name and select the **Create Composite Application Project** check box.
4.  Click **Finish**. The created project is saved in the **Project Explorer**.

#### Create the Main and Fault sequences

1.  Create the Main sequence with the following configuration. See the instructions on [creating a sequence](../../../develop/creating-artifacts/creating-reusable-sequences).
    ```xml
    <sequence name="main" xmlns="http://ws.apache.org/ns/synapse">
        <in>
            <log level="full"/>
            <filter regex="http://localhost:9000.*" source="get-property('To')">
                <then>
                    <send/>
                </then>
                <else/>
            </filter>
        </in>
        <out>
            <send/>
        </out>
    </sequence>
    ```
2.  Create the Fault sequence with tthe following configuration. See the instructions on [creating a sequence](../../../develop/creating-artifacts/creating-reusable-sequences).

    ```xml	
    <sequence name="fault" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
        <log level="full">
            <property name="MESSAGE" value="Executing default 'fault' sequence"/>
            <property expression="get-property('ERROR_CODE')" name="ERROR_CODE"/>
            <property expression="get-property('ERROR_MESSAGE')" name="ERROR_MESSAGE"/>
        </log>
        <drop/>
    </sequence>
    ```

#### Create the `FileProxy`

1.  Create a proxy service named `FileProxy` with the following configuration. See the instructons on [creating a proxy service](../../../develop/creating-artifacts/creating-a-proxy-service).

    ```xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="FileProxy" transports="vfs" startOnLoad="true" trace="disable">
        <target>
            <inSequence>
                <log level="full"/>
                <clone>
                    <target sequence="fileWriteSequence"/>
                    <target sequence="sendMailSequence"/>
                    <target sequence="databaseSequence"/>
                </clone>
            </inSequence>
        </target>
        <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
        <parameter name="transport.PollInterval">15</parameter>
        <parameter name="transport.vfs.MoveAfterProcess">file:///home/username/test/original</parameter>
        <parameter name="transport.vfs.FileURI">file:///home/username/test/in</parameter>
        <parameter name="transport.vfs.MoveAfterFailure">file:///home/username/test/failure</parameter>
        <parameter name="transport.vfs.FileNamePattern">.*.txt</parameter>
        <parameter name="transport.vfs.ContentType">text/plain</parameter>
        <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
    </proxy>
    ```

2.  Edit the proxy service and define the directory to which the original file should be moved after processing.

    ```xml
    <parameter name="transport.vfs.MoveAfterProcess">[file:///home/]<username>/test/original</parameter>
    ```

3.  Edit the proxy service and define where the input file should be placed.

    ```xml
    <parameter name="transport.vfs.FileURI">[file:///home/]<username>/test/in</parameter>
    ```

4.  Edit the proxy service and define the directory to which the file should be moved if an error occurs.

    ```xml
    <parameter name="transport.vfs.MoveAfterFailure">[file:///home/]<username>/test/failure</parameter>
    ```

#### Create the `databaseSequence`

Follow the instructions below to create a sequence that can be used to
connect to the database to insert the data.

1.  Create a sequence named `databaseSequence` with the following configuration. See the instructions on [creating a sequence](../../../develop/creating-artifacts/creating-reusable-sequences).

    ```xml
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="databaseSequence">
        <log level="full">
            <property name="sequence" value="before-smooks"/>
        </log>
        <smooks config-key="smooks">
            <input type="text"/>
            <output type="xml"/>    
        </smooks>
        <log level="full">
            <property name="sequence" value="after-smooks"/>
        </log>
        <iterate expression="//csv-set/csv-record">
          <target>
           <sequence>
            <dbreport>
              <connection>
               <pool>
                 <password>db-password</password>
                 <user>db-username</user>
                 <url>jdbc:mysql://localhost:3306/test</url>
                 <driver>com.mysql.jdbc.Driver</driver>
               </pool>
              </connection>
               <statement>
                <sql><![CDATA[insert into INFO values (?, ?, ?)]]></sql>
                  <parameter expression="//csv-record/name/text()" type="VARCHAR"/>
                  <parameter expression="//csv-record/surname/text()" type="VARCHAR"/>
                  <parameter expression="//csv-record/phone/text()" type="VARCHAR"/>
                </statement>
            </dbreport>
          </sequence>
        </target>
      </iterate>
    </sequence>
    ```

2.  Specify your database username, password, and URL in the
    `          <pool>         ` section of the sequence.

#### Create the `fileWriteSequence`

1.  Create a sequence named `fileWriteSequence` with the following configuration. See the instructions on [creating a sequence](../../../develop/creating-artifacts/creating-reusable-sequences).

    ```xml
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="fileWriteSequence">
        <log level="custom">
            <property name="sequence" value="fileWriteSequence"/>
        </log>
        <property xmlns:ns2="http://org.apache.synapse/xsd" name="transport.vfs.ReplyFileName" expression="fn:concat(fn:substring-after(get-property('MessageID'), 'urn:uuid:'), '.txt')" scope="transport"/>
        <property name="OUT_ONLY" value="true"/>
        <send>
            <endpoint name="FileEpr">
                <address uri="vfs:file:///home/username/test/out"/>
            </endpoint>
        </send>
    </sequence>
    ```

2.  Edit the sequence and define the directory to which the file should be moved.

#### Create the `sendMailSequence`

1.  Create a sequence named `sendMailSequence` with the following configuration. See the instructions on [creating a sequence](../../../develop/creating-artifacts/creating-reusable-sequences). 
      
    ```xml
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="sendMailSequence">
        <log level="custom">
            <property name="sequence" value="sendMailSequence"/>
        </log>
        <property name="messageType" value="text/html" scope="axis2"/>
        <property name="ContentType" value="text/html" scope="axis2"/>
        <property name="Subject" value="File Received" scope="transport"/>
        <property name="OUT_ONLY" value="true"/>
        <send>
            <endpoint name="FileEpr">
                <address uri="mailto:username@gmail.com"/>
            </endpoint>
        </send>
    </sequence>
    ```

2.  Edit the sequence and define the e-mail address to which the notification should be sent.

#### Create the Smooks configuration

Create a smooks configuration as shown below. See the instructions on [creating a smooks configuration](../../../develop/creating-artifacts/creating-smooks-artifacts). 

```xml
<smooks-resource-list xmlns="http://www.milyn.org/xsd/smooks-1.0.xsd">
  <!--Configure the CSVParser to parse the message into a stream of SAX events. -->
  <resource-config selector="org.xml.sax.driver">
    <resource>org.milyn.csv.CSVReader</resource>
    <param name="fields" type="string-list">name,surname,phone</param>
  </resource-config>
</smooks-resource-list>
```

#### Create a local registry entry

Configure a local entry as shown below. This local entry will be used to refer to the [smooks configuration](#create-the-smooks-configuration).
See the instructions on [creating a local registry configuration](../../../develop/creating-artifacts/registry/creating-local-registry-entries).

```xml
<localEntry key="smooks" src="file:resources/smooks-config.xml"/>
```

### Step 3: Package the artifacts

Package the artifacts in your composite application project to be able to deploy the artifacts in the server.

1.  Open the `pom.xml` file in the composite application project you created.
2.  Ensure that all of the artifacts are selected in the POM file.
3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the artifacts that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab.

### Step 5: Test the use case

#### Get details of deployed artifacts (Optional)

Let's use the **CLI Tool** to find information of the artifacts you deployed in the Micro integrator.

!!! Tip
    Be sure to set up the CLI tool for your work environment as explained in the [first step](#step-1-set-up-the-workspace) of this tutorial.

1.  Open a terminal and execute the following command to start the tool:
    ```bash
    mi
    ```
    
2.  Log in to the CLI tool. Let's use the server administrator user name and password:
    ```bash
    mi remote login admin admin
    ```

    You will receive the following message: *Login successful for remote: default!*

3.  Execute the following command to find the sequences deployed in the server:
    ```bash
    mi sequence show
    ```

Similarly, you can get details of other integration artifacts deployed in the server. Read more about [using the CLI tool](../../../administer-and-observe/using-the-command-line-interface).

#### Configure the Micro Integrator

Open the `deployment.toml` file of the embedded Micro Integrator of WSO2 Integration Studio (stored in the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/conf/` directory on **Windows** or `MI_TOOLING_HOME/runtime/microesb/conf` directory on **MacOS/Linux/CentOS**) and apply the following changes:

1.  The **VFS** transport is enabled in the Micro Integrator by default. Enable the [MailTo transport](../../../setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport) for sending the email message as shown below and update the values:

    ```toml
    [[transport.mail.sender]]
    name = "mailto"
    parameter.hostname = "smtp.gmail.com"
    parameter.port = "587"
    parameter.enable_tls = true
    parameter.auth = true
    parameter.username = "demo_user"
    parameter.password = "mailpassword"
    parameter.from = "demo_user@wso2.com"
    ```
          
    !!! Note
        In this sample, you will not retrieve mails from a mailbox. Therefore, you do not need to enable the mailto transport receiver.

2.  Add the following message formatter:

    ```toml
    text_xml = "org.apache.axis2.transport.http.ApplicationXMLFormatter"
    ```

#### Add database drivers

1.  Find the MySQL database driver `mysql-connector-java-5.1.10-bin.jar` in the [attached file](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/Artifacts-fileProcessingTutorial.zip). 
    You can find the file in the `SAMPLE_HOME/lib` directory.
2.  Copy the driver to the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` directory (on **Windows**) or the `MI_TOOLING_HOME/runtime/microesb/lib` directory (on **MacOS/Linux/CentOS**).

#### Add smooks libraries

This example uses a CSV smooks library.

1.  You can find the CSV smooks library `milyn-smooks-csv-1.2.4.jar` in the [attached file](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/Artifacts-fileProcessingTutorial.zip). 
    You can find the file in the `SAMPLE_HOME/lib` directory.
2.  Copy the library to the the `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/lib/` directory (on **Windows**) or the `MI_TOOLING_HOME/runtime/microesb/lib` directory (on **MacOS/Linux/CentOS**).

#### Create the input file

Create a text file with the following format:

```xml
name_1, surname_1, phone_1
name_2, surname_2, phone_2
```

Save the file in the `          .txt         ` format to the `          in         ` directory that you specified.

#### Analyze the result

The Micro Integrator listens on a local file system directory. When a file is dropped into the `in` directory, the Micro Integrator picks
this file.

1.  Make sure the file appears in the `          out         `
    directory.
2.  The Micro Integrator inserts the records from the text file to the database. Make sure the data is in the info table. The following screenshot displays the content of the `          test.info         ` table with the data from the file.  
3.  Make sure the original file is moved to the `          /home/<username>/test/original         ` directory.
4.  Make sure the e-mail notification is sent to the email address that is specified. The message should contain the file data. The
    following screenshot displays a notification received. If you see the error message `Username and Password not accepted` in the logs, you might need to turn on `Allow less secure apps` in your Google account from [here](https://myaccount.google.com/lesssecureapps).