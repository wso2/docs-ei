# MailTo Transport

When you use the ESB profile of WSO2 Enterprise Integrator (WSO2 EI) to
mediate messages, the mediation sequence can be configured to send
emails (over SMTP) or receive emails (Over POP3 or IMAP) by using the
MailTo transport protocol. The MailTo transport sender and receiver
should be enabled in the `         axis2.xml        ` file (stored in
the `         <EI_HOME>/conf/axis2/        ` directory) by uncommenting
the sender/receiver configurations. In addition to enabling the
transport, you can add more properties to this configuration to control
the behavior of the transport.

!!! info

This transport implementation is available as a module of the WS-Commons
Transports project. The JAR consisting of the MailTo transport
implementation is named `         axis2-transport-mail.jar        ` and
the following sender and receiver classes should be included in the WSO2
EI configuration to enable the MailTo transport:

-   `          org.apache.axis2.transport.mail.MailTransportSender         `
-   `          org.apache.axis2.transport.mail.MailTransportListener         `


-   [Sending emails from a mediation
    sequence](#MailToTransport-Sendingemailsfromamediationsequence)
    -   [Step 1: Enabling the transport
        sender](#MailToTransport-enableMailToStep1:Enablingthetransportsender)
    -   [Step 2: Configuring the mediation
        flow](#MailToTransport-Step2:Configuringthemediationflow)
        -   [To send emails from the global
            sender](#MailToTransport-Tosendemailsfromtheglobalsender)
        -   [To send emails from a service-specific
            sender](#MailToTransport-Tosendemailsfromaservice-specificsender)
-   [Receiving emails to a mediation
    sequence](#MailToTransport-Receivingemailstoamediationsequence)
    -   [Step 1: Enabling the transport
        receiver](#MailToTransport-Step1:Enablingthetransportreceiver)
    -   [Step 2: Configuring the mediation
        flow](#MailToTransport-Step2:Configuringthemediationflow.1)

### Sending emails from a mediation sequence

Follow the steps given below to configure your mediation sequences to
trigger emails.

#### Step 1: Enabling the transport sender

To enable the MailTo sender, uncomment the following in the axis2.xml
file (stored in the `         <EI_HOME>/conf/axis2/        ` directory):

``` html/xml
    <transportSender name="mailto" class="org.apache.axis2.transport.mail.MailTransportSender">
        <parameter name="mail.smtp.host">smtp.gmail.com</parameter>
        <parameter name="mail.smtp.port">587</parameter>
        <parameter name="mail.smtp.starttls.enable">true</parameter>
        <parameter name="mail.smtp.auth">true</parameter>
        <parameter name="mail.smtp.user">synapse.demo.0</parameter>
        <parameter name="mail.smtp.password">mailpassword</parameter>
        <parameter name="mail.smtp.from">synapse.demo.0@gmail.com</parameter>
    </transportSender>
```

Note that, in addition to enabling the transport, the following
parameters are used in the above configuration to set a default email
account as the mail sender. You can override this default mail sender by
specifying an email sender account within your mediation sequence.

<table style="width:100%;">
<colgroup>
<col style="width: 17%" />
<col style="width: 82%" />
</colgroup>
<tbody>
<tr class="odd">
<td><pre><code>mail.smtp.from</code></pre></td>
<td>The email address from which mails will be sent.</td>
</tr>
<tr class="even">
<td><pre><code>mail.smtp.user</code></pre></td>
<td>The user name of the email account (mail sender). Note that in some email service providers, the user name is the same as the email address specified for the 'From' parameter.</td>
</tr>
<tr class="odd">
<td><pre><code>mail.smtp.password</code></pre></td>
<td>The password of the email account (mail sender).</td>
</tr>
</tbody>
</table>

If you want to use multiple mail boxes to send emails, make a copy of
the default MailTo sender configuration in the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file and change the
transport sender name. For example, add `         mailtoWSO2        ` as
the name.

``` java
    <transportSender name="mailtoWSO2" class="org.apache.axis2.transport.mail.MailTransportSender">
            .........
```

You can use the `         mailtoWSO2        ` mail box by specifying the
following in a synapse configuration:

``` java
    <endpoint>
         <address uri="mailtoWSO2:user@host"/>
    </endpoint>
```

For a list of parameters supported by the MailTo transport sender, see
[SMTP Package
Summary](https://javaee.github.io/javamail/docs/api/com/sun/mail/smtp/package-summary.html)
. In addition to the parameters described there, the MailTo transport
sender supports the following parameters.

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 54%" />
<col style="width: 27%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Possible Values</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              transport.mail.SMTPBccAddresses             </code></p></td>
<td><p>If one or more e-mail addresses need to be specified as BCC addresses for outgoing mails, this parameter can be used.</p></td>
<td><p>A comma-separated list of e-mail addresses</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.mail.Format             </code></p></td>
<td><p>Format of the outgoing mail.</p></td>
<td><p><em>Text</em> , <em>Multipart</em></p></td>
</tr>
</tbody>
</table>

#### Step 2: Configuring the mediation flow

Once the MailTo transport sender is enabled, you can configure email
messaging within your mediation flow by applying the MailTo transport
properties.

##### To send emails from the global sender

1.  Use WSO2 EI Tooling to create a simple passthru proxy service with
    the following configurations:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Transport</td>
    <td>Select 'http' and 'https' as the transports receiving messages.</td>
    </tr>
    <tr class="even">
    <td>Endpoint URL</td>
    <td><div class="content-wrapper">
    <p>Enter a valid email address prefixed with the transport sender name (specified in the axis2.xml file). For example, if the transport sender is 'mailto', the endpoint URL should be as follows: 'mailto:targetemail@mail.com'.</p>
        !!! tip
        <p>For testing purposes, be sure to enable access from less secure apps to your email account. See the documentation from your email service provider for instructions.</p>

    </div></td>
    </tr>
    </tbody>
    </table>

2.  Deploy the proxy service in you WSO2 EI server.
3.  Invoke the proxy service by sending a request. For example use SOAP
    UI.
4.  Check the inbox of your email account, which is configured as the
    target endpoint. You will receive an email from the email sender
    that is configured globally in the axis2.xml file.

##### To send emails from a service-specific sender

1.  Use WSO2 EI Tooling to create a simple passthru proxy service with
    the following configurations:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Transport</td>
    <td>Select 'http' and 'https' as the transports receiving messages.</td>
    </tr>
    <tr class="even">
    <td>Endpoint URL</td>
    <td><div class="content-wrapper">
    <p>Enter a valid email address prefixed with the transport sender name (specified in the axis2.xml file). For example, if the transport sender is 'mailto', the endpoint URL should be as follows: 'mailto:targetemail@ <a href="http://mail.com">mail.com</a> '.</p>
        !!! tip
        <p>For testing purposes, be sure to enable access from less secure apps to your email account. See the documentation from your email service provider for instructions.</p>

    </div></td>
    </tr>
    </tbody>
    </table>

    The following proxy service is generated:

    -   [**Design View**](#7cd1666f7a754f59a8cbbd1df776f759)
    -   [**Source View**](#8727b4006ef34c6d9e753df9a1fa14cb)

    ![](attachments/119130357/119130359.png){width="600"}

    ``` java
        <?xml version="1.0" encoding="UTF-8"?>
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

2.  Now, let's set the email sender details by adding **Property**
    mediators to the mediation sequence. If these details are not
    provided in the proxy service, the system uses the email [sender
    configurations in the axis2.xml](#MailToTransport-enableMailTo)
    explained above. Shown below is the new mediation sequence:

        !!! tip
    
        Note the following:
    
        -   You need to update the property values with actual values of the
            mail sender account.
        -   In some email service providers, the value for the
            'mail.smtp.user' property is the same as the email address of
            the account.
    

    -   [**Design View**](#98c9a834a905468aa0357e150ededc92)
    -   [**Source View**](#9802631cbddd4131b9bbd071dd693bd7)

    ![](attachments/119130357/119130358.png){width="700"}

    ``` java
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

3.  Deploy the proxy service in you WSO2 EI server.
4.  Invoke the proxy service by sending a request.
5.  Check your inbox. You will receive an email from the email sender
    that you configured for the proxy service.

### Receiving emails to a mediation sequence

Follow the steps given below to configure your mediation sequence to
receive emails, and then process the contents within the sequence.

#### Step 1: Enabling the transport receiver

To enable the MailTo receiver globally, uncomment the following:

``` html/xml
    <transportReceiver name="mailto" class="org.apache.axis2.transport.mail.MailTransportListener">
    </transportReceiver> 
```

The MailTo transport receiver should be configured at service level and
each service configuration should explicitly state the mail transport
receiver configuration. This is required to enable different services to
receive mails over different mail accounts and configurations.

!!! info

Note

You need to provide correct parameters for a valid mail account at the
service level.


#### Step 2: Configuring the mediation flow

Once the MailTo transport receiver is enabled, you can configure email
messaging within your mediation flow by applying the MailTo transport
properties. The MailTo transport listener implementation can be
configured by setting the parameters as described in the JavaMail API
documentation. For IMAP related properties, see [IMAP Package
Summary](https://javaee.github.io/javamail/docs/api/com/sun/mail/imap/package-summary.html)
. For POP3 properties, see [POP3 Package
Summary](https://javaee.github.io/javamail/docs/api/com/sun/mail/pop3/package-summary.html)
. The MailTo transport listener also supports the following transport
parameters in addition to the parameters described in the JavaMail API
documentation.

!!! tip

In the following transport parameter tables, the literals displayed in
italics in the **Possible Values** column should be considered as fixed
literal constant values. Those values can be directly specified in the
transport configuration.


<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>e.g.Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>              transport.mail.Address             </code></p></td>
<td><p>The mail address from which this service should fetch incoming mails.</p></td>
<td><p>Yes</p></td>
<td><p>A valid e-mail address</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>             transport.mail.bodyWhenAttached            </code></td>
<td><p>The content for the body of the mail when sending a mail with an attachment.</p></td>
<td>No</td>
<td>The text you want to appear in the mail body</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><p><code>              transport.mail.Folder             </code></p></td>
<td><p>The mail folder in the server from which the listener should fetch incoming mails.</p></td>
<td><p>No</p></td>
<td><p>A valid mail folder name (e.g., inbox)</p></td>
<td><p>inbox folder if that is available or else the root folder</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.mail.Protocol             </code></p></td>
<td><p>The mail protocol to be used to receive messages.</p></td>
<td><p>No</p></td>
<td><p><em>pop3, imap</em></p></td>
<td><p>imap</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.mail.PreserveHeaders             </code></p></td>
<td><p>A comma separated list of mail header names that this receiver should preserve in all incoming messages.</p></td>
<td><p>No</p></td>
<td><p>A comma separated list</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><p><code>              transport.mail.RemoveHeaders             </code></p></td>
<td><p>A comma separated list of mail header names that this receiver should remove from incoming messages.</p></td>
<td><p>No</p></td>
<td><p>A comma separated list</p></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><p><code>              transport.mail.ActionAfterProcess             </code></p></td>
<td><p>Action to perform on the mails after processing them.</p></td>
<td><p>No</p></td>
<td><p><em>MOVE, DELETE</em></p></td>
<td><p>DELETE</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.mail.ActionAfterFailure             </code></p></td>
<td><p>Action to perform on the mails after a failure occurs while processing them.</p></td>
<td><p>No</p></td>
<td><p><em>MOVE, DELETE</em></p></td>
<td><p>DELETE</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.mail.MoveAfterProcess             </code></p></td>
<td><p>Folder to move the mails after processing them.</p></td>
<td><p>Required if ActionAfterProcess is MOVE</p></td>
<td><p>A valid mail folder name</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><p><code>              transport.mail.MoveAfterFailure             </code></p></td>
<td><p>Folder to move the mails after encountering a failure.</p></td>
<td><p>Required if ActionAfterFailure is MOVE</p></td>
<td><p>A valid mail folder name</p></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><p><code>              transport.mail.ProcessInParallel             </code></p></td>
<td><p>Whether the receiver should process incoming mails in parallel or not. This works only if the mail protocol supports processing incoming mails in parallel. (e.g., IMAP)</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p>false</p></td>
</tr>
<tr class="even">
<td><p><code>              transport.ConcurrentPollingAllowed             </code></p></td>
<td><p>Whether the receiver should poll for multiple messages concurrently.</p></td>
<td><p>No</p></td>
<td><p><em>true, false</em></p></td>
<td><p>false</p></td>
</tr>
<tr class="odd">
<td><p><code>              transport.mail.MaxRetryCount             </code></p></td>
<td><p>Maximum number of retry operations to be performed when fetching incoming mails.</p></td>
<td><p>Yes</p></td>
<td><p>A positive integer</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><p><code>              transport.mail.ReconnectTimeout             </code></p></td>
<td><p>The reconnect timeout in milliseconds to be used when fetching incoming mails.</p></td>
<td><p>Yes</p></td>
<td><p>A positive integer</p></td>
<td><br />
</td>
</tr>
</tbody>
</table>

## What's Next?

-   [Carrying
    Messages](https://docs.wso2.com/display/EI650/Carrying+Messages)
-   [Sample 255: Switching from FTP Transport Listener to Mail Transport
    Sender](https://docs.wso2.com/display/EI610/Sample+255%3A+Switching+from+FTP+Transport+Listener+to+Mail+Transport+Sender)
-   [Sample 256: Proxy Services with the MailTo
    Transport](https://docs.wso2.com/display/EI610/Sample+256%3A+Proxy+Services+with+the+MailTo+Transport)
-   [Sample 271: File
    Processing](https://docs.wso2.com/display/EI610/Sample+271%3A+File+Processing)
