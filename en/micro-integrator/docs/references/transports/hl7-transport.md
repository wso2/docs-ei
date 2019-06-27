# HL7 Transport

The HL7 transport allows you to handle [Health Level 7
International](http://www.hl7.org/about/index.cfm?ref=common) (HL7)
messages. The following sections describe how to install, enable, and
configure the HL7 transport:

!!! info

WSO2 Enterprise Integrator(WSO2 EI) uses the HAPI parser to provide HL7
support, which currently does not support HL7v3.


-   [Enabling the transport](#HL7Transport-Enablingthetransport)
-   [Configuring the transport](#HL7Transport-Configuringthetransport)
    -   [Conformance profile](#HL7Transport-Conformanceprofile)
    -   [Message pre-processing](#HL7Transport-Messagepre-processing)
    -   [Acknowledgement](#HL7Transport-Acknowledgement)
        -   [Configuring application
            acknowledgement](#HL7Transport-Configuringapplicationacknowledgement)
    -   [Validating messages](#HL7Transport-Validatingmessages)
    -   [Configuring the thread
        pool](#HL7Transport-Configuringthethreadpool)
    -   [Storing messages](#HL7Transport-Storingmessages)
-   [Creating an HL7 Proxy
    Service](#HL7Transport-createCreatinganHL7ProxyService)
    -   [Accept acknowledgement](#HL7Transport-Acceptacknowledgement)
    -   [Application
        acknowledgement](#HL7Transport-ApplicationacknowledgementApplicationacknowledgement)
-   [Exchanging HL7 messages with the File
    System](#HL7Transport-ExchangingHL7messageswiththeFileSystem)
    -   [Transferring messages from file system to file
        system](#HL7Transport-Transferringmessagesfromfilesystemtofilesystem)
    -   [Transferring messages between HL7 and the file
        system](#HL7Transport-TransferringmessagesbetweenHL7andthefilesystem)
    -   [Transferring messages between HL7 and
        FTP](#HL7Transport-TransferringmessagesbetweenHL7andFTP)

### Enabling the transport

You can [create an HL7 proxy service](#HL7Transport-create) using this
transport after enabling it. You enable the HL7 transport in the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file as follows:

``` java
    <transportReceiver name="hl7" class="org.wso2.carbon.business.messaging.hl7.transport.HL7TransportListener">
        <parameter name="port">9292</parameter>
    </transportReceiver>
    <transportSender name="hl7" class="org.wso2.carbon.business.messaging.hl7.transport.HL7TransportSender">
        <!--parameter name="non-blocking">true</parameter-->
    </transportSender>
    ...
    <messageFormatters>
      <messageFormatter contentType="application/edi-hl7" class="org.wso2.carbon.business.messaging.hl7.message.HL7MessageFormatter"/>
    ...
    </messageFormatters>
    ...
    <messageBuilders>
      <messageBuilder contentType="application/edi-hl7" class="org.wso2.carbon.business.messaging.hl7.message.HL7MessageBuilder"/>
    </messageBuilders>
```

### Configuring the transport

When [creating an HL7 proxy service](#HL7Transport-create) , you can
optionally configure the following behavior of the HL7 transport.

#### Conformance profile

Add the parameter "transport.hl7.ConformanceProfilePath" in the proxy
service to point to a URL where the HL7 conformance profile (XML file)
can be found.

#### Message pre-processing

Add the "transport.hl7.MessagePreprocessorClass" parameter in the proxy
service to point to an implementation class of the interface
"org.wso2.carbon.business.messaging.hl7.common.HL7MessagePreprocessor",
which is used to process raw HL7 messages before parsing them so that
potential errors in the messages can be rectified using the transport.

#### Acknowledgement

You can enable or disable automatic message acknowledgment. When
automatic message acknowledgment is enabled, an ACK is immediately sent
back to the client after receiving a message. When it is disabled, the
user is given control to send back an ACK/NACK message from an
integration sequence after any message validations or related tasks.

When using a transport such as HTTP, to create an ACK/NACK message from
an HL7 message in the flow, specify an axis2 scope message context
property "HL7\_GENERATE\_ACK" and set its value to true. This ensures
that an ACK/NACK message is created automatically when a message is sent
(using the HL7 formatter). By default, an ACK message is created. If a
NACK message is required instead, use the message context properties
"HL7\_RESULT\_MODE" and "HL7\_NACK\_MESSAGE" as described below.

In the proxy service, add the following parameters to enable or disable
auto-acknowledgement and validation:

``` html/xml
    <proxy>...
       <parameter name="transport.hl7.AutoAck">true|false</parameter> <!-- default is true -->
    </proxy> 
```

When ‘AutoAck’ is false, you can set the following properties inside an
integration sequence.

``` html/xml
    <property name="HL7_RESULT_MODE" value="ACK|NACK" scope="axis2" /> <!-- notice the properties should be in axis2 scope --> 
```

When the result mode is ‘NACK’, you can use the following property to
provide a custom description of the error message.

``` html/xml
    <property name="HL7_NACK_MESSAGE" value="<ERROR MESSAGE>" scope="axis2" />
```

You can use the property "HL7\_RAW\_MESSAGE" in the axis2 scope to
retrieve the original raw EDI format HL7 message in an in sequence. The
user doesn't have to convert from XML to EDI again, so this usage may be
particularly helpful inside a custom mediator.

To control the encoding type of incoming messages, set the Java system
property "ca.uhn.hl7v2.llp.charset".

##### Configuring application acknowledgement

In general, we don't wait for the back-end application's response before
sending an "accept-acknowledgement" message to the client. If you do
want to wait for the application's response before sending the message,
define the following property in the InSequence:  
`         <property name="HL7_APPLICATION_ACK" value="true" scope="axis2"/>        `

In this case, the request thread will wait until the back-end
application returns the response before sending the
"accept-acknowledgement" message to the client. You can configure how
long request threads wait for the application's response by configuring
the time-out in milliseconds at the transport level:

`         <transportReceiver name="hl7" class="org.wso2.carbon.business.messaging.hl7.transport.HL7TransportListener">        `
**`                    <parameter name="transport.hl7.TimeOut">1000</parameter>         `**
`                  </transportReceiver>        `

For more information on configuring the proxy service for application
acknowledgment, see [Application
acknowledgement](#HL7Transport-Applicationacknowledgement) in [Creating
an HL7 Proxy Service](#HL7Transport-create) .

#### Validating messages

By default, the HL7 transport validates messages before building their
XML representation. You configure validation with the following
parameter in the proxy service:

``` html/xml
    <proxy>...
       <parameter name="transport.hl7.ValidateMessage">true|false</parameter> <!-- default is true -->
    </proxy> 
```

When `         transport.hl7.ValidateMessage        ` is set to false,
you can set the following parameters to handle invalid messages:

-   `          transport.hl7.BuildInvalidMessages         ` : when set
    to `          true         ` , builds a SOAP envelope with the
    contents of the raw HL7 message inside the
    `          <rawMessage>         ` element.
-   `          transport.hl7.          PassThroughInvalidMessages         `
    : when `          BuildInvalidMessages         ` is set to
    `          true         ` , you use this parameter to specify
    whether to pass this message through (true) or to throw a fault
    (false).

The following diagram illustrates these flows.

![](https://lh6.googleusercontent.com/lg4_50q29gFZ9i9C_abk6kMed9yfKN_4yyJcT7tAlOoV5oUEm-DJ0BMa6hob_mmh_y5duotKKtjyrYOel3-4gogVTc5d4UjBVXzP8qyhMDoDhaMck7eyJp4cWA)

#### Configuring the thread pool

The HL7 transport uses a thread pool to manage connections. A larger
thread pool provides greater performance, because the transport can
process more messages simultaneously, but it also uses more memory. You
can add the following properties to the proxy service to configure the
thread pool to suit your environment:

-   `          transport.hl7.corePoolSize         ` : the core number of
    threads in the pool. Default is 10.
-   `          transport.hl7.maxPoolSize         ` : the maximum number
    of threads that can be in the pool. Default is 20.
-   `          transport.hl7.idleThreadKeepAlive         ` : the time in
    milliseconds to keep idle threads alive before releasing them.
    Default is 10000 (10 seconds).

#### Storing messages

You can use the HL7 message store to automatically store HL7 messages,
allowing you to audit and replay messages as needed. The HL7 store is a
custom message store implementation on top of Open JPA. To use the
message store, you take the following steps:

1.  Create an empty database in your RDBMS, and then create a message
    store configuration that points to that database.
2.  Create a sequence that points to that message store configuration.

For example:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
    
       <proxy name="HL7Store" startOnLoad="true" trace="disable" transports=”hl7”>
          <description/>
          <target>
             <inSequence>
                <property name="HL7_RESULT_MODE" value="ACK" scope="axis2"/>
                <log level="full"/>
                <property name="messageType" value="application/edi-hl7" scope="axis2"/>
                <clone>
                   <target sequence="StoreSequence"/>
                   <target sequence="SendSequence"/>
                </clone>
             </inSequence>
          </target>
          <parameter name="transport.hl7.AutoAck">false</parameter>
          <parameter name="transport.hl7.Port">55557</parameter>
          <parameter name="transport.hl7.ValidateMessage">false</parameter>
       </proxy>
    
       <sequence name="StoreSequence">
          <property name="OUT_ONLY" value="true"/>
          <store messageStore="HL7StoreJPA"/>
       </sequence>
    
       <sequence name="SendSequence">
          <in>
             <send>
                <endpoint>
                   <address uri="hl7://localhost:9988"/>
                </endpoint>
             </send>
          </in>
          <out>
             <log level="full"/>
             <drop/>
          </out>
       </sequence>
    
       <messageStore class="org.wso2.carbon.business.messaging.hl7.store.jpa.JPAStore"
                     name="HL7StoreJPA">
          <parameter name="openjpa.ConnectionDriverName">org.apache.commons.dbcp.BasicDataSource</parameter>
          <parameter name="openjpa.ConnectionProperties">DriverClassName=com.mysql.jdbc.Driver, Url=jdbc:mysql://localhost/hl7storejpa,  MaxActive=100,  MaxWait=10000,  TestOnBorrow=true,  Username=root,  Password=root</parameter>
          <parameter name="openjpa.jdbc.DBDictionary">blobTypeName=LONGBLOB</parameter>
       </messageStore>
    
    </definitions>
```

In this configuration, when the HL7 proxy service runs, an HL7 service
will start listening on the port defined in the
`         transport.hl7.Port        ` service parameter. When an HL7
message arrives, the proxy will send an ACK back to the client as
specified in the HL7\_RESULT\_MODE property. The Clone mediator is used
inside the proxy to replicate the message into the Send and Store
sequences, where the message is sent to the specified endpoint and is
also stored in the message store HL7StoreJPA.

The HL7StoreJPA message store is a custom message store implemented in
`         org.wso2.carbon.business.messaging.hl7.store.jpa.JPAStore        `
. It takes [OpenJPA
properties](http://openjpa.apache.org/builds/1.0.1/apache-openjpa-1.0.1/docs/manual/ref_guide_conf_openjpa.html)
as parameters. In this example, the
`         openjpa.ConnectionProperties        ` and
`         openjpa.ConnectionDriverName        ` properties are used to
create an Apache DBCP pooled connection set to a MySQL database. You
will need to create the database specified in the connection properties
and provide the database authentication details matching your datab
ase. You may also need to place the JDBC drivers for your database into
`         <EI_HOME>/lib        ` .

You can view the messages in this message store using the HL7 Console
UI. You can search for messages on the unique message UUID or HL7
specific Control ID. The search field supports the wildcard ‘%’ to allow
LIKE queries. The table can also be filtered to search for content
within messages.

Selected messages can be edited and injected into a proxy service.
Reinjecting a message to the same service will result in a new message
being stored with a different message UUID.

### Creating an HL7 Proxy Service

You can create a proxy service that uses the HL7 transport, to connect
to an HL7 server. This proxy service will receive HL7-client connections
and send them to the HL7 server. It can also receive XML messages over
HTTP/HTTPS and transform them into HL7 before sending them to the
server, and it will transform the HL7 responses back into XML.

!!! info

If you want to try the example configurations on this page, you must
have an HL7 client and HL7 back-end application set up and running. To
see a sample that illustrates how to create an HL7 client and back-end
application, see:  
<https://github.com/wso2/carbon-mediation/tree/v4.6.6/components/business-adaptors/hl7/org.wso2.carbon.business.messaging.hl7.samples/src/main/java/org/wso2/carbon/business/messaging/hl7/samples>


**To create the HL7 proxy service:**

1.  In the [Management
    Console](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-run)
    , create a custom proxy service (see [Adding a Proxy
    Service](https://docs.wso2.com/display/EI650/Creating+a+Proxy+Service)
    ).
2.  In the transports list, specify `           https          ` ,
    `           http          ` , and `           hl7          ` .

3.  Create an Address Endpoint with the URL of the HL7 server host and
    port, such as:
    `                     hl7://localhost:9988                   `
4.  Add the following parameter to the proxy service (required to enable
    the transport):  
    `           <parameter name="transport.hl7.Port">9292</parameter>          `

For example:

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="hl7testproxy" transports="https,http,hl7" statistics="disable" trace="disable" startOnLoad="true">
           <target>
              <inSequence>
                 <log level="full" />
              </inSequence>
              <outSequence>
                 <log level="full" />
                 <send />
              </outSequence>
              <endpoint name="endpoint_urn_uuid_9CB8D06C91A1E996796270828144799-1418795938">
                 <address uri="hl7://localhost:9988" />
              </endpoint>
           </target>
           <parameter name="transport.hl7.Port">9292</parameter>
        </proxy>
```

For information on additional configuration you can set on the HL7
transport, see [HL7 Transport](_HL7_Transport_) .

#### Accept acknowledgement

If user doesn't want to wait for the back-end service to process the
message and only needs acknowledgment from the system that the message
was received, you can configure the proxy service to send an ACK/NACK
message after the message is received. For example:

``` html/xml
    <proxy name="HL7Proxy" transports="hl7" startOnLoad="true" trace="disable">
        <description/>
        <target>
            <inSequence>
                <property name="HL7_RESULT_MODE" value="ACK" scope="axis2"/>
                <property name="HL7_GENERATE_ACK" value="true" scope="axis2"/>
                <send>
                   <endpoint name="endpoint_urn_uuid_9CB8D06C91A1E996796270828144799-1418795938">
                        <address uri="hl7://localhost:9988"/>
                    </endpoint>
                </send>
            </inSequence>
            <outSequence>
                <log level="custom">
                    <property name="OUT" value="***********out sequence proxy2***********"/>
                </log>
               <drop/>
            </outSequence>
         </target>
         <parameter name="transport.hl7.AutoAck">false</parameter>
         <parameter name="transport.hl7.ValidateMessage">false</parameter>
    </proxy>
```

#### Application acknowledgement

If you want to wait for the application's response before sending the
acknowledgment message (see [Configuring application
acknowledgement](#HL7Transport-Configuringapplicationacknowledgement) ),
you add the HL7\_APPLICATION\_ACK property to the inSequence and any
additional HL7 properties and transport parameters as needed. For
example:

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="HL7Proxy" transports="hl7" statistics="disable" trace="disable" startOnLoad="true"> 
      <target> 
          <inSequence> 
               <property name="HL7_APPLICATION_ACK" value="true" scope="axis2"/>  
                <send> 
                      <endpoint name="endpoint_urn_uuid_9CB8D06C91A1E996796270828144799-1418795938"> 
                              <address uri="hl7://localhost:9988"/> 
                       </endpoint> 
                 </send> 
          </inSequence> 
          <outSequence>  
               <property name="HL7_RESULT_MODE" value="NACK" scope="axis2"/> 
               <property name="HL7_NACK_MESSAGE" value="error msg" scope="axis2"/> 
               <send/> 
          </outSequence> 
      </target> 
      <parameter name="transport.hl7.AutoAck">false</parameter> 
      <parameter name="transport.hl7.ValidateMessage">true</parameter> 
      <description></description> 
    </proxy>
```

### Exchanging HL7 messages with the File System

The following samples demonstrate how to read and write HL7 messages to
and from the file system using the [HL7 Transport](_HL7_Transport_) and
[VFS Transport](_VFS_Transport_) :

-   [Enabling the transport](#HL7Transport-Enablingthetransport)
-   [Configuring the transport](#HL7Transport-Configuringthetransport)
    -   [Conformance profile](#HL7Transport-Conformanceprofile)
    -   [Message pre-processing](#HL7Transport-Messagepre-processing)
    -   [Acknowledgement](#HL7Transport-Acknowledgement)
        -   [Configuring application
            acknowledgement](#HL7Transport-Configuringapplicationacknowledgement)
    -   [Validating messages](#HL7Transport-Validatingmessages)
    -   [Configuring the thread
        pool](#HL7Transport-Configuringthethreadpool)
    -   [Storing messages](#HL7Transport-Storingmessages)
-   [Creating an HL7 Proxy
    Service](#HL7Transport-createCreatinganHL7ProxyService)
    -   [Accept acknowledgement](#HL7Transport-Acceptacknowledgement)
    -   [Application
        acknowledgement](#HL7Transport-ApplicationacknowledgementApplicationacknowledgement)
-   [Exchanging HL7 messages with the File
    System](#HL7Transport-ExchangingHL7messageswiththeFileSystem)
    -   [Transferring messages from file system to file
        system](#HL7Transport-Transferringmessagesfromfilesystemtofilesystem)
    -   [Transferring messages between HL7 and the file
        system](#HL7Transport-TransferringmessagesbetweenHL7andthefilesystem)
    -   [Transferring messages between HL7 and
        FTP](#HL7Transport-TransferringmessagesbetweenHL7andFTP)

For more information on creating an HL7 proxy service, see [Creating an
HL7 Proxy
Service](https://docs.wso2.com/display/EI6xx/HL7+Transport#HL7Transport-createCreatinganHL7ProxyService)
.

#### Transferring messages from file system to file system

WSO2 Enterprise Integrator(WSO2 EI) allows messages to be transferred
between HL7 and the file system using the HL7 and VFS transports. To
begin, ensure that you have the VFS and HL7 transports enabled by
uncommenting the relevant transportReceiver and transportSender elements
inside the `         <EI_HOME>/conf/axis2/axis2.xml        ` file. You
must also uncomment the relevant builder/formatter pair to enable WSO2
EI to work with the HL7 message format. For more information, see [HL7
Transport](_HL7_Transport_) and [VFS Transport](_VFS_Transport_) .

Once you have enabled the transports and started the WSO2 EI server, use
the following proxy service configuration to run the sample.

!!! info

This sample uses the UNIX temporary directory /tmp/ in several VFS
parameters; be sure to change them to match a location in your file
system.


``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="FileSystemToFileSystem" transports="vfs">
      <target>
        <inSequence>
          <property name="OUT_ONLY" value="true" scope="default" type="STRING"/>
          <property name="transport.vfs.ReplyFileName" expression="get-property('transport','FILE_NAME')" scope="transport" type="STRING"/>
          <log level="full"/>
          <send>
            <endpoint>
              <address uri="vfs:file:///tmp/out"/>
            </endpoint>
          </send>
        </inSequence>
      </target>
      <parameter name="transport.PollInterval">5</parameter>
      <parameter name="transport.vfs.FileURI">/tmp/in</parameter>
      <parameter name="transport.vfs.FileNamePattern">.*\.hl7</parameter>
      <parameter name="transport.vfs.ContentType">application/edi-hl7;charset="iso-8859-15"</parameter>
      <parameter name="transport.hl7.ValidateMessage">false</parameter>
    </proxy>
```

Now, copy the following HL7 message into a text editor and save it as an
`         .hl7        ` file inside the directory you specified with the
`         transport.vfs.FileURI        ` parameter (
`         /tmp/in        ` in the above example).

    MSH|^~\&|Abc|Def|Ghi|JKL|20131231000000||ADT^A01|1234567|P|2.6|||NE|NE|CH|

The proxy service is configured to detect `         .hl7        ` files
in the `         transport.vfs.FileURI        ` directory. We have also
configured the VFS content type as the
`         application/edi-hl7        ` MIME type with an optional
charset encoding. When you save the `         .hl7        ` file to that
directory, the proxy service invokes the HL7 builders/formatters and
builds the HL7 message into its equivalent XML format. It then prints
the XML representation of the message on the management console and
forwards the message to the VFS endpoint ‘/tmp/out’.

For more information on configuring these transports and general
properties you can set, see the following topics:

-   [HL7 Transport](_HL7_Transport_)
-   [VFS Transport](_VFS_Transport_)
-   Properties Reference

#### Transferring messages between HL7 and the file system

Now, let's look at how we can send and receive files between an HL7
service and the file system. For this scenario, we will use the [HAPI
Test Panel](index) to send messages to WSO2 EI. As with the previous
example, ensure that you have the VFS and HL7 transports and their
builders/formatters enabled, and replace the temporary directories shown
in the sample with actual directories on your file system.

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse" 
         name="HL7ToFileSystem" 
         transports="hl7" 
         statistics="disable" 
         trace="disable" 
         startOnLoad="true"> 
     <target> 
        <inSequence> 
           <log level="full"/> 
           <property name="HL7_RESULT_MODE" value="ACK" scope="axis2"/> 
           <property name="OUT_ONLY" value="true"/> 
           <property name="transport.vfs.ReplyFileName" 
                     expression="fn:concat(get-property('SYSTEM_DATE', 'yyyyMMdd.HHmmssSSS'), '.xml')" 
                     scope="transport"/> 
           <send> 
              <endpoint> 
                 <address uri="vfs:file:///tmp/out"/> 
              </endpoint> 
           </send> 
        </inSequence> 
     </target> 
     <parameter name="transport.hl7.AutoAck">false</parameter> 
     <parameter name="transport.hl7.Port">55555</parameter> 
     <parameter name="transport.hl7.ValidateMessage">false</parameter> 
     <description/> 
    </proxy> 
```

To invoke this proxy, use the HAPI Test Panel to connect to the HL7
service at the specified port and send a test message. When this proxy
service runs, an HL7 service will start listening on the port defined in
the `         transport.hl7.Port        ` parameter. When the HL7
message arrives, the proxy will send an ACK back to the client as
specified in the HL7\_RESULT\_MODE property. The HL7 message is then
processed through WSO2 EI and sent to the VFS endpoint, which will save
the HL7 message in ‘/tmp/out’.

#### Transferring messages between HL7 and FTP

The following configuration is similar to the previous example, but it
illustrates how to process files between an HL7 endpoint and files
accessed through FTP. To run this sample, first set up an endpoint by
starting a new receiver connection in the HAPI Test Panel on port 9988.
You then configure this endpoint in the Send mediator as shown in the
following proxy service example. The proxy service will detect
`         .hl7        ` files in the
`         transport.vfs.FileURI        ` directory and send them to the
HL7 endpoint.

``` html/xml
    <proxy xmlns="http://ws.apache.org/ns/synapse"
           name="SFTPToHL7"
           transports="vfs"
           statistics="disable"
           trace="disable"
           startOnLoad="true">
       <target>
          <inSequence>
             <property name="OUT_ONLY" value="true" scope="default" type="STRING"/>
             <log level="full"/>
             <send>
                <endpoint>
                   <address uri="hl7://localhost:9988"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <drop/>
          </outSequence>
       </target>
       <parameter name="transport.vfs.ReconnectTimeout">2</parameter>
       <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
       <parameter name="transport.PollInterval">5</parameter>
       <parameter name="transport.hl7.AutoAck">false</parameter>
       <parameter name="transport.vfs.MoveAfterProcess">vfs:sftp://user:pass@localhost/vfs/out</parameter>
       <parameter name="transport.vfs.FileURI">vfs:sftp://user:pass@localhost/vfs/in</parameter>
       <parameter name="transport.vfs.MoveAfterFailure">vfs:sftp://user:pass@localhost/vfs/failed</parameter>
       <parameter name="transport.vfs.FileNamePattern">.*\.hl7</parameter>
       <parameter name="transport.vfs.ContentType">application/edi-hl7;charset="iso-8859-15"</parameter>
       <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
       <parameter name="transport.hl7.ValidateMessage">false</parameter>
       <description/>
    </proxy>  
```

Once you start the endpoint of the WSO2 EI server, if you place an HL7
message in the `         transport.vfs.FileURI        ` directory, you
will be able to see the message passed to the HL7 endpoint in the HAPI
Test Panel.
