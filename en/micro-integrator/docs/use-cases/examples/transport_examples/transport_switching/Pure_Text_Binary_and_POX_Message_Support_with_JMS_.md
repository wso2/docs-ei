# Sample 252: Pure Text (Binary) and POX Message Support with JMS

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

Objective: Pure POX/Text and Binary JMS Proxy services, including MTOM.

###### **Prerequisites**

-   Configure JMS for ESB.
-   Start the Axis2 server and deploy the
    `          SimpleStockQuoteService         ` and the
    `          MTOMSwASampleService         ` if not already done.

``` html/xml
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <sequence name="text_proxy">
        <header name="Action" value="urn:placeOrder"/>
        <script language="js"><![CDATA[
            var args = mc.getPayloadXML().toString().split(" ");
            mc.setPayloadXML(
            <m:placeOrder xmlns:m="http://services.samples/xsd">
                <m:order>
                    <m:price>{args[0]}</m:price>
                    <m:quantity>{args[1]}</m:quantity>
                    <m:symbol>{args[2]}</m:symbol>
                </m:order>
            </m:placeOrder>);
        ]]></script>
        <property action="set" name="OUT_ONLY" value="true"/>
        <property name="messageType" value="text/xml" scope="axis2"/>
        <send>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="pox"/>
            </endpoint>
        </send>
    </sequence>
    <sequence name="mtom_proxy">
        <property action="set" name="OUT_ONLY" value="true"/>
        <header name="Action" value="urn:oneWayUploadUsingMTOM"/>
        <send>
            <endpoint>
                <address uri="http://localhost:9000/services/MTOMSwASampleService" optimize="mtom"/>
            </endpoint>
        </send>
    </sequence>
    <sequence name="pox_proxy">
        <property action="set" name="OUT_ONLY" value="true"/>
        <header name="Action" value="urn:placeOrder"/>
        <send>
            <endpoint>
                <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
            </endpoint>
        </send>
    </sequence>
    <sequence name="out">
        <send/>
    </sequence>
    <proxy name="JMSFileUploadProxy" transports="jms">
        <target inSequence="mtom_proxy" outSequence="out"/>
        <parameter name="transport.jms.Wrapper">{http://services.samples/xsd}element</parameter>
    </proxy>
    <proxy name="JMSTextProxy" transports="jms">
        <target inSequence="text_proxy" outSequence="out"/>
        <parameter name="transport.jms.Wrapper">{http://services.samples/xsd}text</parameter>
    </proxy>
    <proxy name="JMSPoxProxy" transports="jms">
        <target inSequence="pox_proxy" outSequence="out"/>
        <parameter name="transport.jms.ContentType">application/xml</parameter>
    </proxy>
</definitions>
```

This configuration creates three JMS Proxy Services named
`         JMSFileUploadProxy        ` , `         JMSTextProxy        `
and `         JMSPoxProxy        ` exposed over JMS queues with the same
names as the services. The first part of this example demonstrates the
pure text message support with JMS, where a user sends a space separated
text JMS message of the form " `         <price> <qty> <symbol>        `
". ESB converts this message into a SOAP message and sends this to the
`         SimpleStockQuoteServices        ` '
`         placeOrder        ` operation. ESB uses the Script Mediator to
transform the text message into a XML payload using the Javascript
support available to tokenize the string. The Proxy Service property
named `         transport.jms.Wrapper        ` defines a custom wrapper
element `         QName        ` , to be used when wrapping text/binary
content into a SOAP envelope.

Execute JMS client as follows. This will post a pure text JMS message
with the content defined (for example, "12.33 1000 ACP") to the
specified JMS destination -
`         dynamicQueues/JMSTextProxy        ` .

``` java
ant jmsclient -Djms_type=text -Djms_payload="12.33 1000 ACP" -Djms_dest=dynamicQueues/JMSTextProxy
```

Following the debug logs, you could notice that ESB received the JMS
text message and transformed it into a SOAP payload as follows.

!!! info

Note

The wrapper element "( \[http://services.samples/xsd\] )text" has been
used to hold the text message content.

TEST  

``` java
INFO - To: , WSAction: urn:mediate, SOAPAction: urn:mediate, MessageID: ID:orxus.vedehen.org-50631-1225235276233-1:0:1:1:1, Direction: request,
Envelope:
<?xml version="1.0" encoding="utf-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
  <soapenv:Body>
    <axis2ns1:text xmlns:axis2ns1="http://services.samples/xsd">12.33 1000 ACP</axis2ns1:text>
  </soapenv:Body>
</soapenv:Envelope>
```

Now, you could see how the Script Mediator created a stock quote request
by tokenizing the text as follows, and sent the message to the
`         placeOrder        ` operation of the
`         SimpleStockQuoteService        ` .

``` java
INFO - To: , WSAction: urn:placeOrder, SOAPAction: urn:placeOrder, MessageID: ID:orxus.vedehen.org-50631-1225235276233-1:0:1:1:1, Direction: request,
Envelope:
<?xml version="1.0" encoding="utf-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
  <soapenv:Body>
    <placeOrder xmlns="http://services.samples">
      <order xmlns="http://services.samples/xsd">
        <price>12.33</price>
        <quantity>1000</quantity>
        <symbol>ACP</symbol>
      </order>
    </placeOrder>
  </soapenv:Body>
</soapenv:Envelope>
```

The sample Axis2 server would now accept the one way message and issue
the following message:

``` java
Wed Apr 25 19:50:56 LKT 2007 samples.services.SimpleStockQuoteService :: Accepted order for : 1000 stocks of ACP at $ 12.33
```

The next section of this example demonstrates how a pure binary JMS
message could be received and processed through ESB. The configuration
creates a Proxy Service Sample named
`         JMSFileUploadProxy        ` that accepts binary messages and
wraps them into a custom element "( http://services.samples/xsd
)element." The received message is then forwarded to the
`         MTOMSwASampleService        ` using the SOAP action
`         urn:oneWayUploadUsingMTOM        ` and optimizing binary
content using MTOM. To execute this sample, use the JMS client to
publish a pure binary JMS message containing the file
./../../repository/samples/resources/mtom/asf-logo.gif to the JMS
destination dynamicQueues/JMSFileUploadProxy as follows:

``` java
ant jmsclient -Djms_type=binary -Djms_dest=dynamicQueues/JMSFileUploadProxy -Djms_payload=./../../repository/samples/resources/mtom/asf-logo.gif
```

Examining the ESB debug logs reveals that the binary content was
received over JMS and wrapped with the specified element into a SOAP
infoset as follows:

``` java
INFO - To: , WSAction: urn:mediate, SOAPAction: urn:mediate, MessageID: ID:orxus.vedehen.org-50702-1225236039556-1:0:1:1:1, Direction: request,
Envelope:
<?xml version="1.0" encoding="utf-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
  <soapenv:Body>
    <axis2ns3:element xmlns:axis2ns3="http://services.samples/xsd">R0lGODlhgw...AAOw==</axis2ns3:element>
  </soapenv:Body>
</soapenv:Envelope>
```

Thereafter the message was sent as a MTOM optimized message as specified
by the `         format=mtom        ` attribute of the endpoint, to the
`         MTOMSwASampleService        ` using the SOAP action
`         urn:oneWayUploadUsingMTOM        ` . Once received by the
sample service, it is saved into a temporary file and could be verified
for correctness.

``` java
Wrote to file : ./../../work/temp/sampleServer/mtom-29208.gif
```

The final section of this example shows how a POX JMS message received
by ESB is sent to the `         SimpleStockQuoteService        ` as a
SOAP message. Use the JMS client as follows to create a POX (Plain Old
XML) message with a stock quote request payload (without a SOAP
envelope), and send it to the JMS destination
`         dynamicQueues/JMSPoxProxy        ` as follows:

``` java
ant jmsclient -Djms_type=pox -Djms_dest=dynamicQueues/JMSPoxProxy -Djms_payload=MSFT
```

ESB converts the POX message into a SOAP payload and sends to the
`         SimpleStockQuoteService        ` after setting the SOAP action
as `         urn:placeOrder        ` . The sample Axis2 server displays
a successful message on the receipt of the message as:

``` java
Wed Apr 25 20:24:50 LKT 2007 samples.services.SimpleStockQuoteService :: Accepted order for : 19211 stocks of MSFT at $ 172.39703010684752
```
