# SAP Integration

**Systems, Applications, and Products (SAP)** for data processing is an
industry leading enterprise software solution that is widely used in
product and process oriented enterprises for finance, operations, HR and
many other aspects of a business. SAP ERP solutions provide reliable and
efficient platforms to build and integrate enterprise or business-wide
data and information systems with ease.

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) leverages the
best of both worlds by providing the integration layer so that an
existing SAP R/3 based solutions of an enterprise can be integrated with
other data/business oriented systems so that you can mix-and-match
requirements with minimal effort. As a result, enterprises can keep
parts of their systems independent of SAP and extensible for many other
systems, solutions and middleware.

The WSO2 SAP adapter is shipped with the ESB profile and is implemented
as a transport for the ESB profile. This is provided in the
`         <EI_HOME>/wso2/components/plugins        ` directory
as org.wso2.carbon.transports.sap-VERSION.jar (e.g. org.wso2.carbon.transports.sap\_1.0.0.jar).

The WSO2 SAP adapter has full IDoc and experimental BAPI support. It
uses the SAP JCO library as the underlying framework to communicate with
SAP.  This section describes how to set up the ESB profile in a SAP
environment, how to install the SAP JCo middleware library, SAP
Intermediate Document (IDoc) and Business Application Programming
Interface (BAPI) adapters.

------------------------------------------------------------------------

[Installing the SAP Adapter](#SAPIntegration-InstallingtheSAPAdapter) \|
[Setting up the Client Configuration
File](#SAPIntegration-SettinguptheClientConfigurationFile) \| \|
[Setting up the Server Configuration
File](#SAPIntegration-SettinguptheServerConfigurationFile) \|
[Configuring WSO2 SAP
Adapter](#SAPIntegration-ConfiguringWSO2SAPAdapter) \| [Additional
Configuration
Parameter](#SAPIntegration-AdditionalConfigurationParameter) \|
[Troubleshooting](#SAPIntegration-Troubleshooting)

------------------------------------------------------------------------

### Installing the SAP Adapter

Follow the instructions below to install and set up the SAP adapter.

1.  Download WSO2 EI and install the ESB Profile by following the
    instruction in the [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
2.  Download the `           sapidoc3.jar          ` and
    `           sapjco3.jar          ` middleware libraries from the SAP
    support portal and copy those libraries to the
    `           <EI_HOME>/lib          ` directory.

        !!! info
    
        Note
    
        You need to have SAP login credentials to access the SAP support
        portal.
    

3.  Download the native SAP JCo library and copy it to the system path.
    You need to select the system path applicable to your operating
    system as described below.

    |              |                                                                                                                                                                                     |
    |--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Linux 32-bit | Copy the Linux native SAP jcolibrary `                               libsapjco3.so                             ` to `               <JDK_HOME>/jre/lib/i386/server              ` . |
    | Linux 64-bit | Copy the Linux native SAP jcolibrary `                               libsapjco3.so                             ` to `               <JDK_HOME>/jre/lib/amd64              ` .       |
    | Windows      | Copy the Windows native SAP jcolibrary `               sapjco3.dll              ` to `               <WINDOWS_HOME>/system32              ` .                                       |

4.  Create a directory named `           conf          ` in the
    `           <EI_HOME>/conf/          ` directory.

5.  Create a directory named `           sap          ` within the
    `           conf          ` directory that you created and provide
    access rights to read the properties files you will save in it
    later.

6.  Copy the following SAP endpoint properties files to the
    `           <EI_HOME>/conf/sap/          ` directory. You need to
    have two properties files, one at the server-end and the other at
    the client-end to communicate with an external SAP endpoint using
    IDoc or BAPI.

    -   `                         *.dest                       ` : This
        is where we store SAP endpoint parameters when the ESB Profile
        of WSO2 EI is configured as a client to an external SAP
        endpoint.
    -   `                         *.server                       ` :
        This is where we store SAP endpoint parameters when the ESB
        Profile of WSO2 EI is configured as a server to an external SAP
        endpoint.

    For details on creating the properties files and defining the
    relevant properties, see [Setting up the Client Configuration
    File](#SAPIntegration-SetupClientPropertyFiles) and [Setting up the
    Server Configuration File](#SAPIntegration-SetupServerPropertyFiles)
    .

7.  Start the ESB profile of WSO2 EI using the
    `           -Djava.library.path          ` switch to specify the
    location of your SAP jco library.  
    For example, you can execute the following command to start the ESB
    Profile:

    ``` java
        ./integrator.sh -Djava.library.path=/usr/lib/jvm/jre1.7.0/lib/i386/server/
    ```

------------------------------------------------------------------------

### Setting up the Client Configuration File

To setup the ESB profile as a client to a SAP system you need to create
the `         *.dest        ` properties file and define the relevant
properties. The following table lists the properties and the description
of each property that should be specified in the
`         *.dest        ` properties file.

| Property                                                    | Description                                                        |
|-------------------------------------------------------------|--------------------------------------------------------------------|
| **`               jco.client.client              `**        | Client logon                                                       |
| **`               jco.client.user              `**          | User logon                                                         |
| **`               jco.client.alias_user              `**    | Alias user name                                                    |
| **`               jco.client.passwd              `**        | Logon password                                                     |
| **`               jco.client.lang              `**          | Logon language                                                     |
| **`               jco.client.sysnr              `**         | R/3 system number                                                  |
| **`               jco.client.ashost              `**        | R/3 application server                                             |
| **`               jco.client.mshost              `**        | R/3 message server                                                 |
| **`               jco.client.gwhost              `**        | Gateway host                                                       |
| **`               jco.client.gwserv              `**        | Gateway service                                                    |
| **`               jco.client.r3name              `**        | R/3 name                                                           |
| **`              jco.client.group             `**           | Group of application servers                                       |
| **`               jco.client.tpname              `**        | Program ID of external server program                              |
| **`              jco.client.tphost             `**          | Host of external server program                                    |
| **`               jco.client.type              `**          | Type of remote host (3=R/3, E=External)                            |
| **`              jco.client.codepage             `**        | Initial code page for logon                                        |
| **`               jco.client.use_sapgui              `**    | Use remote SAP graphical user interface                            |
| **`               jco.client.mysapsso2              `**     | Use the specified SAP cookie version 2 as the logon ticket         |
| **`              jco.client.grt_data             `**        | Additional data for GUI                                            |
| **`               jco.client.use_guihost              `**   | Host to which the remote GUI is redirected                         |
| **`               jco.client.use_guiserv              `**   | Service to which the remote GUI is redirected                      |
| **`              jco.client.use_guiprogid             `**   | Progid of the server that starts the remote GUI                    |
| **`              jco.client.snc_partnername             `** | SNC partner name (for example, CN=B20, O=SAP-AG, C=DE\\) snc\_mode |
| **`               jco.client.snc_mode              `**      | SNC mode (0 or 1)                                                  |
| **`               jco.client.snc_qop              `**       | SNC level of security (1-9)                                        |
| **`               jco.client.snc_myname              `**    | SNC name; overrides default SNC partner                            |
| **`              jco.client.snc_lib             `**         | Path to the library                                                |
| **`               jco.client.Dest              `**          | R/2 destination                                                    |
| **`               jco.client.saplogon_id              `**   | SAPLOGON string on 32-bit Windows                                  |
| **`               jco.client.extiddata              `**     | Data for external application (PAS)                                |
| **`               jco.client.extidtype              `**     | Type of external authentication (PAS)                              |
| **`               jco.client.x509cert              `**      | Use the specified X509-certificate as the logon ticket             |
| **`               jco.client.msserv              `**        | R/3 port number of message server                                  |
| **`               jco.client.profile_name              `**  | Profile name used for shared memory communication                  |
| **`               jco.client.idle_timeout              `**  | Idle timeout for the connection                                    |
| **`               jco.client.ice Ignore              `**    | RFC library character conversion errors (1 or 0)                   |
| **`               jco.client.logon              `**         | Enable or disable logon check at open time (1 or 0)                |
| **`               jco.client.trace              `**         | Enable or disable RFC trace (1 or 0)                               |
| **`               jco.client.abap_debug              `**    | Enable ABAP debugging (1 or 0)                                     |
| **`               jco.client.getsso2              `**       | Get or do not get a SSO ticket after logon (1 or 0)                |
| **`               jco.client.toupper              `**       | Enable or disable uppercase character conversions for logon        |

!!! info

Note

You can obtain the values for these properties from your SAP system
administrator.


The `         *.dest        ` properties file should be named
`         <SAP-GWHOST>.dest        ` . For example, if the name of your
SAP gateway is `         SAPSYS        ` , the name of the file should
be `         SAPSYS.dest.        `

Following is a sample configuration for the `         *.dest        `
properties file:

``` java
    jco.client.client=800
    jco.client.user=wso2_user
    jco.client.passwd=wso2pass14
    jco.client.lang=en
    jco.client.ashost=/H/217.116.29.154/S/3299/H/10.100.5.120/S/3200
    jco.client.gwserv=3300
    jco.client.sysnr=00
    jco.client.idle_timeout=300
    jco.client.logon=0
    jco.client.msserv=3600
    jco.client.trace=0
    jco.client.getsso2=0
    jco.client.r3name=CPT
```

### Setting up the Server Configuration File

To setup the ESB Profile of WSO2 EI as an IDoc server, you need to
create the `         *.server        ` properties file and define the
relevant properties. The following table lists the properties and the
description of each property that should be specified in the
`         *.server        ` properties file.

| Property                                                                      | Description                                                                                                                                                                                                  |
|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`               jco.server.gwhost              `**                          | Gateway host                                                                                                                                                                                                 |
| **`               jco.server.gwserv              `**                          | Gateway service                                                                                                                                                                                              |
| **`               jco.server.progid              `**                          | Program ID of the server                                                                                                                                                                                     |
| **`               jco.server.trace              `**                           | You can enable or disable the RFC trace                                                                                                                                                                      |
| **`              jco.server.repository_destination             `**            | Name of the `             .dest            ` file. For example, if the `             .dest            ` file is `             SAPSYS01.dest            ` , set this to `             SAPSYS01            ` . |
| **`              jco.server.params             `**                            | Arbitrary parameters for RFC library                                                                                                                                                                         |
| **`               jco.server.snc_myname              `**                      | SNC name                                                                                                                                                                                                     |
| **`               jco.server.snc_qop              `**                         | SNC level of security (1-9)                                                                                                                                                                                  |
| **`               jco.server.snc_lib              `**                         | Path to the SNC library                                                                                                                                                                                      |
| **`               jco.server.profile_name              `**                    | Name of the profile file used during start-up                                                                                                                                                                |
| **`               jco.server.unicode              `**                         | Determines whether or not you connect in unicodemode (1=true, 0=false)                                                                                                                                       |
| **`               jco.server.max_startup_delay              `**               | Maximum server start-up delay time in seconds                                                                                                                                                                |
| **`              jco.server.connection_count             `**                  | Number of SAP to the ESB connections                                                                                                                                                                         |
| **`                             jco.server.name                           `** | Name of the server configuration. This needs to be the same name provided in the SAP configuration.                                                                                                          |

!!! info

Note

You can obtain the values for these properties from your SAP system
administrator.


This file should be named `         <SAP-GWHOST>.server        ` . For
example, if the name of your SAP gateway is `         SAPSYS        ` ,
the name of the file should be `         SAPSYS.server.        `

Following is a sample configuration for the `         *.server        `
properties file:

``` java
    jco.server.gwhost=/H/217.116.29.154/S/3299/H/10.100.5.120/S/3200
    jco.server.gwserv=3300
    jco.server.progid=IGS.CPT
    jco.server.repository_destination=IGS.CPT
    jco.server.name=IGS.CPT
    jco.server.unicode=1
```

### Configuring WSO2 SAP Adapter

Go to the required tab for detailed steps based on how you need to
configure WSO2 SAP Adapter.

-   [**Configure with IDoc adapter**](#a7885a2cc75a4aed8df01f1412360011)
-   [**Configure with BAPI adapter**](#fc931f5e130e4b17b48fd8ab87ea0e27)

WSO2 SAP adapter can be used with IDoc, which is a synchronous interface
used when exchanging data with the SAP system. WSO2 EI can be configured
for [Sending IDocs](#SAPIntegration-SendingIDocs) or [Receiving
IDocs](#SAPIntegration-ReceivingIDocs) when using the SAP adapter.

##### Sending IDocs

Follow the instructions below to configure the ESB Profile as an IDoc
client using the SAP adapter.

1.  Uncomment the following line in
    `              <EI_HOME>/conf/axis2/axis2.xml             ` file to
    enable the IDoc transport sender on axis2 core.

    ``` xml
            <transportSender name=”idoc” class="org.wso2.carbon.transports.sap.SAPTransportSender"/>
    ```

2.  Create `              IDocSender             ` proxy service with
    the following configuration:

    ``` xml
            <proxy xmlns=http://ws.apache.org/ns/synapse 
                   name="IDocSender"
                   transports="http" 
                   startOnLoad="true" 
                   trace="enable" 
                   statistics="enable">
                <target>
                  <inSequence>
                     <log level="full"/>
                     <send>
                     <endpoint name="sapidocendpoint">
                         <address uri="idoc:/SAPSYS"/>
                     </endpoint>
                     </send>
                  </inSequence>
                  <outSequence/>
               </target>
               <parameter name="serviceType">proxy</parameter>
               <description/>
            </proxy>
    ```

        !!! info
    
        -   If you set the property shown below (use the **Property**
            mediator before the **Send** mediator in the above sequence),
            any business-level error messages that are sent back from the
            SAP endpoint will be successfully passed through the out flow
            sequence. Without this property, the business-level errors from
            SAP can get detected as faulty messages and passed to the Fault
            sequence.
    
        ``` java
                <property name="sap.escape.error.handling" scope="axis2" value="true"/>
        ```
        
            -   The SAP endpoint client properties file
                `               SAPSYS.dest              ` should be in
                `               <EI_HOME>/conf/sap              ` folder .
            -   Additional axis2 level sender parameters that can be defined in
                the axis2 core are listed in [SAP Transport Sender
                Parameters](#SAPIntegration-SenderParameters) .
        

    You can now send IDocs using the configured WSO2 SAP adapter.

##### Receiving IDocs

Follow the instructions below to configure the ESB Profile as an IDoc
server using the SAP adapter.

1.  Uncomment the following line in
    `              <EI_HOME>/conf/axis2/axis2.xml             ` file to
    enable IDoc transport receiver in axis2 core.

    ``` xml
        <transportReceiver name=”idoc” class="org.wso2.carbon.transports.sap.SAPTransportListener"/>
    ```

2.  Ensure the server configuration file
    `             SAPSYS.server            ` is available in
    `             <EI_HOME>/conf/sap            ` folder.
3.  Start the ESB Profile of WSO2 EI using the
    `              -Djava.library.path             ` switch to specify
    the location of your SAP jco library.  
    For example, you can execute the following command to start the ESB
    Profile:

    ``` java
            ./integrator.sh -Djava.library.path=/usr/lib/jvm/jre1.7.0/lib/i386/server/
    ```

    `                           `

4.  Create the `              IDocReceiver             ` proxy service
    with the following configuration:

    ``` xml
            <proxy xmlns=http://ws.apache.org/ns/synapse 
                   name="IDocReceiver"
                   transports="idoc" 
                   statistics="enable" 
                   trace="enable" 
                   startOnLoad="true">
               <target>
                 <inSequence>
                     <log level="full"/>
                     <drop/>
                </inSequence>
                <outSequence>
                    <log level="full"/>
                    <send/>
                </outSequence>
              </target>
              <parameter name="transport.sap.enableTIDHandler">enabled</parameter>
              <parameter name="transport.sap.serverName">SAPSYS</parameter>
              <description/>
            </proxy>
    ```

        !!! info
    
        -   The SAP endpoint server properties file
            `               SAPSYS.server              ` should be in the
            `               <EI_HOME>/conf/sap              ` folder.
        -   Additional proxy level listener parameters that can be defined
            in the proxy configuration are listed in [Proxy Service Listener
            Parameters](#SAPIntegration-ListenerParameters) .
    

    Once the proxy service configuration is saved, WSO2 SAP adapter is
    now ready to receive IDoc messages.

WSO2 SAP adapter so that it can be used with BAPI, which is a
synchronous interface used when exchanging data with the SAP system. The
ESB Profile of WSO2 EI can be configured for [Sending
BAPIs](#SAPIntegration-SendingBAPI) or [Receiving
BAPIs](#SAPIntegration-ReceivingBAPI) when using the SAP adapter.

##### Sending BAPIs

Follow the instructions below to configure the ESB profile as a BAPI
client using the SAP adapter.

1.  Uncomment the following line in
    `               <EI_HOME>/conf/axis2/axis2.xml              ` file
    to enable the BAPI transport sender in axis2 core.

    ``` xml
        <transportSender name="bapi" class="org.wso2.carbon.transports.sap.SAPTransportSender"/>
    ```

2.  Start the ESB Profile of WSO2 EI using the
    `              -Djava.library.path             ` switch to specify
    the location of your SAP jco library.  

    For example, you can execute the following command to start the ESB
    Profile:

    ``` java
            ./integrator.sh -Djava.library.path=/usr/lib/jvm/jre1.7.0/lib/i386/server/
    ```

3.  Create the `               BAPISender              ` proxy service
    with the following configuration:

    ``` xml
            <proxy xmlns="http://ws.apache.org/ns/synapse" 
                   name="BAPISender" 
                   transports="https,http" 
                   startOnLoad="true" 
                   trace="disable">
                <target>
                    <inSequence>
                        <send>
                            <endpoint name="sap_bapi_endpoint">
                                <address uri="bapi:/SAPSYS"/>
                            </endpoint>
                        </send>
                    </inSequence>
                    <outSequence>
                        <log level="full"/>
                        <send/>
                    </outSequence>
                </target>
            </proxy>
    ```

        !!! info
    
        -   If you set the property shown below (use the **Property**
            mediator before the **Send** mediator in the above sequence),
            any business-level error messages that are sent back from the
            SAP endpoint will be successfully passed through the out flow
            sequence. Without this property, the business-level errors from
            SAP can get detected as faulty messages and passed to the Fault
            sequence.
    
        ``` java
                <property name="sap.escape.error.handling" scope="axis2" value="true"/>
        ```
        
            -   The SAP endpoint client properties file
                `                SAPSYS.dest               ` should be in the
                `                <EI_HOME>/conf/sap               ` folder .
            -   Additional axis2-level sender parameters that can be defined in
                the axis2 core are listed in [SAP Transport Sender
                Parameters](#SAPIntegration-SenderParameters) .
        

##### Receiving BAPIs

Follow the instructions below to configure the ESB Profile as a BAPI
server using the SAP adapter.

1.  Uncomment the following line in
    `               <EI_HOME>/conf/axis2/axis2.xml              ` file
    to enable the BAPI transport listener in axis2 core.

    ``` xml
        <transportReceiver name="bapi" class="org.wso2.carbon.transports.sap.SAPTransportListener"/>
    ```

2.  Start the ESB Profile of WSO2 EI using the
    `               -Djava.library.path              ` switch to specify
    the location of your SAP jco library.  
    For example, you can execute the following command to start the ESB
    Profile:

    ``` java
            ./integrator.sh -Djava.library.path=/usr/lib/jvm/jre1.7.0/lib/i386/server/
    ```

3.  Create the `               BAPIReceiver              ` proxy service
    with the following configuration:

    ``` xml
            <proxy xmlns=http://ws.apache.org/ns/synapse 
                   name="BAPIReceiver"
                   transports="bapi" 
                   statistics="enable" 
                   trace="enable" 
                   startOnLoad="true">
               <target>
                 <inSequence>
                     <log level="full"/>
                     <drop/>
                </inSequence>
                <outSequence>
                    <log level="full"/>
                    <send/>
                </outSequence>
              </target>
              <parameter name="transport.sap.enableTIDHandler">enabled</parameter>
              <parameter name="transport.sap.serverName">SAPSYS</parameter>
              <description/>
            </proxy>
    ```

        !!! info
    
        -   The SAP endpoint server properties file
            `                SAPSYS.server               ` should be in the
            `                <EI_HOME>/conf/sap               ` folder .
        -   Additional proxy level listener parameters that can be defined
            in the proxy configuration are listed in [Proxy Service Listener
            Parameters](#SAPIntegration-ListenerParameters) .
    

### Additional Configuration Parameter

This section describes additional parameters that can be used when
configuring WSO2 SAP adapter.

##### SAP Transport Sender Parameters

Following are descriptions of the SAP client properties that can be
defined in the message context with axis2-client scope when using the
ESB Profile of WSO2 EI as a SAP client to send messages. These
properties can be added in
`         <EI_HOME>/conf/axis2/axis-client.        ` xml file:

<table>
<thead>
<tr class="header">
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong><code>              transport.sap.xmlMapper             </code></strong></td>
<td>The key of custom IDOC XML mapper to use. This key should be defined in the <code>             transport.sap.            </code> <code>             customXMLMappers            </code> parameter. If no key is specified the default IDoc XML mapper will be used.</td>
</tr>
<tr class="even">
<td><strong><code>              transport.sap.              xmlParserOptions             </code></strong></td>
<td><p>The options for the default IDoc XML parser to be used in the default IDoc XML mapper. Multiple options can be combined using the bitwise OR "|" operator. The possible parser options are as follows:</p>
<p>PARSE_ACCEPT_ONLY_XMLVERSION_10 3328<br />
PARSE_ACCEPT_ONLY_XMLVERSION_11 2816<br />
PARSE_ACCEPT_ONLY_XMLVERSIONS_10_TO_11 2304<br />
PARSE_IGNORE_INVALID_CHAR_ERRORS 4<br />
PARSE_IGNORE_UNKNOWN_FIELDS 2<br />
PARSE_REFUSE_UNKNOWN_XMLVERSION 256<br />
PARSE_REFUSE_XMLVERSION_10 512<br />
PARSE_REFUSE_XMLVERSION_11 1024<br />
PARSE_WITH_FIELD_VALUE_CHECKING 1<br />
PARSE_WITH_IGNORE_UNKNOWN_FIELDS 2<br />
PARSE_WITHOUT_FIELD_DATATYPE_CHECKING 8</p></td>
</tr>
</tbody>
</table>

Following is an Axis2 transport sender property that can be defined in
`         <EI_HOME>/conf/axis2/axis2.xml        ` file:

-   **`           transport.sap.customXMLMappers          `** : The
    key/value list of custom mappers, where the values are fully
    qualified class names for custom mappers that are implementing
    `          org.wso2.carbon.transports.sap.idoc.IDocXMLMapper         `
    .

##### Proxy Service Listener Parameters

Following are descriptions of the proxy level listener parameters that
can be defined in a proxy configuration when using the ESB Profile of
WSO2 EI as a SAP server:

| Parameter                                                                                            | Description                                                                                                                                                                                                                                                                       |
|------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`              transport.sap.serverName             `**                                            | The name of the server containing the JCO server configuration.                                                                                                                                                                                                                   |
| **`              transport.sap.              enableErrorListener             `**                     | Set this to enable the default error listener. If this is used together with the `             transport.sap.            ` `             customErrorListener            ` parameter, the custom error listener will be used.                                                      |
| **`              transport.sap.              enableTIDHandler             `**                        | Set this to enable the transaction handler to handle transactions that are received from a SAP system. Transactional applications must provide a custom implementation using the `             transport.sap.            ` `             customTIDHandler            ` parameter. |
| **`              transport.sap.              customTIDHandler             `**                        | The fully qualified class name for the custom TID handler implementing `             JCoServerTIDHandler            ` .                                                                                                                                                           |
| **`              transport.sap.connections             `**                                           | The number of registered connections managed by the server instance. The default value is `             1            ` and the maximum value is `             100            ` .                                                                                                  |
| **`              transport.sap.              customErrorListener             `**                     | The fully qualified class name for the custom error listener implementing `             JCoServerErrorListener            ` .                                                                                                                                                     |
| **`              transport.sap.             ` `              customExceptionListener             `** | The fully qualified class name for the custom exception listener implementing `             JCoServerExceptionListener            ` .                                                                                                                                             |

  

------------------------------------------------------------------------

### Troubleshooting

Given below are general troubleshooting guidelines.

-   ##### How to handle the **Server unknown** error

    An example of this error message is as follows:

    ``` java
        [2010-10-25 19:53:00,405] ERROR - DefaultErrorListener Exception occured on :
        JCOSERVER01 and connection : null
        com.sap.conn.jco.JCoException: (129) JCO_ERROR_SERVER_STARTUP: Server startup
        failed at Mon Oct 25 19:53:00 IST 2010.
        This is caused by either a) erroneous server settings, b) the backend system has been shutdown,
        c) network problems. Will try next startup in 1 seconds.
        Could not start server: Connect to SAP gateway failed
        Connect parameters: TPNAME=JCOSERVER01 GWHOST=cynthia GWSERV=sapgw00
        ERROR service 'sapgw00' unknown
        TIME Mon Oct 25 19:53:00 2010
        RELEASE 720
        COMPONENT NI (network interface)
        VERSION 40
        RC -3
        MODULE nixxsl.cpp
        LINE 184
        DETAIL NiSrvLGetServNo: service name cached as unknown
        COUNTER 2
        at com.sap.conn.jco.rt.DefaultServer.openConnection(DefaultServer.java:1168)
        at com.sap.conn.jco.rt.DefaultServer.openConnections(DefaultServer.java:1057)
        at com.sap.conn.jco.rt.DefaultServer.adjustConnectionCount(DefaultServer.java:1004)
        at
        com.sap.conn.jco.rt.DefaultServerManager$DispatcherWorker.run(DefaultServerManager.java:
        299)
        at java.lang.Thread.run(Thread.java:619)
        Caused by: com.sap.conn.jco.JCoException: (129) JCO_ERROR_SERVER_STARTUP: Could
        not start server: Connect to SAP gateway failed
        Connect parameters: TPNAME=JCOSERVER01 GWHOST=cynthia GWSERV=sapgw00
        ERROR service 'sapgw00' unknown
        TIME Mon Oct 25 19:53:00 2010
        RELEASE 720
        COMPONENT NI (network interface)
        VERSION 40
        RC -3
        MODULE nixxsl.cpp
        LINE 184
        DETAIL NiSrvLGetServNo: service name cached as unknown
        COUNTER 2
        at
        com.sap.conn.jco.rt.MiddlewareJavaRfc$JavaRfcServer.accept(MiddlewareJavaRfc.java:2135)
        at com.sap.conn.jco.rt.ServerConnection.accept(ServerConnection.java:380)
        at com.sap.conn.jco.rt.DefaultServer.openConnection(DefaultServer.java:1149)
        © 2012 WSO2
        .. 4 more
        Caused by: RfcException: [null]
        message: Connect to SAP gateway failed
        Connect parameters: TPNAME=JCOSERVER01 GWHOST=cynthia GWSERV=sapgw00
        ERROR service 'sapgw00' unknown
        TIME Mon Oct 25 19:53:00 2010
        RELEASE 720
        COMPONENT NI (network interface)
        VERSION 40
        RC -3
        MODULE nixxsl.cpp
        LINE 184
        DETAIL NiSrvLGetServNo: service name cached as unknown
        COUNTER 2
        Return code: RFC_FAILURE(1)
        error group: 102
        key: RFC_ERROR_COMMUNICATION
        at com.sap.conn.rfc.engine.RfcIoControl.error_end(RfcIoControl.java:255)
        at com.sap.conn.rfc.engine.RfcIoControl.ab_rfcaccept(RfcIoControl.java:43)
        at com.sap.conn.rfc.api.RfcApi.RfcAccept(RfcApi.java:41)
        at
        com.sap.conn.jco.rt.MiddlewareJavaRfc$JavaRfcServer.accept(MiddlewareJavaRfc.java:2121)
        ... 6 more
    ```

    The solution to overcome this is to add your SAP server names to the
    `           /etc/services          ` file with the relevant ports.
    For example, the following lines can be added if we consider the
    example error given above.

    ``` java
            sapgw00 3300/tcp
            sapgw01 3301/tcp
    ```

-   **How to handle connection to message server host failed error**
    **  
    ** An example of this error message is as follows:

    ``` xml
            Connection parameters: TYPE=B DEST=SAPSYS01 MSHOST=SAPSYS01 MSSERV=3600 R3NAME=ERD GROUP=PUBLIC PCS=1
        
            ERROR Group PUBLIC not found
        
            TIME Fri Jan 24 15:48:53 2014
    ```

    This indicates that that the username (i.e. wso2-user) used in the
    above configuration is not assigned to the 'public' user group.  
    The solution to overcome this is to add wso2-user to the user group
    named public in your SAP system. If such a group does not exist, a
    user group named 'public' needs be created and the above user needs
    to be added to that group.
