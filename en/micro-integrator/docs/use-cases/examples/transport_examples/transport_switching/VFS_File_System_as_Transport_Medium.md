# Using the File System as Transport Medium (VFS)

### Introduction

The ESB can access the local file system using the [VFS
transport](https://docs.wso2.com/display/EI650/VFS+Transport) sender and
receiver. This sample demonstrates the VFS transport in action, using
the file system as a transport medium.

### Prerequisites

-   Create 3 new directories (folders) named **in** , **out** and
    **original** in a suitable location in a test directory (e.g.,
    /home/user/test) in the local file system. Then, open the
    `          repository/sample/synapse_sample_254.xml         ` file
    in a text editor and change the
    `          transport.vfs.FileURI         ` ,
    `          transport.vfs.MoveAfterProcess         ` ,
    `          transport.vfs.MoveAfterFailure         ` parameter values
    to the **in** , **original** and **original** directory locations
    respectively. You need to set both
    `          transport.vfs.MoveAfterProcess         ` and
    `          transport.vfs.MoveAfterFailure         ` parameter
    values to point to the **original** directory location.Change the
    endpoint in the `          <outSequence>         ` to point to the
    **out** directory location. Make sure that the prefix
    `          vfs:         ` in the endpoint URL is not removed or
    changed.
-   For a list of general prerequisites, see [Prerequisites to Start the
    ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
    .

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy name="StockQuoteProxy" transports="vfs">
        <parameter name="transport.vfs.FileURI">file:///home/user/test/in</parameter>                          <!--CHANGE-->
        <parameter name="transport.vfs.ContentType">text/xml</parameter>
        <parameter name="transport.vfs.FileNamePattern">.*\.xml</parameter>
        <parameter name="transport.PollInterval">15</parameter>
        <parameter name="transport.vfs.MoveAfterProcess">file:///home/user/test/original</parameter>           <!--CHANGE-->
        <parameter name="transport.vfs.MoveAfterFailure">file:///home/user/test/original</parameter>           <!--CHANGE-->
        <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
        <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
        <target>
            <endpoint>
                <address format="soap12" uri="http://localhost:9000/services/SimpleStockQuoteService"/>
            </endpoint>
            <outSequence>
                <property name="transport.vfs.ReplyFileName"
                          expression="fn:concat(fn:substring-after(get-property('MessageID'), 'urn:uuid:'), '.xml')"
                          scope="transport"/>
                <property action="set" name="OUT_ONLY" value="true"/>
                <send>
                    <endpoint>
                        <address uri="vfs:file:///home/user/test/out"/>    <!--CHANGE-->   
                    </endpoint>
                </send>
            </outSequence>
        </target>
        <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
</definitions>
```

This configuration file `         synapse_sample_254.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory and the values you have to change as specified in the
prerequisites section are marked with `         <!--CHANGE-->        ` .

**To build the sample**

1.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

### Executing the sample

The sample client used here is the **Stock Quote Client** , which can
operate in several modes. For further details on this sample client and
its operation modes, see [Stock Quote
Client](https://docs.wso2.com/display/EI650/Using+the+Sample+Clients#UsingtheSampleClients-StockQuoteClient)
.

**To execute the sample client**

Move the `         test.xml        ` file from the
`         <ESB_HOME>/repository/samples/resources/vfs        ` directory
to the location specified in `         transport.vfs.FileURI        ` in
the configuration (i.e., the **in** directory).

The `         test.xml        ` file contains a simple stock quote
request in XML/SOAP format and is as follows:

**test.xml**

``` xml
<?xml version='1.0' encoding='UTF-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing">
    <soapenv:Body>
        <m0:getQuote xmlns:m0="http://services.samples">
            <m0:request>
                <m0:symbol>IBM</m0:symbol>
            </m0:request>
        </m0:getQuote>
    </soapenv:Body>
</soapenv:Envelope>
```

### Analyzing the output

You will see that the VFS transport listener picks the file from the
**in** directory and sends it to the Axis2 service over HTTP. Then you
will see that the request XML file is moved to the **original**
directory and that the response from the Axis2 server is saved to the
**out** directory.
