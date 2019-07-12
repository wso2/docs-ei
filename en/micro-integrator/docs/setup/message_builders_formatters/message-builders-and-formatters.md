# Working with Message Builders and Formatters

### Overview

When a message comes in to the ESB profile of WSO2 Enterprise Integrator
(WSO2 EI), the receiving transport selects a **message builder** based
on the message's content type. It uses that builder to process the
message's raw payload data and convert it into SOAP. Conversely, when
sending a message out from ESB, a **message formatter** is used to build
the outgoing stream from the message. As with message builders, the
message formatter is selected based on the message's content type. In a
typical routing scenario of the ESB, here is the flow:

<a href=""><img src="../../../assets/img/message-builders-formatters.png"></a>

You can use the messageType property to change the message's content
type as it flows through ESB. For example, if the incoming message is in
JSON format and you want to transform it to XML, you could add the
messageType property before your mediators in the configuration:

``` java
    <property name="messageType" value="application/xml" scope="axis2"/>
```

### Configuring message builders and formatters

Message builders and formatters are specified in
`         <EI_HOME>/conf/axis2/axis2.xml        ` (or
`         <EI_HOME>/conf/axis2/tenant-axis2.xml        ` ). If you are
working in a [multi-tenant
environment](https://docs.wso2.com/display/EI611/Product+Administration#ProductAdministration-Configuringmultitenancy)
), under the `         messageBuilders        ` and
`         messageFormatters        ` configuration sections.

The ESB profile of WSO2 EI has a few default message builders, so even
if you do not specify them explicitly in `         axis2.xml        ` or
`         tenant-axis2.xml        ` , they will take effect when
messages of those content types come into the ESB profile. If you want
to use different builders, specify them in `         axis2.xml        `
or `         tenant-axis2.xml        ` to override the defaults. The ESB
profile does not have default message formatters, so it is important to
specify all of them in the `         axis2.xml        ` or
`         tenant-axis2.xml        ` configuration. Following are the
default message builders:

| Content type                      | Message Builder                                 |
|-----------------------------------|-------------------------------------------------|
| application/soap+xml              | org.apache.axis2.builder.SOAPBuilder            |
| multipart/related                 | org.apache.axis2.builder.MIMEBuilder            |
| text/xml                          | org.apache.axis2.builder.SOAPBuilder            |
| application/xop+xml               | org.apache.axis2.builder.MTOMBuilder            |
| application/xml                   | org.apache.axis2.builder.ApplicationXMLBuilder  |
| application/x-www-form-urlencoded | org.apache.axis2.builder.XFormURLEncodedBuilder |

### Using message relay

If you want to enable message relay, so that messages of a specific
content type are not built or formatted but simply pass through the ESB,
you can specify the message relay builder (
`         org.wso2.carbon.relay.BinaryRelayBuilder        ` ) for that
content type. For more information, see [Configuring Message
Relay](_Configuring_Message_Relay_) .

### Handling messages with no content type

To ensure that messages with no content type are handled gracefully
without causing errors, add the following to
`         axis2.xml        ` :

-   In the parameters section:
    `          <parameter name="defaultContentType" locked="false">"empty/content"</parameter>         `
-   In the message builders section:
    `          <messageBuilder contentType="empty/content" class="org.wso2.carbon.relay.BinaryRelayBuilder"/>         `
-   In the message formatters section:
    `          <messageFormatter contentType="empty/content" class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>         `

### Handling text/csv messages

There is no default builder or formatter for messages with the text/csv
content type. If you just want to pass these messages through the ESB,
you can configure the message relay builder and formatter. If you want
to process these messages, you can access the content inside the
request/response payload of CSV by configuring the
`         org.apache.axis2.format.PlainTextBuilder        ` and
`         org.apache.axis2.format.PlainTextFormatter        ` for the
text/csv content type in `         axis2.xml        ` . For example:

```java
<messagebuilder contenttype="text/csv" class="org.apache.axis2.format.PlainTextBuilder"/>
<messageformatter contenttype="text/csv" class="org.apache.axis2.format.PlainTextFormatter"/>
```

When a text/csv message comes into the ESB, the log will include an
entry similar to the following, and you can observe that the CSV data is
placed inside the payload:

``` java
    [2013-05-09 13:59:03,478] INFO - LogMediator To: , WSAction: urn:mediate, SOAPAction: urn:mediate, MessageID: urn:uuid:5B9A211341DCC368241368088143463, Direction: request, Envelope: <?xml version='1.0' encoding='utf-8'?><soapenv:envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"><soapenv:body><text xmlns="http://ws.apache.org/commons/ns/payload">charitha,wso2,colombo chamara,wso2G,galle </text></soapenv:body></soapenv:envelope>
```

### Handling illegal XML characters in plain text payloads

Plain text payloads that contain illegal XML characters (such as
unicodes) will not be successfully processed by the ESB. Therefore, you
must configure the system to replace the illegal characters in the
payload with an actual character. To enable this configuration, add the
parameter shown below (with a suitable unicode value) to the
`         XMLOutputFactory.properties        ` file (stored in the
`         <EI_HOME>/        ` directory). If this file does not exist in
your product by default, be sure to create a new file.

When this configuration is enabled, all the illegal characters found in
a payload will be replaced with the actual character that is represented
by the unicode value that you specify for the parameter. The below
example uses whitespaces (represented by by the ' \\u0020' unicode
value) to replace illegal characters in payloads.

``` java
    com.ctc.wstx.outputInvalidCharHandler.char=\u0020
```

### JSON message builders and formatters

The ESB Profile provides the following message builders and formatters
for JSON. These are configured in the Axis2 configuration files located
in the `         <EI_HOME>/conf/axis2        ` directory. Both types of
JSON builders use [StAXON](https://github.com/beckchr/staxon) as the
underlying JSON processor.

#### Default message builder and formatter

The default message builder and formatter of the ESB Profile of  WSO2 EI
are as follows:

-   `          org.wso2.carbon.integrator.core.json.JsonStreamBuilder         `
-   `          org.wso2.carbon.integrator.core.json.JsonStreamFormatter         `

The default message builder and formatter maintain the JSON
representation intact without converting it to XML during message
mediation. You can access the payload content using JSON Path or XPath
and convert the payload to XML at any point in the mediation flow.

The default message builder and formatter can also be used by default
for JSON mapping when you expose datasources as data services via the
ESB Profile of WSO2 EI.

#### Other message builders and formatters

Other message builders and formatters can be enabled by adding the
required parameters to the `         axis2.xml        ` file as
explained below.

If you want to convert the JSON representation to XML before the
mediation flow begins, you need to add the message builder and formatter
shown below. Note that some data loss can occur during the JSON to XML
to JSON conversion process.

- `<parameter name="passthruJsonBuilder">org.apache.synapse.commons.json.JsonBuilder<parameter>`
- `<parameter name="passthruJsonFormatter">org.apache.synapse.commons.json.JsonFormatter<parameter>`

You can enable the following builders and formatters for JSON mapping in
data services.

-  `\<parameter name="dsJsonBuilder"\>org.apache.axis2.json.gson.JsonBuilder\</parameter\>`
-  `\<parameter name="dsJsonFormatter"\>org.apache.axis2.json.JSONMessageFormatter\</parameter\>`

You can also enable the following builders and formatters if necessary.
Note that this is necessary for compatibility with WSO2 ESB:

-   `          <parameter name="passthruJSONBuilder">org.apache.axis2.json.JSONBuilder</parameter>         `
-   `          <parameter name="passthruJSONFormatter">org.apache.axis2.json.JSONMessageFormatter</parameter>         `
    `                   `
-   `          <parameter name="passthruJSONBuilder">org.apache.axis2.json.JSONStreamBuilder</parameter>         `
    `                   `
-   `           <parameter name="passthruJSONFormatter">org.apache.axis2.json.JSONStreamFormatter</parameter>          `

-   `          <parameter name="passthruJSONBuilder">org.apache.axis2.json.JSONBadgerfishOMBuilder</parameter>         `
-   `           <parameter name="passthruJSONFormatter">org.apache.axis2.json.JSONBadgerfishMessageFormatter</parameter>          `

> Always use the same type of builder and formatter combination. Mixing
different builders and formatters will cause errors at runtime.

> If you want the ESB Profile of WSO2 EI to handle JSON payloads that are
sent using a media type other than `         application/json        ` ,
you must register the JSON builder and formatter for that media type in
the following two files at minimum (for best results, register them in
all Axis2 configuration files found in the
`         <EI_HOME>/conf/axis2        ` directory):

-   `         <EI_HOME>/conf/axis2/axis2.xml         `
-   `          <EI_HOME>/conf/axis2/axis2_blocking_client.xml         `

For example, if the media type is `         text/javascript        ` ,
register the message builder and formatter as follows:

```java
<messageBuilder contentType="text/javascript"
                   class="org.apache.synapse.commons.json.JsonStreamBuilder"/>

<messageFormatter contentType="text/javascript"
                    class="org.apache.synapse.commons.json.JsonStreamFormatter"/>
```

> When you modify the builders/formatters in Axis2 configuration, make
sure that you have enabled only one correct message builder/formatter
pair for a given media type.


#### Validating JSON messages

If you want the  JSON builder to validate JSON messages that are
received by the ESB, the following property should be added to the
passthru `         -        ` http `         .properties        ` file
stored in the `         <EI_HOME>/conf/        ` directory. This
validation ensures that erroneous JSON messages are rejected by the ESB.

``` java
    force.json.message.validation=true
```

### Writing a custom Message Builder and Formatter

In addition to using the default message builders and formatters in WSO2
EI, you can create your own custom message builders and formatters.

#### Custom message builder

Let's look at how to create a custom message builder using a sample
scenario where you need to Base64 encode an XML entry field. In this
sample, you retrieve the text content from the payload and then Base64
encode the text. This is then converted to SOAP, and the content is then
processed in the WSO2 EI mediation flow.

1.  You will first need to write a class implementing the
    `           org.apache.axis2.builder.Builder          ` interface in
    the Axis2 Kernel module and then override the
    `           processDocument          ` method. Within the
    `           processDocument          ` method, you can define your
    specific logic to process the payload content as required and then
    convert it to SOAP format.

    ``` xml
            package org.test.builder;

            import org.apache.axiom.om.OMAbstractFactory;
            import org.apache.axiom.om.OMElement;
            import org.apache.axiom.om.impl.OMNodeEx;
            import org.apache.axiom.om.impl.builder.StAXBuilder;
            import org.apache.axiom.om.impl.builder.StAXOMBuilder;
            import org.apache.axiom.om.util.StAXParserConfiguration;
            import org.apache.axiom.om.util.StAXUtils;
            import org.apache.axiom.soap.SOAPBody;
            import org.apache.axiom.soap.SOAPEnvelope;
            import org.apache.axiom.soap.SOAPFactory;
            import org.apache.axis2.AxisFault;
            import org.apache.axis2.Constants;
            import org.apache.axis2.builder.Builder;
            import org.apache.axis2.context.MessageContext;
            import org.apache.commons.codec.binary.Base64;

            import javax.xml.stream.XMLStreamException;
            import java.io.IOException;
            import java.io.InputStream;
            import java.io.PushbackInputStream;

            public class CustomBuilderForTextXml implements Builder{
                public OMElement processDocument(InputStream inputStream, String s, MessageContext messageContext) throws AxisFault {
                    SOAPFactory soapFactory = OMAbstractFactory.getSOAP11Factory();
                    SOAPEnvelope soapEnvelope = soapFactory.getDefaultEnvelope();

                    PushbackInputStream pushbackInputStream = new PushbackInputStream(inputStream);

                    try {
                        int byteVal = pushbackInputStream.read();
                        if (byteVal != -1) {
                            pushbackInputStream.unread(byteVal);

                            javax.xml.stream.XMLStreamReader xmlReader = StAXUtils.createXMLStreamReader(StAXParserConfiguration.SOAP,
                                    pushbackInputStream, (String) messageContext.getProperty(Constants.Configuration.CHARACTER_SET_ENCODING));

                            StAXBuilder builder = new StAXOMBuilder(xmlReader);
                            OMNodeEx documentElement = (OMNodeEx) builder.getDocumentElement();
                            documentElement.setParent(null);
                            String elementVal = ((OMElement) documentElement).getText();
                            byte[]   bytesEncoded = Base64.encodeBase64(elementVal.getBytes());
                            ((OMElement) documentElement).setText(new String(bytesEncoded ));
                            SOAPBody body = soapEnvelope.getBody();
                            body.addChild(documentElement);
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    } catch (XMLStreamException e) {
                        e.printStackTrace();
                    }

                    return soapEnvelope;
                }
            }
    ```

2.  Create a JAR file of this class and add it into the classpath of the
    Axis2 installation, i.e., the `          <EI_HOME>/lib         `
    folder.
3.  To enable your custom message builder for content type text/xml, add
    the following line in the Message Buillders section in the
    `          <EI_HOME>/conf/axis2/axis2.xml         ` file (or
    `          <EI_HOME>/conf/axis2/tenant-axis2.xml         ` if you
    are working in a [multi-tenant
    environment](https://docs.wso2.com/display/EI611/Product+Administration#ProductAdministration-Configuringmultitenancy)
    ):

``` xml
    <messageBuilder contentType="text/xml" class="org.test.builder.http.CustomBuilderForTextXml"/>
```

#### Custom message formatter

Similarly, you can write your own message formatter to manipulate the
outgoing payload from the WSO2 EI.

When creating a custom message formatter, you will need to create a
class implementing the
`         org.apache.axis2.transport.MessageFormatter        ` interface
and then override the `         writeTo        ` method. You can
implement your logic within the `         writeTo        ` method.

Add the following line in the Message Formatters section in the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file (or
`         <EI_HOME>/conf/axis2/tenant-axis2.xml        ` if you are
working in a [multi-tenant
environment](https://docs.wso2.com/display/EI611/Product+Administration#ProductAdministration-Configuringmultitenancy)
):

``` xml
    <messageFormatter contentType= "text/xml" class="org.apache.axis2.transport.http.SOAPMessageFormatter" />
```

> The class name used in the above line should be the name used for the
class when writing the formatter.
