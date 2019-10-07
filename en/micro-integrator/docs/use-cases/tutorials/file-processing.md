# File processing

## What you'll build

This sample demonstrates how to pick a file from a directory and process it within the Micro Integrator. In this sample scenario you pick a file from the local directory, insert the records in the file to a database, send an email with the file content, trace and write the log and finally move the file to another directory.

The result of the query should be as follows when you query to view the records in the `test.info` table. You will see that there is no data in the table.
  
## Let's get started!

### Step 1: Set up the workspace

-  Select the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system and extract the ZIP file.  The path to the extracted folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
-  Download the [CLI Tool](https://wso2.com/integration/micro-integrator/install/) for monitoring artifact deployments.

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

All files required for this sample are in [sample.zip](attachments/119129588/119129589.zip) .

### Step 2: Develop the integration artifacts 

Follow the instructions given in this section to create and configure the required artifacts.

#### Create an ESB Config project

To create an ESB solution consisting of an **ESB config** project and a **Composite Application** project:

1.  Open **WSO2 Integration Studio**.
2.  Go to **ESB Project** and click **Create New**.
    ![](../../assets/img/tutorials/119132413/119132414.png)

3.  Enter `FileProcessingService` as the project name.
4.  Click **Finish**. The created project is saved in the **Project Explorer**.

#### Create the Main and Fault sequences

1.  Find the `main.xml` and `fault.xml` files in the attached `sample.zip` archive. You can find the files in the `<SAMPLE_HOME>/conf/synapse-config/sequences` directory.
2.  Copy the files to `<ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences` folder.

    !!! Note
        The `main` and `fault` sequences are created and preconfigured automatically when you install WSO2 ESB.

**main.xml**
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

**fault.xml**
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

1.  You can find the `FileProxy.xml` file in the attached `sample.zip` archive. It is located in the `<SAMPLE_HOME>/conf/synapse-config/proxy-services`
    directory.  
      
    The XML code of the sequence is as follows:

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

2.  Edit the FileProxy.xml file, and define the directory to which the original file should be moved after processing.

    ```xml
    <parameter name="transport.vfs.MoveAfterProcess">[file:///home/]<username>/test/original</parameter>
    ```

3.  Edit the FileProxy.xml file, and define where the input file should be placed.

    ```xml
    <parameter name="transport.vfs.FileURI">[file:///home/]<username>/test/in</parameter>
    ```

4.  Edit the FileProxy.xml file, and define the directory to which the file should be moved if an error occurs.

    ```xml
    <parameter name="transport.vfs.MoveAfterFailure">[file:///home/]<username>/test/failure</parameter>
    ```

5.  Save the FileProxy.xml file to the `<ESB_HOME>/repository/deployment/server/synapse-configs/default/proxy-services`
    directory.

#### Create the `databaseSequence`

Follow the instructions below to create a sequence that can be used to
connect to the database to insert the data.

1.  You can find the `databaseSequence.xml` file in the attached `sample.zip` archive. It is located in the `<SAMPLE_HOME>/conf/synapse-config/sequences` directory.  

    The XML code of the database sequence is as follows.

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
        <iterate expression="//csv-records/csv-record">
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
3.  Save the `          databaseSequence.xml         ` file to the
    `          <ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences         `
    directory.


#### Create the `fileWriteSequence`

1.  You can find the `fileWriteSequence.xml` in the attached `sample.zip` archive. It is located in the `<SAMPLE_HOME>/conf/synapse-config/sequences`
    directory.  
      
    The XML code of the sequence is as follows:

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

2.  Edit the `fileWriteSequence.xml` file, and define the directory to which the file should be moved.
3.  Save the `fileWriteSequence.xml` file to the `<ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences` directory.

#### Create the `sendMailSequence`

1.  You can find the `sendMailSequence.xml` file in the attached `sample.zip` archive. It is located in the `<SAMPLE_HOME>/conf/synapse-config/sequences` directory.  
      
    The XML code of the sequence is as follows:

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

2.  Edit the `sendMailSequence.xml` file, and define the e-mail address to which the notification should be sent.
3.  Save the `sendMailSequence.xml` file to the `<ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences         `
    directory.

### Step 3: Package the artifacts

Package the artifacts in your composite application project to be able to deploy the artifacts in the server.

1.  Open the `          pom.xml         ` file in the composite application project POM editor.
2.  Ensure that the artifacts are selected in the POM file.
3.  Save the project.

### Step 4: Build and run the artifacts

To test the artifacts, deploy the [packaged artifacts](#step-3-package-the-artifacts) in the embedded Micro Integrator:

1.  Right-click the composite application project and click **Export Project Artifacts and Run**.
2.  In the dialog that opens, select the composite application project that you want to deploy.  
4.  Click **Finish**. The artifacts will be deployed in the embedded Micro Integrator and the server will start. See the startup log in the **Console** tab.

### Step 5: Test the use case

#### Configure the Micro Integrator

The **VFS** transport is enabled in the Micro Integrator by default. Enable the [MailTo transport](../../setup/transport_configurations/configuring-transports/#configuring-the-mailto-transport) for sending the email message as shown below and update the values:

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

Add the following message formatter in the `deployment.toml` file (stored in the `MI_HOME/conf` directory):

```toml
<messageFormatter contentType="text/html" class="org.apache.axis2.transport.http.ApplicationXMLFormatter"/>
```

#### Add database drivers

1.  Find the MySQL database driver `mysql-connector-java-5.1.10-bin.jar` in the attached `sample.zip` archive. You can find the file in the `SAMPLE_HOME/lib` directory.
2.  Copy the driver to the `MI_HOME/components/lib` directory.

#### Add smooks libraries

This example uses a CSV smooks library.

1.  You can find the CSV smooks library `milyn-smooks-csv-1.2.4.jar` and the `groovy-all-1.1-rc-1.jar` in the attached `sample.zip` archive. You can find the file in
    the `SAMPLE_HOME/lib` directory.
2.  Copy the library to the `MI_HOME/repository/components/lib` directory.
3.  Configure a local entry as follows. For information on how to add a local entry, see [this link](https://docs.wso2.com/display/EI650/Working+with+Local+Registry+Entries). This local entry will be used to refer to the smooks configuration saved in the `SAMPLE_HOME/resources/smooks-config.xml` file.

    ```xml
    <localEntry key="smooks" src="file:resources/smooks-config.xml"/>
    ```

#### Create the input file

Create a text file with the following format:

```xml
name_1, surname_1, phone_1
name_2, surname_2, phone_2
```

![](attachments/119129588/119129595.png)

Save the file in the `          .txt         ` format to the `          in         ` directory that you specified.

#### Analyze the result

The Micro Integrator listens on a local file system directory. When a file is dropped into the `in` directory, the Micro Integrator picks
this file.

1.  Make sure the file appears in the `          out         `
    directory.
2.  The Micro Integrator inserts the records from the text file to the database. Make sure the data is in the info table. The following screenshot displays the content of the `          test.info         ` table with the data from the file.  
    ![](attachments/119129588/119129598.png)
3.  Make sure the original file is moved to the `          /home/<username>/test/original         ` directory.
4.  Make sure the e-mail notification is sent to the email address that is specified. The message should contain the file data. The
    following screenshot displays a notification received.  
    ![](attachments/119129588/119129594.png)
