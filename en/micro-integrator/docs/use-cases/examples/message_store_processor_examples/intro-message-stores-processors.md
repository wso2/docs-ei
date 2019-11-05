# Introduction to Message Store
This sample demonstrates the basic functionality of a [message store](../../../../references/synapse-properties/about-message-stores-processors).

## Synapse configuration

The XML configuration for this sample is as follows:

```xml tab='Proxy Service'
<proxy xmlns="http://ws.apache.org/ns/synapse" name="SampleProxy" transports="http" startOnLoad="true" trace="disable">
    <description/>
    <target>
        <inSequence>
            <log level="full"/>
            <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
            <store messageStore="MyStore" sequence="onStoreSequence"/>
        </inSequence>
        <faultSequence>
            <sequence key="OnError"/>
        </faultSequence>
    </target>
</proxy>
```

```xml tab='On Store Sequence'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="onStoreSequence">
    <log>
        <property name="On-Store" value="Storing message"/>
    </log>
</sequence>
```

```xml tab='Message Store'
<messageStore xmlns="http://ws.apache.org/ns/synapse" name="MyStore" />
```

```xml tab='OnError Sequence'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="OnError">
    <log level="full">
        <property name="MESSAGE" value="Executing default 'fault' sequence"/>
        <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
        <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
    </log>
    <drop/>
</sequence>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Invoke the service.

Send the following request:
```xml
POST http://localhost:9090/services/SampleProxy HTTP/1.1
Accept-Encoding: gzip,deflate
Content-Type: text/xml;charset=UTF-8
SOAPAction: "urn:getQuote"
Content-Length: 492
Host: localhost:9090
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
   <soapenv:Header/>
   <soapenv:Body>
      <ser:getQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
         <ser:request>
            <xsd:symbol>IBM</xsd:symbol>
         </ser:request>
      </ser:getQuote>
   </soapenv:Body>
</soapenv:Envelope>
```

In the proxy service, the store mediator will store the
`         getQuote        ` request message in the
`         MyStore        ` message store. Before storing the request,
the message store mediator will invoke the
`         onStoreSequence        ` sequence.

Analyze the logs and you will see the following log:

```xml
INFO {org.apache.synapse.mediators.builtin.LogMediator} - To: /services/SampleProxy, WSAction: urn:getQuote, SOAPAction: urn:getQuote, MessageID: urn:uuid:ab78ee5d-f5ed-4346-a0ea-1beb2e6c0b1d, Direction: request, On-Store = Storing message
```

You can then use the JMX view of the Synapse message store by using the
jconsole in order to view the stored messages and delete them.
