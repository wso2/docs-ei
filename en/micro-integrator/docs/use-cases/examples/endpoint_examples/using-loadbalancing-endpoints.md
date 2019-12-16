# Using the Load Balance Endpoint

**Session Affinity Load Balancing between Three Endpoints**

This sample demonstrates how the Micro Integrator can handle load balancing with
session affinity using simple client sessions. Here the
session type is specified as `         simpleClientSession        ` .
This is a client initiated session, which means that the client
generates the session identifier and sends it with each request. In this
sample session type, the client adds a SOAP header named ClientID
containing the identifier of the client. The MI binds this ID with a
server on the first request and sends all successive requests containing
that ID to the same server.

## Synapse configuration

Following is a sample REST API configuration that we can used to implement this scenario.

```xml 
<proxy name="LoadBalanceProxy" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
   <target>
       <inSequence>
            <header name="Action" value="urn:placeOrder"/>
            <call>
                <endpoint>
                    <!-- specify the session as the simple client session provided by Synapse for
                    testing purpose -->
                    <session type="simpleClientSession"/>

                    <loadbalance>
                        <endpoint>
                            <address uri="http://localhost:9001/services/LBService1">
                                <enableAddressing/>
                            </address>
                        </endpoint>
                        <endpoint>
                            <address uri="http://localhost:9002/services/LBService1">
                                <enableAddressing/>
                            </address>
                        </endpoint>
                        <endpoint>
                            <address uri="http://localhost:9003/services/LBService1">
                                <enableAddressing/>
                            </address>
                        </endpoint>
                    </loadbalance>
                </endpoint>
            </call>
            <respond/>
       </inSequence>
       <outSequence>
            <send/>
       </outSequence>
       <faultSequence>
            <sequence key="errorHandler"/>
       </faultSequence
   </target>
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
```

<!--
**To build the sample**

### Executing the sample

The sample client used here is the **Load Balance and Failover Client**
.

**To execute the sample client**

-   Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant loadbalancefailover -Dmode=session
    ```

### Analyzing the output

When the client is run in the session mode, the client continuously
sends requests with three different session IDs. Out of these three IDs
one ID is selected randomly for each request. Then the client prints the
session ID with the server that responds for each request.

When you analyze the output on the client console, you will see the
client output for the first 10 requests, which will be as follows:

``` java
    [java] Request: 1 Session number: 1 Response from server: MyServer3
    [java] Request: 2 Session number: 2 Response from server: MyServer2
    [java] Request: 3 Session number: 0 Response from server: MyServer1
    [java] Request: 4 Session number: 2 Response from server: MyServer2
    [java] Request: 5 Session number: 1 Response from server: MyServer3
    [java] Request: 6 Session number: 2 Response from server: MyServer2
    [java] Request: 7 Session number: 2 Response from server: MyServer2
    [java] Request: 8 Session number: 1 Response from server: MyServer3
    [java] Request: 9 Session number: 0 Response from server: MyServer1
    [java] Request: 10 Session number: 0 Response from server: MyServer1
    ... 
```

By analysing the above output, y ou will see that the session number 0
is always directed to the server named MyServer1. This means that the
session number 0 is bound to MyServer1. Similarly, session 1 s always
directed to MyServer3 and session 2 is always directed to MyServer2.
This means that session 1 and 2 are bound to MyServer3 and MyServer2
respectively.

-->
