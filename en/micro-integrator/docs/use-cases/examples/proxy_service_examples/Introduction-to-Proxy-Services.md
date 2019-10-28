# Using a Simple Proxy Service
## Synapse configuration

An `inSequence` or `endpoint` or both of these would decide how the message would be handled after the Proxy Service receives the message. In the above example the request received is forwarded to the sample service hosted in the back-end service. The
`outSequence` defines how the response is handled before it is sent back to the client. By default, a Proxy Service is exposed over all transports configured for Micro Integrator, unless these are specifically mentioned through the `transports` attribute.

```xml
<proxy name="StockQuoteProxy">
    <target>
        <endpoint>
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <outSequence>
            <send/>
        </outSequence>
    </target>
    <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
</proxy>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. [Create the proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.

When the Micro Integrator starts, you could go to <http://localhost:8280/services/StockQuoteProxy?wsdl> and view the WSDL generated for the proxy service defined in the configuration. This WSDL is based on the source WSDL supplied in the proxy service definition and is updated to reflect the proxy service EPR.

Send the payloads listed below as SOAP messages:

-   Send the following payload to receive a response containing the last sales price for the
    stock.

    ```xml
    <m:getQuote xmlns:m="http://services.samples/xsd">
        <m:request>
            <m:symbol>IBM</m:symbol>
        </m:request>
    </m:getQuote>
    ```

-   Send the following payload as a quote request in a custom format. The Micro Integrator would transform this custom request into the standard stock quote request format and send it to the service. Upon receipt of the response, it would be transformed again to a custom response format and returned to the client, which will then display the last sales price.

    ```xml
    <m0:checkPriceRequest xmlns:m0="http://services.samples/xsd">
        <m0:Code>symbol</m0:Code>
    </m0:checkPriceRequest>
    ```

-   Send the following payload to get quote reports for the stock over a number of days (i.e. last 100 days of the year).

    ```xml
    <m:getFullQuote xmlns:m="http://services.samples/xsd">
        <m:request>
            <m:symbol>IBM</m:symbol>
        </m:request>
    </m:getFullQuote>
    ```

-   Send the following payload as an order for stocks using a
    one way request

    ```xml
    <m:placeOrder xmlns:m="http://services.samples/xsd">
        <m:order>
            <m:price>3.141593E0</m:price>
            <m:quantity>4</m:quantity>
            <m:symbol>IBM</m:symbol>
        </m:order>
    </m:placeOrder>
    ```

-   Send the following paylaod to get a market activity report
    for the day (i.e. quotes for multiple symbols)

    ```xml
    <m:getMarketActivity xmlns:m="http://services.samples/xsd">
        <m:request>
            <m:symbol>IBM</m:symbol>
            ...
            <m:symbol>MSFT</m:symbol>
        </m:request>
    </m:getMarketActivity>
    ```

!!! Info
    See `samples/axis2Client/src/samples/common/StockQuoteHandler.java` for sample responses expected by the clients.

```bash
ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy
```