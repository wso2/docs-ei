# Load Balancing with Proxy Services

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="LBProxy" transports="https http" startOnLoad="true">
            <target faultSequence="errorHandler">
                <inSequence>
                    <send>
                        <endpoint>
                            <session type="simpleClientSession"/>
                            <loadbalance algorithm="org.apache.synapse.endpoints.algorithms.RoundRobin">
                                <endpoint>
                                    <address uri="http://localhost:9001/services/LBService1">
                                        <enableAddressing/>
                                        <suspendOnFailure>
                                            <initialDuration>20000</initialDuration> 
                                            <progressionFactor>1.0</progressionFactor>
                                        </suspendOnFailure>
                                    </address>
                                </endpoint>
                                <endpoint>
                                    <address uri="http://localhost:9002/services/LBService1">
                                        <enableAddressing/>
                                        <suspendOnFailure>
                                            <initialDuration>20000</initialDuration> 
                                            <progressionFactor>1.0</progressionFactor>
                                        </suspendOnFailure>
                                    </address>
                                </endpoint>
                                <endpoint>
                                    <address uri="http://localhost:9003/services/LBService1">
                                        <enableAddressing/>
                                        <suspendOnFailure>
                                            <initialDuration>20000</initialDuration> 
                                            <progressionFactor>1.0</progressionFactor>
                                        </suspendOnFailure>
                                    </address>
                                </endpoint>
                            </loadbalance>
                        </endpoint>
                    </send>
                </inSequence>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_2.wsdl"/>
        </proxy>
        <sequence name="errorHandler">
            <makefault>
                <code value="tns:Receiver" xmlns:tns="http://www.w3.org/2003/05/soap-envelope"/>
                <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER."/>
            </makefault>
            <header name="To" action="remove"/>
            <property name="RESPONSE" value="true"/>
            <send/>
        </sequence>
    </definitions>
```

## Prerequisites

-   Start three instances of Axis2 server (on ports 9001, 9002, and
    9003), and deploy the `          LBService         ` to each of
    them. For more information, see [Setting Up the ESB
    Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
    .

Run the client:

``` java
    ant loadbalancefailover -Dmode=session -Dtrpurl=http://localhost:8280/services/LBProxy
```
