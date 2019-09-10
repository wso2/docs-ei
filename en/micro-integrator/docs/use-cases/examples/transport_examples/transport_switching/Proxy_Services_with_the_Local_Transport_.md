# Proxy Services with the Local Transport

Proxy services with the [Local
transport](https://docs.wso2.com/display/EI650/Local+Transport) .

``` 
<definitions xmlns="http://ws.apache.org/ns/synapse">
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="LocalTransportProxy"
           transports="https http" startOnLoad="true" trace="disable">
        <target>
            <endpoint name="ep1">
                <address uri="local://services/SecondProxy"/>
            </endpoint>
            <inSequence>
                <log level="full"/>
                <log level="custom">
                    <property name="LocalTransportProxy" value="In sequence of LocalTransportProxy invoked!"/>
                </log>
            </inSequence>
            <outSequence>
                <log level="custom">
                    <property name="LocalTransportProxy" value="Out sequence of LocalTransportProxy invoked!"/>
                </log>
                <send/>
            </outSequence>
        </target>
        <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="SecondProxy"
           transports="https http" startOnLoad="true" trace="disable">
        <target>
            <endpoint name="ep2">
                <address uri="local://services/StockQuoteProxy"/>
            </endpoint>
            <inSequence>
                <log level="full"/>
                <log level="custom">
                    <property name="SecondProxy" value="In sequence of Second proxy invoked!"/>
                </log>
            </inSequence>
            <outSequence>
                <log level="custom">
                    <property name="SecondProxy" value="Out sequence of Second proxy invoked!"/>
                </log>
                <send/>
            </outSequence>
        </target>
        <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
    <proxy xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteProxy"
           startOnLoad="true">
        <target>
            <endpoint name="ep3">
                <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
            </endpoint>
            <outSequence>
                <log level="custom">
                    <property name="StockQuoteProxy"
                              value="Out sequence of StockQuote proxy invoked!"/>
                </log>
                <log level="full"/>
                <send/>
            </outSequence>
        </target>
        <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
    </proxy>
</definitions>
```

This sample contains three proxy services. The stockquote client invokes
the LocalTransportProxy. Then the message will be sent to the
SecondProxy and then it will be sent to the StockQuoteProxy. The
StockQuoteProxy will invoke the backend service and return the response
to the client. In this sample, the communication between proxy services
are done through the Local transport. Since Local transport calls are
in-JVM calls, it will reduce the time taken for the communication
between proxy services.

1.  Enable [Non Blocking Local
    Transport](https://docs.wso2.com/display/EI650/Local+Transport) for
    the ESB.
2.  Start the Axis2 server and deploy the SimpleStockQuoteService if not
    already done
3.  Execute the stock quote client by requesting for a stock quote on
    the proxy service as follows:

``` java
ant stockquote -Daddurl=http://localhost:8280/services/LocalTransportProxy
```
