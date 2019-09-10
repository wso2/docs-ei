# Inbound Endpoint File Protocol Sample (VFS)

## Introduction

This sample demonstrates how to use the file system as an input medium
via the inbound file listener.

## Prerequisites

-   Create 3 new directories named `          in         ` ,
    `          out         ` and `          original         ` in a test
    directory (e.g., /home/user/test) in the local file system.
    Then, open the
    `          <ESB_HOME>/repository/samples/synapse_sample_900.xml         `
    file in a text editor and change the
    `          transport.vfs.FileURI         ` ,
    `          transport.vfs.MoveAfterProcess         ` ,
    `          transport.vfs.MoveAfterFailure         ` parameter values
    to the **in** , **out** ,and **original** directory locations
    respectively.

## Building the sample

The XML configuration for this sample is as follows:

``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse">
       <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
          <parameter name="cachableDuration">15000</parameter>
       </registry>
       <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager">
          <parameter name="cachableDuration">15000</parameter>
       </taskManager>
       <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="file_inbound" sequence="request" onError="fault" protocol="file" suspend="false">
          <parameters>
             <parameter name="interval">1000</parameter>
             <parameter name="sequential">true</parameter>
             <parameter name="transport.vfs.FileURI">file:///home/user/test/in</parameter>
             <!--CHANGE-->
             <parameter name="transport.vfs.ContentType">text/xml</parameter>
             <parameter name="transport.vfs.FileNamePattern">.*\.xml</parameter>
             <parameter name="transport.vfs.MoveAfterProcess">file:///home/user/test/out</parameter>
             <!--CHANGE-->
             <parameter name="transport.vfs.MoveAfterFailure">file:///home/user/test/original</parameter>
             <!--CHANGE-->
             <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
             <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
          </parameters>
       </inboundEndpoint>
       <sequence name="request" onError="fault">
          <call>
             <endpoint>
                <address format="soap12" uri="http://localhost:9000/services/SimpleStockQuoteService"/>
             </endpoint>
          </call>
          <drop/>
       </sequence>
    </definitions>
```

This configuration file `         synapse_sample_900.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start the ESB with the sample 900 configuration. For instructions on
    starting a sample ESB configuration, see [Starting the ESB with a
    sample
    configuration](https://docs.wso2.com/enterprise-service-bus/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Startingasample)
    .  
    The operation log keeps running until the server starts, which
    usually takes several seconds. Wait until the server has fully
    booted up and displays a message similar to " *WSO2 Carbon started
    in n seconds.* "

2.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/enterprise-service-bus/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

3.  Deploy the back-end service **SimpleStockQuoteService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/enterprise-service-bus/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

## Executing the sample

**To execute the sample client**

-   Copy the
    `           ESB_HOME/repository/samples/resources/vfs/test.xml          `
    file to the location specified in
    `           transport.vfs.FileURI          ` in the configuration
    (i.e., the **in** directory).  
      
    The `           test.xml          ` file contains a simple stock
    quote request and is as follows:

    ```
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

You will see that the inbound polling file listener picks the file from
`         in        ` directory and sends it to the Axis2 service, and
that the request XML file is moved to `         out        ` directory.
