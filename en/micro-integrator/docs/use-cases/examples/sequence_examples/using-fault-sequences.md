# Using Fault Sequences 
WSO2 Micro Integrator provides fault sequences for dealing with errors. Whenever an error occurs, the mediation engine attempts to provide as much information as possible on the error to the user by initializing the following properties on the erroneous message:

-	ERROR_CODE
-   ERROR_MESSAGE
-   ERROR_DETAIL
-   ERROR_EXCEPTION

## Synapse configuration
Following are the integration artifacts that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

-   Proxy service:
    ```xml
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
    
-   Mediation sequences:

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

    ```xml tab='Error Handling Sequence'
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="sunSequence" onError="sunErrorHandler">
        <send>
            <endpoint key="sunPort"/>
        </send>
    </sequence>
    ```

Note how the `ERROR_MESSAGE` property is being used to get the error message text. Within the fault sequence, you can access these property values using
the `get-property` XPath function. The following log mediator logs the actual error message:

```xml
<log level="custom">  
    <property name="text" value="An unexpected error occured"/>
    <property name="message" expression="get-property('ERROR_MESSAGE')"/>
</log>
``` 

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service), and the [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service:

1. Download the [stockquote_service.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar).
2. Open a terminal, navigate to the location of the downloaded service, and run it using the following command:

    ```bash
    java -jar stockquote_service.jar
    ```

Send a request to invoke the proxy service:
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
      <getQuote xmlns="http://services.samples">
         <request>
            <price>50</price>
            <quantity>10</quantity>
            <symbol>SUN</symbol>
         </request>
      </getQuote>
   </soapenv:Body>
</soapenv:Envelope>
```

The following line is logged:
```bash
INFO {org.apache.synapse.mediators.builtin.LogMediator} - text = An unexpected error occured for stock SUN, message = Couldn't find the endpoint with the key : sunPort
```