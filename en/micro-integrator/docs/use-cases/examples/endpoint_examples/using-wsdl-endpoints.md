# Using a WSDL Endpoint as the Target Endpoint

### Introduction

This sample demonstrates how you can use a WSDL endpoint as the target
endpoint. The configuration in this sample uses a WSDL endpoint inside
the send mediator. This WSDL endpoint extracts the target endpoint reference from the WSDL document specified in the configuration. In this
configuration the WSDL document is specified as a URI.

### Building the sample

The XML configuration for this sample is as follows:

```
<proxy name="SimpleStockQuoteProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
   <target>
       <inSequence>
            <header name="Action" value="urn:placeOrder"/>
            <call>
                <endpoint>
                    <wsdl uri="file:/path/to/sample_proxy_1.wsdl"
                        service="SimpleStockQuoteService" port="SimpleStockQuoteServiceHttpSoap11Endpoint"/>
                </endpoint>
            </call>
            <respond/>
       </inSequence>
       <outSequence>
            <send/>
       </outSequence>
       <faultSequence/>
   </target>
</proxy>
```

The wsdl file `sample_proxy_1.wsdl` can be downloaded from  [sample_proxy_1.wsdl](https://github.com/wso2-docs/WSO2_EI/blob/master/samples-protocol-switching/sample_proxy_1.wsdl). 
The wsdl uri of the endpoint needs to be updated with the path to the sample_proxy_1.wsdl file

Set up the back-end service.

* Download the [stockquote_service.jar](
https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar)

* Run the mock service using the following command
```
$ java -jar stockquote_service.jar
```

### Executing the sample

Invoking the service.

The request sent by the client is as follows:

```bash
POST http://localhost:8290/services/SimpleStockQuoteProxy.SimpleStockQuoteProxyHttpSoap11Endpoint HTTP/1.1
Accept-Encoding: gzip,deflate
Content-Type: text/xml;charset=UTF-8
SOAPAction: "urn:mediate"
Content-Length: 428
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Header/>
   <soapenv:Body>
   <m0:placeOrder xmlns:m0="http://services.samples">
            <m0:order>
                <m0:price>172.23182849731984</m0:price>
                <m0:quantity>18398</m0:quantity>
                <m0:symbol>IBM</m0:symbol>
            </m0:order>
        </m0:placeOrder>
   </soapenv:Body>
</soapenv:Envelope>
```

### Analyzing the output

When the client is run, you will see the following output as the response:

``` xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://services.samples" xmlns:ax21="http://services.samples/xsd">
    <soapenv:Body>
        <ns:placeOrderResponse>
            <ax21:status>created</ax21:status>
        </ns:placeOrderResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

The WSDL endpoint
inside the send mediator extracts the EPR from the WSDL document.
Since WSDL documents can have many services and many ports inside each
service, the service and port of the required endpoint has to be
specified in the configuration via the `         service        ` and
`         port        ` attributes respectively. When it comes to
address endpoints, the QoS parameters for the endpoint can be specified
in the configuration. An excerpt taken from
`         sample_proxy_1.wsdl        ` , which is the WSDL document used
in above sample is given below.

```
    <wsdl:service name="SimpleStockQuoteService">
       <wsdl:port name="SimpleStockQuoteServiceHttpSoap11Endpoint" binding="ns:SimpleStockQuoteServiceSoap11Binding">
                <soap:address location="http://localhost:9000/services/SimpleStockQuoteService.SimpleStockQuoteServiceHttpSoap11Endpoint"/>
       </wsdl:port>
       <wsdl:port name="SimpleStockQuoteServiceHttpSoap12Endpoint" binding="ns:SimpleStockQuoteServiceSoap12Binding">
                <soap12:address location="http://localhost:9000/services/SimpleStockQuoteService.SimpleStockQuoteServiceHttpSoap12Endpoint"/>
       </wsdl:port>
    </wsdl:service>
```

According to the above WSDL, the service and port specified in the
configuration refers to the endpoint address
`                   http://localhost:9000/services/SimpleStockQuoteService.SimpleStockQuoteServiceHttpSoap11Endpoint                 `
