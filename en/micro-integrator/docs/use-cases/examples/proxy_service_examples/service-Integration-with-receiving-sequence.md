# Service Integration with specifying the receiving sequence

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <localEntry key="sec_policy" src="file:repository/conf/sample/resources/policy/policy_3.xml"/>
        <proxy name="StockQuoteProxy">
            <target>
                <inSequence>
                    <enrich>
                        <source type="body"/>
                        <target type="property" property="REQUEST"/>
                    </enrich>
    
                    <send receive="SimpleServiceSeq">
                        <endpoint name="secure">
                            <address uri="http://localhost:9000/services/SecureStockQuoteService">
                                <enableSec policy="sec_policy"/>
                            </address>
                        </endpoint>
                    </send>
                </inSequence>
                <outSequence>
                    <drop/>
                </outSequence>
            </target>
        </proxy>
        <sequence name="SimpleServiceSeq">
            <property name="SECURE_SER_AMT" expression="//ns:getQuoteResponse/ns:return/ax21:last"
                    xmlns:ns="http://services.samples" xmlns:ax21="http://services.samples/xsd"/>
            <log level="custom">
                <property name="SecureStockQuoteService-Amount" expression="get-property('SECURE_SER_AMT')"/>
            </log>
            <enrich>
                <source type="body"/>
                <target type="property" property="SecureService_Res"/>
            </enrich>
            <enrich>
                <source type="property" property="REQUEST"/>
                <target type="body"/>
            </enrich>
            <header name="Action" scope="default" value="urn:getQuote"/>
            <send receive="ClientOutSeq">
                <endpoint name="SimpleStockQuoteService">
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </sequence>
        <sequence name="ClientOutSeq">
            <property name="SIMPLE_SER_AMT" expression="//ns:getQuoteResponse/ns:return/ax21:last"
                           xmlns:ns="http://services.samples" xmlns:ax21="http://services.samples/xsd"/>
            <log level="custom">
                <property name="SimpleStockQuoteService-Amount" expression="get-property('SIMPLE_SER_AMT')"/>
            </log>
            <enrich>
                <source type="body"/>
                <target type="property" property="SimpleService_Res"/>
            </enrich>
            <filter xpath="fn:number(get-property('SIMPLE_SER_AMT')) > fn:number(get-property('SECURE_SER_AMT'))">
                <then>
                    <log>
                        <property name="StockQuote" value="SecureStockQuoteService"/>
                    </log>
                    <enrich>
                        <source type="property" property="SecureService_Res"/>
                        <target type="body"/>
                    </enrich>
                </then>
                <else>
                    <log>
                        <property name="StockQuote" value="SimpleStockQuoteService"/>
                    </log>
                </else>
            </filter>
            <send/>
        </sequence>
    </definitions>
```

**Prerequisites:**  
We will be using two services in this sample; i.e.
`         SimpleStockQuoteService        ` and
`         SecureStockQuoteService        ` . Therefore the prerequisites
of sample 100 is also applied here.

-   Start the Axis2 server and deploy the SimpleStockQuoteService if not
    already done.

This sample contains a proxy service which provides the seamless
integration of `         SimpleStockQuoteService        ` and
`         SecureStockQuoteService        ` . Once a client send a
request to this proxy service, it sends the same request to both these
services and resolve the service with cheapest stock quote price.

Execute the stock quote client by requesting for a cheapest stock quote
from the proxy service as follows:

``` bash
    ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy
```

Above sample uses the concept of specifying the receiving sequence in
the send mediator. In this case once the message is sent from the in
sequence then the response won't come to out sequence as the receiving
sequence is specified in the send mediator.
