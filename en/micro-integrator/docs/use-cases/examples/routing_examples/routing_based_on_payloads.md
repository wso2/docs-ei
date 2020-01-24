# Routing Based on Message Payloads
    
The example scenario uses two back-end services (IBM and MSFT) and illustrates how a request message is routed to the backend based on the message content (payload). If the request is made for IBM, the request is routed to the IBM stock inventory service. If the request is for MSFT, it is routed to the MSFT stock inventory service. 
    
## Synapse configuration
    
Listed below are the synapse configurations (proxy service) that are necessary for implementing this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="ContentBasedRoutingProxy" xmlns="http://ws.apache.org/ns/synapse" transports="https http" startOnLoad="true" trace="disable">
    <target>
       <!-- When a request arrives the following sequence will be followed -->   
       <inSequence>
         <!-- The content of the incoming message will be isolated -->
         <switch source="//ser:getQuote/ser:request/xsd:symbol" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
                <case regex="IBM">
                    <!-- Will Route the content to the appropriate destination -->  
                    <send>
                         <endpoint>
                             <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                         </endpoint>
                    </send>
                </case>
                <case regex="MSFT">
                    <send>
                        <endpoint>
                            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                        </endpoint>
                    </send>
                </case>
                <default>
                 <!-- it is possible to assign the result of an XPath expression as well -->
                <property name="symbol" expression="fn:concat('Normal Stock - ', //m0:getQuote/m0:request/m0:symbol)" xmlns:m0="http://services.samples"/>
                </default>
        </switch>      
        </inSequence>
        <outSequence>
            <send/>
        </outSequence>
    </target>     
</proxy>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. [Create the proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service:

1. Download the [stockquote_service.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar).
2. Open a terminal, navigate to the location of the downloaded service, and run it using the following command:

    ```bash
    java -jar stockquote_service.jar
    ```

Send a request to get the IBM stock quote as shown below.

```xml
HTTP method: POST 
Request URL: http://localhost:8290/services/ContentBasedRoutingProxy
SOAPAction: "urn:getQuote"
Message Body:
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

You will receive the following response:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://services.samples" xmlns:ax21="http://services.samples/xsd">
    <soapenv:Body>
        <ns:getQuoteResponse>
            <ax21:change>-2.86843917118114</ax21:change>
            <ax21:earnings>-8.540305401672558</ax21:earnings>
            <ax21:high>-176.67958828498735</ax21:high>
            <ax21:last>177.66987465262923</ax21:last>
            <ax21:low>-176.30898912339075</ax21:low>
            <ax21:marketCap>5.649557998178506E7</ax21:marketCap>
            <ax21:name>IBM Company</ax21:name>
            <ax21:open>185.62740369461244</ax21:open>
            <ax21:peRatio>24.341353665128693</ax21:peRatio>
            <ax21:percentageChange>-1.4930577008849097</ax21:percentageChange>
            <ax21:prevClose>192.11844053187397</ax21:prevClose>
            <ax21:symbol>IBM</ax21:symbol>
            <ax21:volume>7791</ax21:volume>
        </ns:getQuoteResponse>
    </soapenv:Body>
</soapenv:Envelope>
```