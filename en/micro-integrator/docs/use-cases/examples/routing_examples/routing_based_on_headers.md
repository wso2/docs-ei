# Routing Based on Message Headers
    
The example scenario uses an inventory for stocks as the back-end service and illustrates how a request message is routed to the backend based on the message header. When the router receives the stock request, it reads the request header and routes the message to the relevant mediation sequence for processing. The sequence will then forward the message to the endpoint.
    
## Synapse configuration
    
Listed below are the synapse configurations that are necessary for implementing this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml tab="Proxy Service"
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="HeaderBasedRoutingProxy" xmlns="http://ws.apache.org/ns/synapse" transports="https http" startOnLoad="true" trace="disable">
  <target>
     <!-- When a request arrives the following sequence will be followed -->   
     <inSequence>
       <switch xmlns:ns="http://org.apache.synapse/xsd" source="get-property('transport','CustomHeader')">
              <case regex="application/json">
                  <log level="custom"> 
                   <property name="'CustomHeader'" value="application/json" /> 
                  </log> 
                  <sequence key="sequence1" />
              </case>
              <case regex="text/xml">
                  <log level="custom"> 
                    <property name="'CustomHeader'" value="text/xml" /> 
                  </log> 
                  <sequence key="sequence2" />
              </case>
              <default>
              <log level="custom"> 
                  <property name="AcceptHeader" value="other" /> 
              </log> 
              <sequence key="sequence3" />
              </default>
      </switch>      
      </inSequence>
      <outSequence>
          <send/>
      </outSequence>
  </target>
</proxy>
```

```xml tab="Sequence 1"
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="sequence1"> 
        <sequence key="send_seq"/> 
        <property name="messageType" value="application/json" scope="axis2"/>
        <respond/>
</sequence>
```

```xml tab="Sequence 2"
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="sequence2"> 
        <sequence key="send_seq"/> 
        <property name="messageType" value="text/xml" scope="axis2"/>
        <respond/>
</sequence>
```

```xml tab="Sequence 3"
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="sequence3"> 
        <sequence key="send_seq"/> 
        <respond/>
</sequence>
```    

```xml tab="send_seq"
<?xml version="1.0" encoding="UTF-8"?>
<sequence xmlns="http://ws.apache.org/ns/synapse" name="send_seq"> 
    <header name="Action" scope="default" value="urn:getQuote"/>
    <call> 
      <endpoint name="simple">
       <address uri="http://localhost:9000/services/SimpleStockQuoteService"/> 
      </endpoint> 
    </call> 
</sequence>
```   

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) and [mediation sequences](../../../../develop/creating-artifacts/creating-reusable-sequences) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service:

1. Download the [stockquote_service.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar).
2. Open a terminal, navigate to the location of the downloaded service, and run it using the following command:

    ```bash
    java -jar stockquote_service.jar
    ```

Invoke the sample Proxy Service as shown below.

```xml
HTTP method: POST 
Request URL: http://localhost:8290/services/HeaderBasedRoutingProxy
Content-Type: text/xml;charset=UTF-8
SOAPAction: "urn:mediate"
CustomHeader: application/json
Message Body:
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
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

```json
{"Envelope":
  {"Body":
    {"getQuoteResponse":
      {"change":-2.86843917118114,
       "earnings":-8.540305401672558,
       "high":-176.67958828498735,
       "last":177.66987465262923,
       "low":-176.30898912339075,
       "marketCap":56495579.98178506,
       "name":"IBM Company",
       "open":185.62740369461244,
       "peRatio":24.341353665128693,
       "percentageChange":-1.4930577008849097,
       "prevClose":192.11844053187397,
       "symbol":"IBM","volume":7791}
    }
  }
}
```