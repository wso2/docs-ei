# MailTo Transport Examples

## Globally setting the email sender

Once the MailTo transport sender is enabled, you can configure email
messaging within your mediation flow by applying the MailTo transport
properties.

### Synapse configuration

Enter a valid email address prefixed with the transport sender name (which is specified in the deployment.toml file). For example, if the transport sender is 'mailto', the endpoint URL should be as follows: `mailto:targetemail@mail.com`

```xml
<proxy name="EmailSender" startOnLoad="true" transports="https http" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
            <log/>
            <send>
                <endpoint>
                    <address uri="mailto:targetemail@mail.com"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </target>
</proxy>
```
### Build and run

1.  Deploy the proxy service in you WSO2 Micro Integrator.
2.  Invoke the proxy service by sending a request. For example use SOAP UI.
3.  Check the inbox of your email account, which is configured as the
    target endpoint. You will receive an email from the email sender
    that is configured globally in the deployment.toml file.

## Dynamically setting the email sender

### Synapse configuration

Enter a valid email address prefixed with the transport sender name (specified in the axis2.xml file). For example, if the transport sender is 'mailto', the endpoint URL should be as follows: `mailto:targetemail@mail.com`.

!!! Note
    -   You need to update the property values with actual values of the mail sender account.
    -   In some email service providers, the value for the 'mail.smtp.user' property is the same as the email address of the account.

!!! Tip
    For testing purposes, be sure to enable access from less secure apps to your email account. See the documentation from your email service provider for instructions.

Now, let's set the email sender details by adding **Property** mediators to the mediation sequence. If these details are not provided in the proxy service, the system uses the email [sender configurations in the deployment.toml file](#sending-emails-using-global-sender) explained above. Shown below is the new mediation sequence:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="EmailSender" startOnLoad="true" transports="https http" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
            <log/>
            <property name="From" scope="transport" type="STRING" value="frommail@mail.com"/>
            <property name="mail.smtp.user" scope="transport" type="STRING" value="userID"/>
            <property name="mail.smtp.password" scope="transport" type="STRING" value="xxxxxx"/>
            <send>
                <endpoint>
                    <address uri="mailto:targetemail@mail.com"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </target>
</proxy>
```
### Build and run

3.  Deploy the proxy service in you Micro Integrator server.
4.  Invoke the proxy service by sending a request.
5.  Check your inbox. You will receive an email from the email sender
    that you configured for the proxy service.

## Receiving emails

Follow the steps given below to configure your mediation sequence to receive emails, and then process the contents within the sequence. The MailTo transport receiver should be configured at service level and each service configuration should explicitly state the mail transport receiver configuration. This is required to enable different services to receive mails over different mail accounts and configurations.

!!! Info
    You need to provide correct parameters for a valid mail account at the service level.

### Synapse configuration

In this sample, we used the `transport.mail.ContentType` property to make sure that the transport parses the request message as POX. If you remove this property, you may still be able to send requests using a standard mail client if instead of writing the XML in the body of the message, you add it as an attachment. In that case, you should use XML as a suffix for the attachment and format the request as a SOAP 1.1 message. Indeed, for a file with suffix XML the mail client will most likely use text/XML as the content type, exactly as required for SOAP 1.1. Sending a POX message using this approach will be a lot trickier, because most standard mail clients do not allow the user to explicitly set the content type.

```xml
<proxy name="StockQuoteProxy" transports="mailto">
    <parameter name="transport.mail.Address">synapse.demo.1@gmail.com</parameter>
    <parameter name="transport.mail.Protocol">pop3</parameter>
    <parameter name="transport.PollInterval">5</parameter>
    <parameter name="mail.pop3.host">pop.gmail.com</parameter>
    <parameter name="mail.pop3.port">995</parameter>
    <parameter name="mail.pop3.user">synapse.demo.1</parameter>
    <parameter name="mail.pop3.password">mailpassword1</parameter>
    <parameter name="mail.pop3.socketFactory.class">javax.net.ssl.SSLSocketFactory</parameter>
    <parameter name="mail.pop3.socketFactory.fallback">false</parameter>
    <parameter name="mail.pop3.socketFactory.port">995</parameter>
    <parameter name="transport.mail.ContentType">application/xml</parameter>
    <target>
        <inSequence>
            <property name="senderAddress" expression="get-property('transport', 'From')"/>
            <log level="full">
                <property name="Sender Address" expression="get-property('senderAddress')"/>
            </log>
            <send>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <property name="Subject" value="Custom Subject for Response" scope="transport"/>
            <header name="To" expression="fn:concat('mailto:', get-property('senderAddress'))"/>
            <log level="full">
                <property name="message" value="Response message"/>
                <property name="Sender Address" expression="get-property('senderAddress')"/>
            </log>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
</proxy>
```

Once the MailTo transport receiver is enabled, you can configure email messaging within your mediation flow by applying the MailTo transport properties. The MailTo transport listener implementation can be configured by setting the parameters as described in the JavaMail API documentation. For IMAP related properties, see [IMAP Package Summary](https://javaee.github.io/javamail/docs/api/com/sun/mail/imap/package-summary.html). For POP3 properties, see [POP3 Package Summary](https://javaee.github.io/javamail/docs/api/com/sun/mail/pop3/package-summary.html). The MailTo transport listener also supports the following transport parameters in addition to the parameters described in the JavaMail API documentation.

### Build and run

-   Send a plain/text e-mail (Make sure you switch to **Plain text**
    **mode** when you are composing the email) with the following body
    and any custom Subject from your mail account to the mail address
    `          synapse.demo.1@gmail.com         ` . 

    ```xml 
    <m0:getQuote xmlns:m0="http://services.samples">
        <m0:request>
            <m0:symbol>IBM</m0:symbol>
        </m0:request>
    </m0:getQuote>
    ```

-   After a few seconds (for example 30 seconds), you should receive a
    POX response in your e-mail account with the stock quote reply.