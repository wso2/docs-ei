# Tuning the VFS Transport

The Virtual File System (VFS) transport is used by WSO2 Enterprise
Integrator(WSO2 EI) to process files in a specified source directory. It
supports numerous file systems such as ftp, ftps, sftp, local and samba.

When you work with the VFS transport, you might have a scenario where
you need to send large files to a destination. If you use the normal VFS
configuration, the memory consumption will be very high since WSO2 EI
builds the file inside it. To overcome this issue, WSO2 EI provides VFS
file streaming support. With VFS file streaming, only the stream is
passed and therefore memory consumption is less.

> **Tip** : When you transfer a file to a remote FTP location via VFS, the
ESB tries to detect the FTP location by navigating from the root folder
first. If the ESB does not have **at least list permission** to the root
(/), the file transfer fails.


To use the streaming mode with the VFS transport, follow the steps
below:

1.  In `           <EI_HOME>/conf/axis2/axis2.xml          ` , in the
    `           messageBuilders          ` section, add the binary
    message builder as follows:

    ```
    <messageBuilder contentType="application/binary" class="org.apache.axis2.format.BinaryBuilder"/>
    ```

    and in the `messageFormatters` section, add the
    binary message formatter as follows:

    ```
    <messageFormatter contentType="application/binary"class="org.apache.axis2.format.BinaryFormatter">
    ```

2.  In the proxy service where you use the VFS transport, add the
    following parameter to enable streaming:

    ```java
    <parameter name="transport.vfs.Streaming">true</parameter>
    ```

3.  In the same proxy service, before the Send mediator, add the
    following property:

    > You also need to add the following property if you want to use the
        VFS transport to transfer files from VFS to VFS.


    ```
    <property name="ClientApiNonBlocking" value="true" scope="axis2" action="remove"/>
    ```

    For more information, see Example 3 of the [Send
    Mediator](https://docs.wso2.com/display/EI650/Send+Mediator#SendMediator-blocking).

Following is a sample configuration that uses the VFS transport to
handle large files:

``` xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="StockQuoteProxy" transports="vfs">
            <parameter name="transport.vfs.FileURI">smb://host/test/in</parameter>        
            <parameter name="transport.vfs.ContentType">text/xml</parameter>
            <parameter name="transport.vfs.FileNamePattern">.*\.xml</parameter>
            <parameter name="transport.PollInterval">15</parameter>
            <parameter name="transport.vfs.Streaming">true</parameter>
            <parameter name="transport.vfs.MoveAfterProcess">smb://host/test/original</parameter>            
            <parameter name="transport.vfs.MoveAfterFailure">smb://host/test/original</parameter>          
            <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
            <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
            <target>
                 <inSequence>
                    <property name="transport.vfs.ReplyFileName"
                              expression="fn:concat(fn:substring-after(get-property('MessageID'), 'urn:uuid:'), '.xml')" scope="transport"/>
                    <property action="set" name="OUT_ONLY" value="true"/>
                    <property name="ClientApiNonBlocking" value="true" scope="axis2" action="remove"/>
                    <send>
                        <endpoint>
                            <address uri="vfs:smb://host/test/out"/>
                        </endpoint>
                    </send>
                </inSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
    </definitions>
```
