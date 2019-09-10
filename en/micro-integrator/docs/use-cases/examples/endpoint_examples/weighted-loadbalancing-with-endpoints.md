# Weighted load balancing between 3 endpoints

Demonstrate the weighted load balancing among a set of
endpoints.

``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <sequence name="main" onError="errorHandler">
            <in>
                <send>
                    <endpoint>
                        <loadbalance
                                algorithm="org.apache.synapse.endpoints.algorithms.WeightedRoundRobin">
                            <endpoint>
                                <address uri="http://localhost:9001/services/LBService1">
                                    <enableAddressing/>
                                    <suspendOnFailure>
                                        <initialDuration>20000</initialDuration>
                                        <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                                <property name="loadbalance.weight" value="1"/>
                            </endpoint>
                            <endpoint>
                                <address uri="http://localhost:9002/services/LBService1">
                                    <enableAddressing/>
                                    <suspendOnFailure>
                                        <initialDuration>20000</initialDuration>
                                        <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                                <property name="loadbalance.weight" value="2"/>
                            </endpoint>
                            <endpoint>
                                <address uri="http://localhost:9003/services/LBService1">
                                    <enableAddressing/>
                                    <suspendOnFailure>
                                        <initialDuration>20000</initialDuration>
                                        <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                                <property name="loadbalance.weight" value="3"/>
                            </endpoint>
                        </loadbalance>
                    </endpoint>
                </send>
                <drop/>
            </in>
            <out>
                <send/>
            </out>
        </sequence>
        <sequence name="errorHandler">
            <makefault response="true">
                <code xmlns:tns="http://www.w3.org/2003/05/soap-envelope" value="tns:Receiver"/>
                <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER."/>
            </makefault>
            <send/>
        </sequence>
    </definitions>
```

**Prerequisites:**

-   Deploy the `          LoadbalanceFailoverService         ` and start
    three instances of sample Axis2 server as mentioned in sample 52.

Above configuration sends messages with the weighted loadbalance
behaviour. Weight of each leaf address endpoint is defined by integer
value of "loadbalance.weight" property associated with each endpoint. If
weight of a endpoint is x, x number of requests will send to that
endpoint before switch to next active endpoint.  
To test this, run the loadbalancefailover client to send 100 requests as
follows:

``` bash
    ant loadbalancefailover -Di=100
```

This client sends 100 requests to the LoadbalanceFailoverService through
ESB. ESB will distribute the load among the three endpoints mentioned in
the configuration in weighted round-robin manner.
LoadbalanceFailoverService appends the name of the server to the
response, so that client can determine which server has processed the
message. If you examine the console output of the client, you can see
that requests are processed by three servers as follows:

``` java
    [java] Request: 1 ==> Response from server: MyServer1
    [java] Request: 2 ==> Response from server: MyServer2
    [java] Request: 3 ==> Response from server: MyServer2
    [java] Request: 4 ==> Response from server: MyServer3
    [java] Request: 5 ==> Response from server: MyServer3
    [java] Request: 6 ==> Response from server: MyServer3
    [java] Request: 7 ==> Response from server: MyServer1
    [java] Request: 8 ==> Response from server: MyServer2
    [java] Request: 9 ==> Response from server: MyServer2
    [java] Request: 10 ==> Response from server: MyServer3
    [java] Request: 11 ==> Response from server: MyServer3
    [java] Request: 12 ==> Response from server: MyServer3
    ...
```

As logs you see something similar to
`         endpoint with weight 1 received 1 request and endpoint with weight 2 received 2 requests,        `
etc. in a cycle.
