# Sample 256: Proxy Services with the MailTo Transport

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .

TEST  

**Objective** : Using the [MailTo
transport](https://docs.wso2.com/display/EI650/MailTo+Transport) with
Proxy Services .

###### **Prerequisites**

-   You will need access to an e-mail account.
-   Start the Axis2 server and deploy the
    `          SimpleStockQuoteService         ` if not already done.
-   Enable the mail transport listener in the ESB
    `          axis2.xml         ` . Simply uncomment the relevant
    transport receiver entry in the file.
-   Enable mail transport sender in the ESB
    `          axis2.xml         ` . See [MailTo
    transport](https://docs.wso2.com/display/EI650/MailTo+Transport) for
    details.
-   Send a plain/text e-mail (Make sure you switch to **Plain text**
    **mode** when you are composing the email) with the following body
    and any custom Subject from your mail account to the mail address
    `          synapse.demo.1@gmail.com         ` .  
    ``` html/xml
    <m0:getQuote xmlns:m0="http://services.samples">
        <m0:request>
            <m0:symbol>IBM</m0:symbol>
        </m0:request>
    </m0:getQuote>
    ```

-   After a few seconds (for example 30 seconds), you should receive a
    POX response in your e-mail account with the stock quote reply.

``` html/xml
<!-- Using the mail transport -->
<definitions xmlns="http://ws.apache.org/ns/synapse">
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
</definitions>
```

!!! info

Note

In this sample, we used the
`         transport.mail.ContentType        ` property to make sure that
the transport parses the request message as POX. If you remove this
property, you may still be able to send requests using a standard mail
client if instead of writing the XML in the body of the message, you add
it as an attachment. In that case, you should use XML as a suffix for
the attachment and format the request as a SOAP 1.1 message. Indeed, for
a file with suffix XML the mail client will most likely use text/XML as
the content type, exactly as required for SOAP 1.1. Sending a POX
message using this approach will be a lot trickier, because most
standard mail clients do not allow the user to explicitly set the
content type.

TEST  
