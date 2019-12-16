# Using a Simple Proxy Service
This example demonstrates how to use a simple proxy service to expose a back-end service. In this example, a request received by the proxy service is forwarded to the sample service hosted in the backend.

## Synapse configuration
Following is a sample proxy service configuration that we can used to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

An `inSequence` or `endpoint` or both of these would decide how the message would be handled after the proxy service receives the message. The
`outSequence` defines how the response is handled before it is sent back to the client.

```xml
<proxy name="StockQuoteProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:/path/to/sample_proxy_1.wsdl"/>
</proxy>
```

## Build and run

The wsdl file `sample_proxy_1.wsdl` can be downloaded from [sample_proxy_1.wsdl](https://github.com/wso2-docs/WSO2_EI/blob/master/samples-protocol-switching/sample_proxy_1.wsdl).
The wsdl uri needs to be updated with the path to the sample_proxy_1.wsdl file.

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. [Create the proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

When the Micro Integrator starts, you could go to the following URL and view the WSDL generated for the proxy service defined in the configuration. 

```bash
http://localhost:8290/services/StockQuoteProxy?wsdl
```

This WSDL is based on the source WSDL supplied in the proxy service definition and is updated to reflect the proxy service EPR.

Set up the back-end service:

1. Download the [stockquote_service.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar).
2. Open a terminal, navigate to the location of the downloaded service, and run it using the following command:

    ```bash
    java -jar stockquote_service.jar
    ```

Send the payloads listed below as SOAP messages:

-   Send the following payload to receive a response containing the last sales price for the
    stock.

    ```xml
    <ser:getQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
        <ser:request>
            <xsd:symbol>IBM</xsd:symbol>
        </ser:request>
    </ser:getQuote>
    ```

-   Send the following payload to get simple quote response containing the last sales price for stock.

    ```xml
    <ser:getSimpleQuote xmlns:ser="http://services.samples">
        <ser:symbol>IBM</ser:symbol>
    </ser:getSimpleQuote>
    ```

-   Send the following payload to get quote reports for the stock over a number of days (i.e. last 100 days of the year).

    ```xml
    <ser:getFullQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
        <ser:request>
            <xsd:symbol>IBM</xsd:symbol>
        </ser:request>
    </ser:getFullQuote>
    ```

-   Send the following payload as an order for stocks using a
    one way request

    ```xml
    <ser:placeOrder xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
        <ser:order>
            <xsd:price>3.141593E0</xsd:price>
            <xsd:quantity>4</xsd:quantity>
            <xsd:symbol>IBM</xsd:symbol>
        </ser:order>
    </ser:placeOrder>
    ```

-   Send the following paylaod to get a market activity report
    for the day (i.e. quotes for multiple symbols)

    ```xml
    <ser:getMarketActivity xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
        <ser:request>
            <xsd:symbols>IBM</xsd:symbols>
            ...
            <xsd:symbols>MSFT</xsd:symbols>
        </ser:request>
    </ser:getMarketActivity>
    ```
