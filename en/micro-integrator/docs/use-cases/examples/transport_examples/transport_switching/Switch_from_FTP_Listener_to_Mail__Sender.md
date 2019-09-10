# Switching from FTP Transport Listener to Mail Transport Sender

Switching from FTP transport listener to mail transport
sender.

###### **Prerequisites**

-   You will need access to an FTP server and an SMTP server to try this
    sample.
-   Start the Axis2 server and deploy the
    `          SimpleStockQuoteService         ` if not already done.
-   Enable mail transport sender in the ESB
    `          axis2.xml         ` .
-   Create a new test directory in the FTP server. Open
    ESB\_HOME/repository/samples/synapse\_sample\_255.xml and edit the
    following values. Change transport.vfs.FileURI parameter value point
    to the test directory at the FTP server. Change outSequence endpoint
    address URI email address to a working email address. Values you
    have to change are marked with " `          <!--CHANGE-->         `
    ".
-   Copy
    `          ESB_HOME/repository/samples/resources/vfs/test.xml         `
    to the ftp directory given in
    `          transport.vfs.FileURI         ` below.

```
<definitions xmlns="http://ws.apache.org/ns/synapse">
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
                        <address uri="mailto:user@host"/> <!--CHANGE-->
                    </endpoint>
                </send>
            </outSequence>
        </target>
        <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
</definitions>
```

VFS transport listener will pick the file from the directory in the FTP
server and send it to the Axis2 service. The file in the FTP directory
will be deleted. The response will be sent to the given e-mail address.

### Setting up Mail Transport Sender

To enable the mail transport sender for samples, you need to uncomment
the mail transport sender configuration in the
`         repository/conf/axis2/axis2.xml        ` . Uncomment the mail
transport sender sample configuration and make sure it points to a valid
SMTP configuration for any actual scenarios.

```
<transportSender name="mailto" class="org.apache.synapse.transport.mail.MailTransportSender">
    <parameter name="mail.smtp.host">smtp.gmail.com</parameter>
    <parameter name="mail.smtp.port">587</parameter>
    <parameter name="mail.smtp.starttls.enable">true</parameter>
    <parameter name="mail.smtp.auth">true</parameter>
    <parameter name="mail.smtp.user">synapse.demo.0</parameter>
    <parameter name="mail.smtp.password">mailpassword</parameter>
    <parameter name="mail.smtp.from">synapse.demo.0@gmail.com</parameter>
</transportSender>
```