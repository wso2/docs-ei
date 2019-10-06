# Switching from FTP Listener to Mail Sender

Switching from FTP transport listener to mail transport sender.

## Synapse configuration

```xml
<proxy name="StockQuoteProxy" transports="vfs">
    <parameter name="transport.vfs.FileURI">vfs:ftp://guest:guest@localhost/test?vfs.passive=true</parameter> <!--CHANGE-->
    <parameter name="transport.vfs.ContentType">text/xml</parameter>
    <parameter name="transport.vfs.FileNamePattern">.*\.xml</parameter>
    <parameter name="transport.PollInterval">15</parameter>
    <target>
        <inSequence>
            <header name="Action" value="urn:getQuote"/>
        </inSequence>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <outSequence>
            <property action="set" name="OUT_ONLY" value="true"/>
            <send>
                <endpoint>
                    <address uri="mailto:user@host"/>
                </endpoint>
            </send>
        </outSequence>
    </target>
    <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
</proxy>
```

VFS transport listener will pick the file from the directory in the FTP server and send it to the Axis2 service. The file in the FTP directory will be deleted. The response will be sent to the given e-mail address.

## Build and run

1. Open WSO2 Integration Studio.
2. Create and ESB config project.
3. Create the proxy service configuration given above.

Start the ActiveMQ broker.

Deploy the integration artifacts in the Micro Integrator and start the server.

Execute the following command:
