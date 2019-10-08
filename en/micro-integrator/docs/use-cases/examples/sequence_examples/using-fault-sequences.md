# Using fault sequences 
## Example use case

WSO2 Micro Integrator provides fault sequences for dealing with errors. Whenever an error occurs, the mediation engine attempts to provide as much information as possible on the error to the user by initializing the following properties on the erroneous message:

-	ERROR_CODE
-   ERROR_MESSAGE
-   ERROR_DETAIL
-   ERROR_EXCEPTION

## Synapse configuration

Note how the ERROR_MESSAGE property is being used to get the error message text. If you want to customize the error message that is sent back to the client, you can use the [Fault mediator](https://docs.wso2.com/display/EI650/Fault+Mediator) as demonstrated in [Sample 5: Creating SOAP Fault Messages and Changing the Direction of a Message](https://docs.wso2.com/display/ESB500/Sample+5%3A+Creating+SOAP+Fault+Messages+and+Changing+the+Direction+of+a+Message). 

Within the fault sequence, you can access these property values using
the `         get-property        ` XPath function. 

```xml tab='Fault Sequence'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="fault">
    <log level="custom">
        <property name="text" value="An unexpected error occured"/>
        <property name="message" expression="get-property('ERROR_MESSAGE')"/>
    </log>
    <drop/>
</sequence>
```

```xml tab='Error Handling Sequence with Logs'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="sunErrorHandler">
    <log level="custom">
        <property name="text" value="An unexpected error occured for stock SUN"/>
        <property name="message" expression="get-property('ERROR_MESSAGE')"/>
    </log>
    <drop/>
</sequence>
```

```xml tab='Proxy Service'
<proxy xmlns="http://ws.apache.org/ns/synapse" name="FaultTestProxy" startOnLoad="true" transports="http https">
    <target>
        <inSequence>
            <switch source="//m0:getQuote/m0:request/m0:symbol" xmlns:m0="http://services.samples">
                <case regex="IBM">
                    <send>
                        <endpoint><address uri="http://localhost:9000/services/SimpleStockQuoteService"/></endpoint>
                    </send>
                </case>
                <case regex="MSFT">
                    <send>
                        <endpoint key="bogus"/>
                    </send>
                </case>
                <case regex="SUN">
                    <sequence key="sunSequence"/>
                </case>
            </switch>
            <drop/>
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
    </target>
</proxy>
```

```xml tab='Error Handling Sequence'
<sequence xmlns="http://ws.apache.org/ns/synapse" name="sunSequence" onError="sunErrorHandler">
    <send>
        <endpoint key="sunPort"/>
    </send>
</sequence>
```

log mediator as follows to log the actual error message:

```
<log level="custom">  
    <property name="text" value="An unexpected error occured"/>
    <property name="message" expression="get-property('ERROR_MESSAGE')"/>
</log>
``` 

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the mediation artifacts with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Invoke the sample Proxy Service:
```xml
POST http://localhost:8290/services/FaultTestProxy HTTP/1.1
Accept-Encoding: gzip,deflate
Content-Type: text/xml;charset=UTF-8
SOAPAction: "urn:mediate"
Content-Length: 263
Host: Chanikas-MacBook-Pro.local:8290
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)

<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Header />
   <soapenv:Body>
      <placeOrder xmlns="http://org.apache.synapse/xsd">
         <order>
            <price>50</price>
            <quantity>10</quantity>
            <symbol>SUN</symbol>
         </order>
      </placeOrder>
   </soapenv:Body>
</soapenv:Envelope>
```

The following line is getting logged:
```
INFO {org.apache.synapse.mediators.builtin.LogMediator} - text = An unexpected error occured for stock SUN, message = Couldn't find the endpoint with the key : sunPort
```