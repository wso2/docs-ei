# Converting SOAP Messages to JSON

Let's consider the same scenario that we used in the [JSON to SOAP conversion examples](../json-to-soap-conversion). We have a SOAP-based backend and a JSON client. When the JSON client sends a message to the SOAP client, the proxy service in the Micro Integrator first converts the JSON message to SOAP (as demonstrated by the [JSON to SOAP conversion examples](../json-to-soap-conversion)). 

The following examples demonstrate how the Micro Integrator converts the response message from the backend (which is in SOAP) back to JSON before delivering to the client.

## Using the messageType property

Let's convert SOAP messages to JSON using the [messageType property](../../../../references/mediators/property-reference/generic-Properties).

### Synapse configuration
Following is a sample proxy service configuration that we can use to implement this scenario. Note that the [PayloadFactory mediator](../../../../references/mediators/payloadFactory-Mediator) is used in the **in sequence** to convert the incoming JSON request to SOAP. The [messageType property](../../../../references/mediators/property-reference/generic-Properties) is used in the **out sequence** to convert the SOAP response back to JSON.

See the instructions on how to [build and run](#build-and-run) this example.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="SOAP_To_JSON_Convert_Msgtype_Proxy"
       startOnLoad="true"
       statistics="disable"
       trace="disable"
       transports="http,https">
   <target>
      <inSequence>
            <payloadFactory media-type="xml">
                <format>
                    <soapenv:Envelope xmlns:ser="http://services.samples" xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://services.samples/xsd">
                        <soapenv:Header/>
                        <soapenv:Body>
                            <ser:getQuote>
                                <ser:request>
                                    <xsd:symbol>$1</xsd:symbol>
                                </ser:request>
                            </ser:getQuote>
                        </soapenv:Body>
                    </soapenv:Envelope>
                </format>
                <args>
                    <arg evaluator="json" expression="$.symbol"/>
                </args>
            </payloadFactory>
            <send>
                <endpoint>
                    <address format="soap11" uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </inSequence>
      <outSequence>
         <property name="messageType" scope="axis2" value="application/json"/>
         <send/>
      </outSequence>
   </target>
</proxy>
```

### Build and run

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

Invoke the proxy service:

- HTTP method: POST
- Request URL: http://localhost:8290/services/SOAP_To_JSON_Convert_Msgtype_Proxy
- Content-Type: application/json
- SoapAction: urn:getQuote
- Message Body:
    ```xml
    {"getQuote":
      {"request":
        {"symbol":"IBM"}
      }
    }
    ```

See that the IBM stock quote information is returned in JSON.

## Using the PayloadFactory Mediator

Let's convert SOAP messages to JSON using a [PayloadFactory mediator](../../../../references/mediators/payloadFactory-Mediator).

### Synapse configuration
Following is a sample proxy service configuration that we can use to implement this scenario. Note that [PayloadFactory mediators](../../../../references/mediators/payloadFactory-Mediator) are used in both the **in sequence** and the **out sequence**. The in sequence converts incoming JSON messages to SOAP, and the out sequence converts the SOAP responses back to JSON.

See the instructions on how to [build and run](#build-and-run) this example.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" 
       name="SOAP_To_JSON_Convert_PayloadFactory_Proxy" 
       startOnLoad="true" 
       transports="http https">
   <target>
      <inSequence>
         <payloadFactory media-type="xml">
            <format>
               <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
                  <soapenv:Header />
                  <soapenv:Body>
                     <ser:getQuote>
                        <ser:request>
                           <xsd:symbol>$1</xsd:symbol>
                        </ser:request>
                     </ser:getQuote>
                  </soapenv:Body>
               </soapenv:Envelope>
            </format>
            <args>
               <arg evaluator="json" expression="$.symbol" />
            </args>
         </payloadFactory>
         <send>
            <endpoint>
               <address format="soap11" uri="http://localhost:9000/services/SimpleStockQuoteService" />
            </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <payloadFactory media-type="json">
            <format>{"getQuoteResponse":
              {
                "change":"$1",
                "earnings":"$2",
                "high":"$3",
                  "last":"$4",
                "low":"$5",
                "marketCap":"$6",
                "name":"$7",
                "open":"$8",
                "perRatio":"$9",
                "percentageChange":"$10",
                "symbol":"$11",
                "volume":"$12"
              }
        }</format>
            <args>
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:change" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:earnings" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:high" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:last" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:low" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:marketCap" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:name" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:open" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:perRatio" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:percentageChange" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:symbol" />
               <arg xmlns:ax21="http://services.samples/xsd" xmlns:ns="http://services.samples" evaluator="xml" expression="//ns:getQuoteResponse/ax21:volume" />
            </args>
         </payloadFactory>
         <send />
      </outSequence>
   </target>
</proxy>
```

### Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. [Create the proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Invoke the proxy service:

- HTTP method: POST
- Request URL: http://localhost:8290/services/SOAP_To_JSON_Convert_PayloadFactory_Proxy
- Content-Type: application/json
- Soap Action: urn:getQuote
- Message Body:
    ```xml
    {"getQuote":
      {"request":
        {"symbol":"WSO2"}
      }
    }
    ```

See that the WSO2 stock quote information is returned in JSON.