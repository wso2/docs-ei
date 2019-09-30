# Conditional Router for Routing Messages based on HTTP URL, HTTP Headers and Query Parameters

!!! Note
    Please note that Conditional Router Mediator is deprecated.

Routing Messages based on the HTTP Transport properties using the [Conditional Router mediator](https://docs.wso2.com/display/EI650/Conditional+Router+Mediator).

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="StockQuoteProxy" transports="https http" startOnLoad="true" trace="disable">
            <target>
                <inSequence>
                    <conditionalRouter continueAfter="false">
                        <conditionalRoute breakRoute="false">
                            <condition>
                                <match xmlns="" type="header" source="foo" regex="bar.*"/>
                            </condition>
                            <target sequence="cnd1_seq"/>
                        </conditionalRoute>
                        <conditionalRoute breakRoute="false">
                            <condition>
                                <and xmlns="">
                                    <match type="header" source="my_custom_header1" regex="foo.*"/>
                                    <match type="url" regex="/services/StockQuoteProxy.*"/>
                                </and>
                            </condition>
                            <target sequence="cnd2_seq"/>
                        </conditionalRoute>
                        <conditionalRoute breakRoute="false">
                            <condition>
                                <and xmlns="">
                                    <match type="header" source="my_custom_header2" regex="bar.*"/>
                                    <equal type="param" source="qparam1" value="qpv_foo"/>
                                    <or>
                                        <match type="url" regex="/services/StockQuoteProxy.*"/>
                                        <match type="header" source="my_custom_header3" regex="foo.*"/>
                                    </or>
                                    <not>
                                        <equal type="param" source="qparam2" value="qpv_bar"/>
                                    </not>
                                </and>
                            </condition>
                            <target sequence="cnd3_seq"/>
                        </conditionalRoute>
                    </conditionalRouter>
                </inSequence>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
        </proxy>
        <sequence name="cnd1_seq">
            <log level="custom">
                <property name="MSG_FLOW" value="Condition (I) Satisfied"/>
            </log>
            <sequence key="send_seq"/>
        </sequence>
        <sequence name="cnd2_seq">
            <log level="custom">
                <property name="MSG_FLOW" value="Condition (II) Satisfied"/>
            </log>
            <sequence key="send_seq"/>
        </sequence>
        <sequence name="cnd3_seq">
            <log level="custom">
                <property name="MSG_FLOW" value="Condition (III) Satisfied"/>
            </log>
            <sequence key="send_seq"/>
        </sequence>
        <sequence name="send_seq">
            <log level="custom">
                <property name="DEBUG" value="Condition Satisfied"/>
            </log>
            <send>
                <endpoint name="simple">
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
            </send>
        </sequence>
    </definitions>
```

**Prerequisites:**

-   Start the Axis2 server and deploy the SimpleStockQuoteService if not
    already done. For this particular case we will be using 'curl' to
    send requests with custom HTTP Headers to the proxy service. You may
    use a similar tool with facilitate those requirements.

  
The request file `         stockQuoteReq.xml        ` should contain the
following request.

```
    <soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
       <soap:Header/>
       <soap:Body>
          <ser:getQuote>
             <ser:request>
                <xsd:symbol>IBM</xsd:symbol>
             </ser:request>
          </ser:getQuote>
       </soap:Body>
    </soap:Envelope>
```

Condition I : Matching HTTP Header

``` bash
    curl -d @stockQuoteReq.xml -H "Content-Type: application/soap+xml;charset=UTF-8" -H "foo:bar" "http://localhost:8280/services/StockQuoteProxy"
```

Condition II : Matching HTTP Header AND Url

``` bash
    curl -d @stockQuoteReq.xml -H "Content-Type: application/soap+xml;charset=UTF-8" -H "my_custom_header1:foo1" "http://localhost:8280/services/StockQuoteProxy"
```

Condition III :  
Complex conditions with AND, OR and NOT

``` bash
    curl -d @stockQuoteReq.xml -H "Content-Type: application/soap+xml;charset=UTF-8" -H "my_custom_header2:bar" -H "my_custom_header3:foo" "http://localhost:8280/services/StockQuoteProxy?qparam1=qpv_foo&qparam2=qpv_foo2"
```
