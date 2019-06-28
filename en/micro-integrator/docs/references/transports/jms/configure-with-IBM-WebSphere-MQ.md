# Configure with IBM WebSphere MQ

The WSO2 JMS transport can be configured with IBM® WebSphere® MQ. The
following topics cover the configuration steps.

-   [Prerequisites](#ConfigurewithIBMWebSphereMQ-Prerequisites)
-   [Creating queue manager, queue and channel in IBM WebSphere
    MQ](#ConfigurewithIBMWebSphereMQ-Creatingqueuemanager,queueandchannelinIBMWebSphereMQ)
-   [Generating the .bindings
    file](#ConfigurewithIBMWebSphereMQ-generateGeneratingthe.bindingsfile)
-   [Configuring the ESB JMS
    transport](#ConfigurewithIBMWebSphereMQ-ConfiguringtheESBJMStransport)
-   [Copying IBM Websphere MQ
    libraries](#ConfigurewithIBMWebSphereMQ-CopyingIBMWebsphereMQlibraries)
-   [Deploying JMS listener proxy
    service](#ConfigurewithIBMWebSphereMQ-DeployingJMSlistenerproxyservice)
-   [Testing the proxy
    service](#ConfigurewithIBMWebSphereMQ-Testingtheproxyservice)
-   [Sample Scenarios](#ConfigurewithIBMWebSphereMQ-SampleScenarios)

!!! note

From the below configurations, do the ones in the **axis2.xml** file
based on the profile you use as follows:

-   To enable the JMS transport in the Integration profile, edit the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file.
-   To enable the JMS transport in other profiles, edit the
    `          <EI_HOME>/wso2/<PROFILE_HOME>/conf/axis2/axis2.xml         `
    file. `          <PROFILE_HOME>         ` refers to the main
    directory of the profile inside the WSO2 EI distribution. For
    example, to enable the JMS transport in the Business Process
    profile, edit the
    `          <EI_HOME>/wso2/business-process/conf/axis2/axis2.xml         `
    file

!!! info The configuration steps below are for a Windows environment.

### Prerequisites

-   WSO2 Enterprise Interator (WSO2 EI) is installed. To test the
    samples, you must also have Apache Ant installed. For details, see
    [Installation
    Prerequisites](https://docs.wso2.com/display/EI650/Installation+Prerequisites)
    .
-   WebSphere MQ is installed and the latest fix pack applied (see the
    [IBM
    documentation](http://publib.boulder.ibm.com/infocenter/sametime/v8r0/index.jsp?topic=/com.ibm.help.sametime.advanced.doc/stv_inst_mq_appl_win_t.html)
    ). The fix pack can be obtained from
    <http://www-01.ibm.com/software/integration/wmq> . ( These
    instructions are tested on [IBM WebSphere MQ version
    8.0.0.4](http://www-01.ibm.com/support/docview.wss?uid=swg24040022)
    .)

### Creating queue manager, queue and channel in IBM WebSphere MQ

1.  Start IBM WebSphere MQ Explorer as an administrator. If you are not
    running on an administrator account, right-click on the IBM
    WebSphere MQ icon/menu item and then click **Run as Administrator**
    .
2.  Right-click on **Queue Managers** , move the cursor to **New** and
    then click **Queue Manager** to open the **Create Queue Manager**
    wizard. Enter ESBQManager as the queue manager name. Make sure you
    select **make this the default queue manager** check box. Leave the
    default values unchanged in the other fields. Click **Next** to move
    to the next page.
3.  Click **Next** in the page for entering data and log values without
    changing any default values.
4.  In the page for entering configuration options, select the
    following. Then click **Next** .

    | Field Name                                                  | Value                                                                                       |
    |-------------------------------------------------------------|---------------------------------------------------------------------------------------------|
    | **Start queue manager after it has been created** check box | If this is selected, the queue manager will start running immediately after it is created.  |
    | **Automatic** field                                         | If this is selected, the queue manager is automatically started when the machine starts up. |

5.  Configure the following in the page for entering listener options.

    | Field Name                                          | Value                                                                                                           |
    |-----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
    | **Create listener configured for TCP/IP** check box | Select this check box to create the listener.                                                                   |
    | **Listen on port number** field                     | Enter the number of the port where you want to set the listener. In this example, the port number will be 1414. |

6.  Click **Next** and then click **Finish** to save the configuration.
    The queue manager will be created as shown below.  
    ![](attachments/119130332/119130334.png)
7.  Expand the navigation tree of the ESBQManager queue manager in the
    navigation tree. Right-click on **Queues** , move the cursor to
    **New** and then click **Local** **Queue** to open the **Create a
    Local Queue** wizard. Enter the local queue name as
    `          LocalQueue1         ` and complete running the wizard.
    Leave the default values of all other fields unchanged, and click
    **Finish** to save the local queue.  
8.  Right-click on **Channels** , move the cursor to **New** , and then
    click **Server-connection Channel** to open the **Create a
    Server-connection Channel** wizard. Enter **myChannel** as the
    channel name and click **Next** . Make sure that the value for the
    **Transmission Protocol** is **TCP** . Leave the default values
    unchanged for the rest of the fields, and click **Finish** to save
    the channel.

### Generating the .bindings file

1.  Create a directory in which the `          .bindings         ` file
    can be saved in any location of your computer. In this example, a
    directory named `          jndidirectory         ` will be created
    in the `          G         ` folder.
2.  Go to IBM Websphere MQ, and right-click on **JMS Administered
    Objects** , and then click **Add Initial Context** .  
    ![](attachments/119130332/119130339.png)
3.  Select the **File system** option in the **Connection Details**
    wizard. Enter `          file:G/jndidirectory         ` in the
    **Context nickname** field. Leave the default values unchanged for
    other fields and complete running the wizard. The new file initial
    context will be displayed in the left navigator under **JMS
    Administered Objects** as shown below.  
    ![](attachments/119130332/119130338.png)
4.  Click the file initial context (named
    `          file:G/jndidirectory         ` in this example) in the
    navigator to expand it. Right-click on **Connection Factories** ,
    move the cursor to **New** , and then click **Connection Factory** .
    Enter the name of the connection factory as
    `          MyQueueConnectionFactory         ` . Select
    `          Queue Connection Factory         ` as the connection
    factory type. Select `          MQClient         ` as the transport.
    Leave the default values unchanged for other fields and complete
    running the wizard.
5.  Right-click on the newly connected connection factory in the left
    navigator, and the click **Properties** . Click **Connection** .
    Then browse and select `          ESBQManager         ` for the
    **Base queue manager** field. You can change the host and port name
    for the connection factory if required. No changes will be made in
    this example since default values are used. Leave the default values
    unchanged for other fields and click **OK** .
6.  Right-click **Destination** under **JMS Administered Objects** in
    the left navigator. Move the cursor to **New** and then click
    **Destination** to open the **New Destination** wizard. In order to
    map the destination to the local queue you created in step 7 of the
    [Creating queue manager, queue and channel in IBM WebSphere
    MQ](#ConfigurewithIBMWebSphereMQ-Qmanager) section, enter the same
    queue name ( `          LocalQueue1         ` in this example) in
    the **Name** field. Select `          Queue         ` for the
    **Type** field. Select `          ESBQManager         ` as the queue
    manager and `          LocalQueue1         ` as the queue in the
    wizard. Leave the default values unchanged for other fields and
    complete running the wizard.  

The .bindings file will be created in the location you specified (
`         file:G/jndidirectory        ` in this example) after you carry
out the above steps.

In order to connect to the queue, you need to configure channel
authentication. Run the following two commands to disable channel
authentication for the ease of use. Alternatively, you can configure the
authentication for the MQ server.

!!! info

Note that you have to run the command prompt as a admin user and run
these commands.


``` java
    runmqsc ESBQManager
```

``` java
    ALTER QMGR CHLAUTH(DISABLED)
```

``` java
    REFRESH SECURITY TYPE(CONNAUTH)
```

  
The following will be displayed in the command prompt.

![](attachments/119130332/119130336.png)

### Configuring the ESB JMS transport

!!! info

-   If you use the default configuration of the IBM MQ queue manager,
    you need to provide username and password client authentication. The
    username and password that you need to provide here is the username
    and password that you provide to log on to your operating system.
-   The `          vender.class.loader.enabled         ` parameter in
    the above configuration should be added only when you use IBM
    Websphere MQ as the JMS broker.
-   WSO2 uses some external class loader mechanisms for some external
    products such as QPID and AMQP due to the limitation of serializing
    the JMSObject message. However, it is not required to use this
    mechanism for IBM Websphere MQ. By adding the
    `          vender.class.loader.enabled         ` parameter, you can
    skip the external class loader for IBM Websphere MQ.
-   This property can also be included in a proxy service, REST API,
    message store, JMS receiver or the Synapse configuration depending
    on the use case.
-   **If you are using Windows Operating Systems (e.g., Windows 10)** ,
    mention the `          .bindings         ` file location starting
    with `          file:///         ` format, in the below
    `          <transportReceiver'>         ` and
    `          <transportSender>         ` configurations in the
    `          <EI_HOME>/repository/conf/axis2/axis2.xml         ` file
    as follows: `          <         ` `          parameter         `
    `          name         ` `          =         `
    `          "java.naming.provider.url"         `
    `          locked         ` `          =         `
    `          "false"         `
    `          >file:///G:/jndidirectory</         `
    `          parameter         ` `          >         `


1.  Add the following transport receiver to the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file.

    ``` xml
        <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
          <parameter name="default" locked="false">
            <parameter name="java.naming.factory.initial" locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
            <parameter name="java.naming.provider.url" locked="false">file:/G:/jndidirectory</parameter>
            <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MyQueueConnectionFactory</parameter>
            <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
            <parameter name="transport.jms.UserName" locked="false">nandika</parameter>
            <parameter name="transport.jms.Password" locked="false">password</parameter>
          </parameter>
    
    
          <parameter name="myQueueConnectionFactory1" locked="false">
            <parameter name="java.naming.factory.initial" locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
            <parameter name="java.naming.provider.url" locked="false">file:/G:/jndidirectory</parameter>
            <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MyQueueConnectionFactory</parameter>
            <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
            <parameter name="transport.jms.UserName" locked="false">nandika</parameter>
            <parameter name="transport.jms.Password" locked="false">password</parameter>
          </parameter>
        </transportReceiver>
    ```

2.  Add the following transport sender to the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file.

    ``` xml
            <transportSender name="jms" class="org.apache.axis2.transport.jms.JMSSender">
              <parameter name="default" locked="false">
                <parameter name="vender.class.loader.enabled">false</parameter>
                <parameter name="java.naming.factory.initial" locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/G:/jndidirectory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MyQueueConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.UserName" locked="false">nandika</parameter>
                <parameter name="transport.jms.Password" locked="false">password</parameter>
              </parameter>
        
              <parameter name="myQueueConnectionFactory1" locked="false">
                <parameter name="java.naming.factory.initial" locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/G:/jndidirectory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MyQueueConnectionFactory</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.UserName" locked="false">nandika</parameter>
                <parameter name="transport.jms.Password" locked="false">password</parameter>
              </parameter>
            </transportSender>
    ```

### Copying IBM Websphere MQ libraries

Follow the instructions below to build and install IBM WebSphere MQ
client JAR files to WSO2 EI.

!!! info

These instructions are tested on IBM WebSphere MQ version 8.0.0.4.
However, you can follow them for other versions appropriately.


1.  Create a new directory named `           wmq-client          ` , and
    then create another new directory named `           lib          `
    inside it.

2.  Copy the following JAR files from the
    `           <IBM_MQ_HOME>/java/lib/          ` directory (where
    `           <IBM_MQ_HOME>          ` refers to the IBM WebSphere MQ
    installation directory) to the
    `           wmq-client/lib/          ` directory.

        !!! note
    
        **Note:** If you are using IBM MQ 8 with Mutual SSL enabled, you
        need to download the
        [wmq-client-8.0.0.zip](attachments/119130332/119130333.zip)
        file and follow the instructions in the readme.txt file.
    

    -   `             com.ibm.mq.allclient.jar            `

    -   `             fscontext.jar            `

    -   `             jms.jar            `

    -   `             providerutil.jar            `

3.  Create a `           POM.xml          ` file inside the wmq
    `           -client/          ` directory and add all the required
    dependencies as shown in the example below.

        !!! tip
    
        You need to change the values of the
        `           <version>          ` and
        `           <systemPath>          ` properties accordingly.
    

    ``` java
        <?xml version="1.0"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <groupId>wmq-client</groupId>
        <artifactId>wmq-client</artifactId>
        <version>8.0.0.4</version>
        <packaging>bundle</packaging>
        <dependencies>
            <dependency>
                <groupId>com.ibm</groupId>
                <artifactId>fscontext</artifactId>
                <version>8.0.0.4</version>
                <scope>system</scope>
                <systemPath>${basedir}/lib/fscontext.jar</systemPath>
            </dependency>
            <dependency>
                <groupId>com.ibm</groupId>
                <artifactId>providerutil</artifactId>
                <version>8.0.0.4</version>
                <scope>system</scope>
                <systemPath>${basedir}/lib/providerutil.jar</systemPath>
            </dependency>
            <dependency>
                <groupId>com.ibm</groupId>
                <artifactId>allclient</artifactId>
                <version>8.0.0.4</version>
                <scope>system</scope>
                <systemPath>${basedir}/lib/com.ibm.mq.allclient.jar</systemPath>
            </dependency>
            <dependency>
                <groupId>javax.jms</groupId>
                <artifactId>jms</artifactId>
                <version>1.1</version>
                <scope>system</scope>
                <systemPath>${basedir}/lib/jms.jar</systemPath>
            </dependency>
        </dependencies>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.felix</groupId>
                    <artifactId>maven-bundle-plugin</artifactId>
                    <version>2.3.4</version>
                    <extensions>true</extensions>
                    <configuration>
                        <instructions>
                            <Bundle-SymbolicName>${project.artifactId}</Bundle-SymbolicName>
                            <Bundle-Name>${project.artifactId}</Bundle-Name>
                            <Export-Package>*;-split-package:=merge-first</Export-Package>
                            <Private-Package/>
                            <Import-Package/>
                            <Embed-Dependency>*;scope=system;inline=true</Embed-Dependency>
                            <DynamicImport-Package>*</DynamicImport-Package>
                        </instructions>
                    </configuration>
                </plugin>
            </plugins>
        </build>
        </project>
    ```

4.  Navigate to the wmq `           -client          ` directory using
    your Command Line Interface (CLI), and execute the following
    command, to build the project: mvn
    `           clean install          `

5.  Stop the WSO2 EI server, if it is already running.

6.  Remove any existing IBM MQ client JAR files from the
    `           <EI_HOME>/          ` dropins `          ` directory and
    the `           <EI_HOME>/lib          ` directory.

7.  Copy the
    `           <wmq-client>/target/wmq-client-8.0.0.4.jar          `
    file to the `           <EI_Home>/dropins          ` directory.

8.  Download the [`            jta.jar           ` file from the maven
    repository](http://central.maven.org/maven2/javax/transaction/jta/1.1/jta-1.1.jar)
    , and copy it to the `           <EI_HOME>/lib          ` directory.

9.  Remove following line from the
    `          <EI_HOME>/conf/etc/launch.         ` ini file:
    `          j          avax.jms,\         `
10. [Regenerate `            .bindings           `
    file](#ConfigurewithIBMWebSphereMQ-generate) with the
    `           Provider Version : 8          ` property (if you already
    generated one before), and replace the existing
    `           .bindings          ` file (if you have one) with the new
    `           .bindings          ` file you generated.

11. Start the WSO2 EI server.

### Deploying JMS listener proxy service

In this section, the following simple proxy service is deployed to
listen to the `         LocalQueue1        ` queue. When a message is
published in this queue, the proxy service would pull the message out of
the queue and log it. See [Working with Proxy
Services](https://docs.wso2.com/display/EI650/Working+with+Proxy+Services)
for detailed instructions to create a proxy service.

``` xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="MyJMSProxy"
           transports="jms"
           startOnLoad="true"
           trace="disable">
       <description/>
       <target>
          <inSequence>
             <log level="full"/>
             <drop/>
          </inSequence>
       </target>
       <parameter name="transport.jms.Destination">LocalQueue1</parameter>
    </proxy>
```

### Testing the proxy service

Open IBM Websphere MQ and publish a message to
`         LocalQueue1        ` .

![](attachments/119130332/119130337.png)

The message will be logged in the WSO2 EI's Management console as well
as the log file.

### Sample Scenarios

This section describes how to configure the following sample scenarios
using the JMS transport, WebSphere MQ, and WSO2 EI:

-   [Queue Scenario 1](#ConfigurewithIBMWebSphereMQ-QueueScenario1) :
    JMS Client -\> Queue -\> WSO2 EI -\> Axis2server
-   [Queue Scenario 2](#ConfigurewithIBMWebSphereMQ-QueueScenario2) :
    JMS Client -\> WSO2 EI -\> Queue -\> Axis2server
-   [Topic Scenario 1](#ConfigurewithIBMWebSphereMQ-TopicScenario1) :
    JMS Client -\> Topic -\> WSO2 EI -\> Axis2server
-   [Topic Scenario 2](#ConfigurewithIBMWebSphereMQ-TopicScenario2) :
    JMS Client -\> WSO2 EI -\> Topic -\> Axis2server

In scenarios where the client places the message directly on the queue
or topic, and the message is then picked up by WSO2 EI, you configure
the non-default connection factories in
`         <EI_HOME>\conf\axis2\axis2.xml        ` and comment them out
in the
`         <EI_HOME>\samples\axis2Server\repository\conf\axis2.xml        `
file. In scenarios where the client sends the message to WSO2 EI first,
and WSO2 places the message on the queue or topic, you configure the
non-default connection factories in
`         <EI_HOME>\samples\axis2Server\repository\conf\axis2.xml        `
and comment them out in
`         <EI_HOME>\conf\axis2\axis2.xml        ` .

#### Queue Scenario 1: Client to Queue to WSO2 EI

In this scenario, the JMS client places an order on the JMS\_QUEUE
queue. WSO2 EI listens on this queue, gets the message, and sends it to
the back-end server to process.

1.  In `           <EI_HOME>\conf\axis2\axis2.xml          ` , comment
    out the myTopicConnectionFactory parameter and uncomment the
    SQProxyCF parameter. It should look as shown below.

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                <!--parameter name="myTopicConnectionFactory" locked="false"> 
                  <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter--> 
        
                <parameter name="SQProxyCF" locked="false">
                  <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
        
                <parameter name="default" locked="false">
                  <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
            </transportReceiver>
    ```

2.  If you are using version 6.0 of IBM WebSphere MQ, add the following
    parameter to axis2.xml to ensure that JMS Spec version 1.1.2b is
    used instead of version 1.1:

    ``` java
            <parameter name="transport.jms.JMSSpecVersion">1.0.2b</parameter>
    ```

3.  Start WSO2 EI with the [Sample
    250](https://docs.wso2.com/display/ESB500/Sample+250%3A+Introduction+to+Switching+Transports)
    configuration by running the following command.  
    `           wso2ei-samples.bat -sn 250          `

4.  Log in to the server management console at:
    <https://localhost:9443/carbon/> .

5.  Click **Services -\> list -\> StockQuoteProxy -\> edit (Specific
    Configuration)**

6.  Add a service parameter as follows and save it.  
    `           name = transport.jms.ConnectionFactory value = SQProxyCF          `

7.  Go to the `           <EI_HOME>/samples/axis2Client          `
    directory and build it using the `           ant          ` command.

8.  Go to the
    `           <EI_HOME>/samples/axis2Client/src/samples/userguide          `
    directory, open the `           GenericJMSClient.java          `
    source file, and make the following changes in the code:

    1.  Set the jms\_dest property default value to
        `             JMS_QUEUE            ` (line 45)

    2.  Set the java.naming.provider.url to
        `             file:/C:/JNDI-Directory            ` (line 82)

    3.  Set the java.naming.factory.initial to
        `             com.sun.jndi.fscontext.RefFSContextFactory            `
        (line 85)

    4.  Set the lookup key to `             MQ_JMS_MANAGER            `
        (line 89)

9.  Configure the proxy configuration so that it appears as follows.

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
              <proxy name="StockQuoteProxy" transports="https http jms" startOnLoad="true" trace="disable">
                <target>
                  <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                  </endpoint>
                  <inSequence>
                    <property name="OUT_ONLY" value="true"/>
                  </inSequence>
                  <outSequence>
                    <send/>
                  </outSequence>
                </target>
                <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
                <parameter name="transport.jms.ContentType">
                  <rules>
                    <jmsProperty>contentType</jmsProperty>
                    <default>application/xml</default>
                  </rules>
                </parameter>
                <parameter name="transport.jms.ConnectionFactory">SQProxyCF</parameter>
              </proxy>
              <sequence name="fault">
                <log level="full">
                  <property name="MESSAGE" value="Executing default "fault" sequence"/>
                  <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                  <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
                </log>
                <drop/>
              </sequence>
              <sequence name="main">
                <log/>
                <drop/>
              </sequence>
            </definitions>
    ```

10. Configure
    `           <EI_HOME>\samples\axis2Server\repository\conf\axis2.xml          `
    so that it looks as follows.

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
              <!--parameter name="myTopicConnectionFactory" locked="false">
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
              </parameter-->
        
              <!--parameter name="SQProxyCF" locked="false">
                <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
              </parameter-->
        
              <parameter name="default" locked="false">
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
              </parameter>
            </transportReceiver>
    ```

11. Start the axis2 Server with the following command.  
    `           axis2Server.bat          `

12. Send the request from the JMS client, and the sample Axis2 server
    console will print a message.  
    `           ant jmsclient -Djms_type=pox -Djms_dest=JMS_QUEUE -Djms_payload=MSFT          `

#### Queue Scenario 2: Client to WSO2 EI to Queue

In this scenario, the JMS client places an order to the WSO2 EI, which
then places it on the queue. The back-end server listens on this queue,
and then gets the message and processes the request.

1.  In `           <EI_HOME>\conf\axis2\axis2.xml          ` , comment
    out both the myTopicConnectionFactory parameter and the SQProxyCF
    parameter. It should look as shown below.

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                <!--parameter name="myTopicConnectionFactory" locked="false"> 
                  <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter--> 
        
                <!--parameter name="SQProxyCF" locked="false">
                  <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter-->
        
                <parameter name="default" locked="false">
                  <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
            </transportReceiver>
    ```

2.  Start the WSO2 EI with the [Sample
    251](https://docs.wso2.com/display/ESB500/Sample+251%3A+Switching+from+HTTP%28S%29+to+JMS)
    configuration using the following command.  
    `           wso2ei-samples.bat -sn 251          `

3.  Log into the WSO2 EI management console at: [https://localhost: 8243
    /carbon/](https://localhost:9443/carbon/) .
4.  Select **Service Bus -\> Source view** and update the JMS URL as
    follows.  
    `          jms:/JMS_QUEUE?transport.jms.ConnectionFactoryJNDIName=MQ_JMS_MANAGER&java.naming.factory.initial=com.sun.jndi.fscontext.RefFSContextFactory&java.naming.provider.url=file:/C:/JNDI-Directory&transport.jms.DestinationType=queue&transport.jms.ConnectionFactoryType=queue &transport.jms.Destination=JMS_QUEUE         `
5.  Configure the proxy service as follows.

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
              <proxy name="StockQuoteProxy" transports="https http jms" startOnLoad="true" trace="disable">
                <target>
                  <endpoint>
                    <address uri="jms:/JMS_QUEUE?transport.jms.ConnectionFactoryJNDIName=MQ_JMS_MANAGER&amp;java.naming.factory.initial=com.sun.jndi.fscontext.RefFSContextFactory&amp;java.naming.provider.url=file:/C:/JNDI-Directory&amp;transport.jms.DestinationType=queue&transport.jms.ConnectionFactoryType=queue&amp;transport.jms.Destination=JMS_QUEUE"/>
                  </endpoint>
                  <inSequence>
                    <property name="TRANSPORT_HEADERS" scope="axis2" action="remove"/>
                    <property name="OUT_ONLY" value="true"/>
                  </inSequence>
                  <outSequence>
                    <send/>
                  </outSequence>
                </target>
                <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
              </proxy>
              <sequence name="fault">
                <log level="full">
                  <property name="MESSAGE" value="Executing default "fault" sequence"/>
                  <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                  <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
                </log>
                <drop/>
              </sequence>
              <sequence name="main">
                <log/>
                <drop/>
              </sequence>
            </definitions>
    ```

6.  Comment out `           myTopicConnectionFactory          ` and
    uncomment `           SQProxyCF          ` in the
    `           <EI_HOME>\samples\axis2Server\repository\conf\axis2.xml          `
    file as follows.

    ``` html/xml
              <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                <!--parameter name="myTopicConnectionFactory" locked="false"> 
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                 <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                 <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter-->
        
                <parameter name="SQProxyCF" locked="false">
                 <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                 <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                 <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                 <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                 <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
        
                <parameter name="default" locked="false"> 
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                 <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                 <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
              </transportReceiver>
    ```

7.  Start the axis2 Server with the following command.  
    `           axis2Server.bat          `

8.  Add the following parameters to `           service.xml          `
    in
    `           <EI_HOME>\samples\axis2Server\repository\services\SimpleStockQuoteService.aar.          `

    ``` java
            <parameter name="transport.jms.ConnectionFactory">SQProxyCF</parameter>
            <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
    ```

9.  Send the request from the JMS client, and the sample Axis2 server
    console will print a message as follows.

    ``` plain
            ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=placeorder -Dsymbol=MSFT 
    ```

#### Topic Scenario 1: Client to Topic to WSO2 EI 

In this scenario, the JMS client places an order on the topic ivtT. The
WSO2 EI listens to this topic, gets the message, and sends it to the
back-end server to process the request.

1.  In `           <EI_HOME>           \conf\axis2\axis2.xml          `
    , uncomment the `           myTopicConnectionFactory          `
    parameter and comment out the `           SQProxyCF          `
    parameter. It should look as shown below.

    ``` xml
              <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                <parameter name="myTopicConnectionFactory" locked="false"> 
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                 <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                 <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
        
                <!--parameter name="SQProxyCF" locked="false">
                 <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                 <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                 <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                 <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                 <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter-->
        
                <parameter name="default" locked="false"> 
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                 <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                 <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter>
              </transportReceiver>
    ```

2.  S tart WSO2 EI with the [Sample
    250](https://docs.wso2.com/display/ESB500/Sample+250%3A+Introduction+to+Switching+Transports)
    configuration by running the following command.  
    `           wso2ei-samples.bat -sn 250          `

3.  L og in to the server management console at: [https://localhost:
    8243 /carbon/](https://localhost:9443/carbon/) .

4.  Click **web services -\> list -\> StockQuoteProxy -\> edit (Specific
    Configuration)**

5.  Add a service parameter as follows and save it.  
    `           name = transport.jms.ConnectionFactory value = myTopicConnectionFactory          `  

6.  Go to the `           <EI_HOME>/samples/axis2Client          `
    directory and build it using the `           ant          ` command.

7.  Go to the `           <EI_HOME>/samples/axis2Client          `
    `           /src/samples/userguide          ` directory, open the
    `           GenericJMSClient.java          ` source file, and make
    the following changes in the code.

    1.  Set the jms\_dest property default value to
        `             ivtT            ` (line 45)

    2.  Set the java.naming.provider.url to
        `             file:/C:/JNDI-Directory            ` " (line 82)

    3.  Set the java.naming.factory.initial to
        `             com.sun.jndi.fscontext.RefFSContextFactory            `
        (line 85)

    4.  Set the lookup key to `             MQ_JMS_MANAGER            `
        (line 89)

8.  Configure the proxy configuration so that it appears as follows .

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
              <proxy name="StockQuoteProxy" transports="https http jms" startOnLoad="true" trace="disable">
                <target>
                  <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                  </endpoint>
                  <inSequence>
                    <property name="OUT_ONLY" value="true"/>
                  </inSequence>
                  <outSequence>
                    <send/>
                  </outSequence>
                </target>
                <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
                <parameter name="transport.jms.ContentType">
                  <rules>
                    <jmsProperty>contentType</jmsProperty>
                    <default>application/xml</default>
                  </rules>
                </parameter>
                <parameter name="transport.jms.ConnectionFactory">myTopicConnectionFactory</parameter>
              </proxy>
              <sequence name="fault">
                <log level="full">
                  <property name="MESSAGE" value="Executing default "fault" sequence"/>
                  <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                  <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
                </log>
                <drop/>
              </sequence>
              <sequence name="main">
                <log/>
                <drop/>
              </sequence>
            </definitions>
    ```

9.  Comment out the non-default connection factories in the
    `           <EI_HOME>\samples\axis2Server\repository\conf\axis2.xml          `
    file so that it looks as follows.

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
              <!--parameter name="myTopicConnectionFactory" locked="false">
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
              </parameter-->
        
              <!--parameter name="SQProxyCF" locked="false">
                <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
              </parameter-->
        
              <parameter name="default" locked="false">
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/D/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">bogusq</parameter>
              </parameter>
            </transportReceiver>
    ```

10. Start the axis2 server with the following command.  
    `           axis2Server.bat          `

11. Send the request from the JMS client, and the sample Axis2 server
    console will print a message.

#### Topic Scenario 2: Client to WSO2 EI to Topic

In this scenario, the JMS client sends an order to the WSO2 EI, which
places it on the topic `         ivtT        ` . The back-end server
listens on this topic, and then gets the message and processes the
request.

1.  In `           <EI_HOME>\conf\axis2\axis2.xml          ` , comment
    out the non-default connection factories as follows:

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
                <!--parameter name="myTopicConnectionFactory" locked="false"> 
                  <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                  <parameter name="transport.jms.Destination">ivtT</parameter>
                </parameter--> 
        
                <!--parameter name="SQProxyCF" locked="false">
                  <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                  <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
                </parameter-->
        
                <parameter name="default" locked="false">
                  <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                  <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                  <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                  <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                  <parameter name="transport.jms.Destination">bogusq</parameter>
                </parameter>
            </transportReceiver>
    ```

2.  Start the WSO2 EI with the [Sample
    251](https://docs.wso2.com/display/ESB500/Sample+251%3A+Switching+from+HTTP%28S%29+to+JMS)
    configuration by running the following command.  
    `           wso2ei-samples.bat -sn 251          `

3.  Log into WSO2 EI server management console at [https://localhost:
    8243 /carbon/](https://localhost:9443/carbon/) .

4.  Click **Service Bus -\> Source view** , and in the JMS URL, change
    transport.jms.DestinationType to `           topic.          `

5.  Add the following parameters to service.xml in
    `           <EI_HOME>\samples\axis2Server\repository\services\SimpleStockQuoteService.aar.          `

    ``` java
            <parameter name="transport.jms.ConnectionFactory">myTopicConnectionFactory</parameter>
            <parameter name="transport.jms.Destination">ivtT</parameter>
    ```

6.  Configure the proxy service so that it appears as follows.

    ``` html/xml
            <definitions xmlns="http://ws.apache.org/ns/synapse">
              <proxy name="StockQuoteProxy" transports="https http jms" startOnLoad="true" trace="disable">
                <target>
                  <endpoint>
                    <address uri="jms:/ivtT?transport.jms.ConnectionFactoryJNDIName=MQ_JMS_MANAGER&amp;java.naming.factory.initial=com.sun.jndi.fscontext.RefFSContextFactory&amp;java.naming.provider.url=file:/C:/JNDI-Directory&amp;transport.jms.DestinationType=topic&amp;transport.jms.ConnectionFactoryType=topic&transport.jms.Destination=ivtT"/>
                  </endpoint>
                  <inSequence>
                    <property name="TRANSPORT_HEADERS" scope="axis2" action="remove"/>
                    <property name="OUT_ONLY" value="true"/>
                  </inSequence>
                  <outSequence>
                    <send/>
                  </outSequence>
                </target>
                <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
              </proxy>
              <sequence name="fault">
                <log level="full">
                  <property name="MESSAGE" value="Executing default "fault" sequence"/>
                  <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
                  <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
                </log>
                <drop/>
              </sequence>
              <sequence name="main">
                <log/>
                <drop/>
              </sequence>
            </definitions>
    ```

7.  In
    `           <EI_HOME>\samples\axis2Server\repository\conf\axis2.xml          `
    , uncomment `           myTopicConnectionFactory          ` and
    comment out `           SQProxyCF          ` .

    ``` html/xml
            <transportReceiver name="jms" class="org.apache.axis2.transport.jms.JMSListener">
              <parameter name="myTopicConnectionFactory" locked="false">
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/C:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">topic</parameter>
                <parameter name="transport.jms.Destination">ivtT</parameter>
              </parameter>
        
              <!--parameter name="SQProxyCF" locked="false">
                <parameter name="java.naming.factory.initial">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url">file:/D:/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">JMS_QUEUE</parameter>
              </parameter-->
        
              <parameter name="default" locked="false">
                <parameter name="java.naming.factory.initial" 
            locked="false">com.sun.jndi.fscontext.RefFSContextFactory</parameter>
                <parameter name="java.naming.provider.url" locked="false">file:/D/JNDI-Directory</parameter>
                <parameter name="transport.jms.ConnectionFactoryJNDIName" locked="false">MQ_JMS_MANAGER</parameter>
                <parameter name="transport.jms.ConnectionFactoryType" locked="false">queue</parameter>
                <parameter name="transport.jms.Destination">bogusq</parameter>
              </parameter>
            </transportReceiver>
    ```

8.  Start the axis2 server with the following command.  
    `           axis2Server.bat          `

9.  Send the request from the JMS client, and the sample Axis2 server
    console will print a message.

For implementation details of other JMS use cases, see [JMS
Usecases](_JMS_Usecases_) .
