# File Processing

### Introduction

This sample demonstrates how to pick a file from a directory and process
it within the ESB. In this sample scenario you pick a file from the
local directory, insert the records in the file to a database, send an
email with the file content, trace and write the log and finally move
the file to another directory.

The following diagram displays the entities involved in this example.

![](attachments/119129588/119129597.png)

!!! Note
    This example works with the MySQL database.

### Building the sample

All files required for this sample are in
[sample.zip](attachments/119129588/119129589.zip) .

Follow the steps given below to build this sample.

#### Set up the database

1.  Manually set up the database.
2.  Create a table named `           info          ` in your schema. You
    can run the following commands to do this.

    ``` java
    delimiter $$

    CREATE TABLE `info` (
      `name` varchar(45) DEFAULT '',
      `surname` varchar(45) DEFAULT NULL,
      `phone` varchar(45) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8$$
    ```

3.  Make sure the `           info          ` table is created and that
    it contains the following columns:

    -   **name**
    -   **surname**
    -   **phone**

The result of the query should be as follows when you query to view the
records in the `         test.info        ` table. You will see that
there is no data in the table.

  
![](attachments/119129588/119129591.png)

#### Create the main and fault sequences

1.  Find the `          main.xml         ` and
    `          fault.xml         ` files in the attached
    `          sample.zip         ` archive. You can find the files in
    the
    `          <SAMPLE_HOME>          /conf/synapse-config/sequences         `
    directory.
2.  Copy the files to
    `           <ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences          `
    folder.

    !!! Note
        The `           main          ` and `           fault          ` sequences are created and preconfigured automatically when you install WSO2 ESB.

#### Configure the ESB

You need to configure the ESB to use the [VFS
transport](https://docs.wso2.com/display/EI650/VFS+Transport) for
processing the files, and the [MailTo
transport](https://docs.wso2.com/display/EI650/MailTo+Transport) for
sending the email message. You also need to configure the message
formatter as specified.

-   Edit the
    `           <ESB_HOME>/repository/conf/axis2/axis2.xml          `
    file and configure the mailto transport sender as follows to use a
    mailbox for sending the messages:

    ```
    <transportSender name="mailto" class="org.apache.axis2.transport.mail.MailTransportSender">
       <parameter name="mail.smtp.host">smtp.gmail.com</parameter>
       <parameter name="mail.smtp.port">587</parameter>
       <parameter name="mail.smtp.starttls.enable">true</parameter>
       <parameter name="mail.smtp.auth">true</parameter>
       <parameter name="mail.smtp.user">username</parameter>
       <parameter name="mail.smtp.password">userpassword</parameter>
       <parameter name="mail.smtp.from">username@gmail.com</parameter>
    </transportSender>
    ```
      
    !!! Note
        In this sample, you will not retrieve mails from a mailbox. Therefore, you do not need to enable the mailto transport receiver.

-   Add the following message formatter in the
    `           <ESB_HOME>/repository/conf/Axis2/axis2.xml          `
    file under the `           Message Formatters          ` section:

    ```
    <messageFormatter contentType="text/html" class="org.apache.axis2.transport.http.ApplicationXMLFormatter"/>
    ```

#### Add database drivers

1.  Find the MySQL database driver
    `          mysql-connector-java-5.1.10-bin.jar         ` in the
    attached `          sample.zip         ` archive. You can find the
    file in the `          <SAMPLE_HOME>          /lib         `
    directory.
2.  Copy the driver to the
    `          <WSO2ESB_HOME>/repository/components/lib         `
    directory.

#### Add smooks libraries

This example uses a CSV smooks library.

1.  You can find the CSV smooks library
    `          milyn-smooks-csv-1.2.4.jar         ` in the attached
    `          sample.zip         ` archive. You can find the file in
    the `          <SAMPLE_HOME>/lib         ` directory.
2.  Copy the library to the
    `           <WSO2ESB_HOME>/repository/components/lib          `
    directory.

    !!! Note
        These configuration changes make system-wide changes to the ESB and the ESB has to be restarted for these changes to take effect.

3.  Configure a local entry as follows. For information on how to add a
    local entry, see [this
    link](https://docs.wso2.com/display/EI650/Working+with+Local+Registry+Entries)
    . This local entry will be used to refer to the smooks configuration
    saved in the
    `           <SAMPLE_HOME>/resources/smooks-config.xml          `
    file.

    ```
    <localEntry key="smooks" src="file:resources/smooks-config.xml"/>
    ```

#### Create and configure `         FileProxy        `

1.  You can find the `           FileProxy.xml          ` file in
    the attached `           sample.zip          ` archive. It is
    located in the
    `           <SAMPLE_HOME>/conf/synapse-config/proxy-services          `
    directory.  
      
    The XML code of the sequence is as follows:

    ```
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

2.  Edit the FileProxy.xml file, and define the directory to which the
    original file should be moved after processing.

    ```
    <parameter name="transport.vfs.MoveAfterProcess">[file:///home/]<username>/test/original</parameter>
    ```

3.  Edit the FileProxy.xml file, and define where the input file should
    be placed.

    ```
    <parameter name="transport.vfs.FileURI">[file:///home/]<username>/test/in</parameter>
    ```

4.  Edit the FileProxy.xml file, and define the directory to which the
    file should be moved if an error occurs.

    ```
    <parameter name="transport.vfs.MoveAfterFailure">[file:///home/]<username>/test/failure</parameter>
    ```

5.  Save the FileProxy.xml file to the
    `          <ESB_HOME>/repository/deployment/server/synapse-configs/default/proxy-services         `
    directory.

#### Create and configure `         databaseSequence        `

Follow the instructions below to create a sequence that can be used to
connect to the database to insert the data.

1.  You can find the `           databaseSequence.xml          ` file in
    the attached `           sample.zip          ` archive. It is
    located in the
    `           <SAMPLE_HOME>/conf/synapse-config/sequences          `
    directory.  
    The XML code of the database sequence is as follows.

    ``` java
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
        <iterate xmlns:ns2="http://org.apache.synapse/xsd"
                 xmlns:ns="http://org.apache.synapse/xsd"
                 xmlns:sec="http://secservice.samples.esb.wso2.org"
                 expression="//csv-records/csv-record">
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
                <sql>insert into info values (?, ?, ?)</sql>
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


#### Create and Configure fileWriteSequence

1.  You can find the `           fileWriteSequence.xml          ` in the
    attached `           sample.zip          ` archive. It is located in
    the
    `           <SAMPLE_HOME>/conf/synapse-config/sequences          `
    directory.  
      
    The XML code of the sequence is as follows:

    ```
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

2.  Edit the `           fileWriteSequence.xml          ` file, and
    define the directory to which the file should be moved.

3.  Save the `          fileWriteSequence.xml         ` file to the
    `          <ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences         `
    directory.

#### Create and configure `         sendMailSequence        `

1.  You can find the `           sendMailSequence.xml          ` file in
    the attached `           sample.zip          ` archive. It is
    located in the
    `           <SAMPLE_HOME>/conf/synapse-config/sequences          `
    directory.  
      
    The XML code of the sequence is as follows:

    ```
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

2.  Edit the `          sendMailSequence.xml         ` file, and define
    the e-mail address to which the notification should be sent.
3.  Save the `          sendMailSequence.xml         ` file to the
    `          <ESB_HOME>/repository/deployment/server/synapse-configs/default/sequences         `
    directory.

#### Create the input file

-   Create a text file in the following format.

    ``` java
    name_1, surname_1, phone_1
    name_2, surname_2, phone_2
    ```

    ![](attachments/119129588/119129595.png)

### Executing the sample

-   Save the file in the `          .txt         ` format to the
    `          in         ` directory that you specified in [step
    3](#Sample271:FileProcessing-FileProxyStep) , under the create and
    configure `          FileProxy         ` section.

### Analyzing the output

In this sample, the ESB listens on a local file system directory. When a
file is dropped into the `         in        ` directory, the ESB picks
this file.

1.  Make sure the file appears in the `          out         `
    directory.
2.  The ESB inserts the records from the text file to the database. Make
    sure the data is in the info table. The following screenshot
    displays the content of the `          test.info         ` table
    with the data from the file.  
    ![](attachments/119129588/119129598.png)
3.  Make sure the original file is moved to the
    `          /home/<username>/test/original         ` directory.
4.  Make sure the e-mail notification is sent to the email address that
    is specified. The message should contain the file data. The
    following screenshot displays a notification received.  
    ![](attachments/119129588/119129594.png)
