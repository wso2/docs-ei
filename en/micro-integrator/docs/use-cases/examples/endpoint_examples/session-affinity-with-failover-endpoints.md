# Sample 55: Session Affinity Load Balancing between Failover Endpoints

!!! warning

Note that WSO2 EI is shipped with the following changes to what is
mentioned in this documentation :

-   `           <PRODUCT_HOME>/          `
    `           repository/samples/          ` directory that includes
    all Integration profile samples is changed to
    `           <EI_HOME>/          `
    `           samples/service-bus/          ` .
    `                     `
-   `           <PRODUCT_HOME>/          `
    `           repository/samples/resources/          ` directory that
    includes all artifacts related to the Integration profile samples is
    changed to `           <EI_HOME>/          `
    `           samples/service-bus/resources/          ` .


-   [Introduction](#Sample55:SessionAffinityLoadBalancingbetweenFailoverEndpoints-Introduction)
-   [Prerequisites](#Sample55:SessionAffinityLoadBalancingbetweenFailoverEndpoints-Prerequisites)
-   [Building the
    sample](#Sample55:SessionAffinityLoadBalancingbetweenFailoverEndpoints-Buildingthesample)
-   [Executing the
    sample](#Sample55:SessionAffinityLoadBalancingbetweenFailoverEndpoints-Executingthesample)
-   [Analyzing the
    output](#Sample55:SessionAffinityLoadBalancingbetweenFailoverEndpoints-Analyzingtheoutput)

### Introduction

This sample demonstrates how the ESB can handle session aware load
balancing between failover endpoints.

The configuration in this sample also uses the session type
`         simpleClientSession        ` to bind session ID values to
servers as in sample 54. The difference in this sample is that fail-over
endpoints are specified as child endpoints of the load balance endpoint.
Therefore, sessions are bound to the fail-over endpoints. The session
specific data maintained in endpoint servers are replicated among the
failover endpoints using a clustering mechanism. Therefore, if one
endpoint bound to a session fails, successive requests for that session
will be directed to the next endpoint in that failover group.

### Prerequisites

For a list of prerequisites, see [Prerequisites to Start the ESB
Samples](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-ESBSamplePrerequisites)
.

### Building the sample

The XML configuration for this sample is as follows:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <sequence name="main" onError="errorHandler">
            <in>
                <send>
                    <endpoint>
                        <!-- specify the session as the simple client session provided by Synapse for
                        testing purpose -->
                        <session type="simpleClientSession"/>
                        <loadbalance>
                            <endpoint>
                                <failover>
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
                                </failover>
                            </endpoint>
                            <endpoint>
                                <failover>
                                    <endpoint>
                                        <address uri="http://localhost:9003/services/LBService1">
                                            <enableAddressing/>
                                        </address>
                                    </endpoint>
                                    <endpoint>
                                        <address uri="http://localhost:9004/services/LBService1">
                                            <enableAddressing/>
                                        </address>
                                    </endpoint>
                                </failover>
                            </endpoint>
                        </loadbalance>
                    </endpoint>
                </send>
                <drop/>
            </in>
            <out>
                <!-- Send the messages where they have been sent (i.e. implicit To EPR) -->
                <send/>
            </out>
        </sequence>
        <sequence name="errorHandler">
            <makefault>
                <code value="tns:Receiver" xmlns:tns="http://www.w3.org/2003/05/soap-envelope"/>
                <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER."/>
            </makefault>
            <send/>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_55.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start four instances of the sample Axis2 server on HTTP ports 9001,
    9002, 9003 and 9004 respectively and give unique names to each
    server. For instructions on starting the Axis2 server, see [Starting
    the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service **LoadbalanceFailoverService** . For
    instructions on deploying sample back-end services, see [Deploying
    sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

### Executing the sample

The sample client used here is the **Load Balance and Failover Client**
.

**To execute the sample client**

1.  Run the following command from the
    `           <ESB_HOME>/samples/axis2Client          ` directory.

    ``` bash
            ant loadbalancefailover -Dmode=session
    ```

2.  While the client is running, stop the server named MyServer1.

### Analyzing the output

When the client is run, you will see the following output on the client
console:

``` java
    ...
    [java] Request: 222 Session number: 0 Response from server: MyServer1
    [java] Request: 223 Session number: 0 Response from server: MyServer1
    [java] Request: 224 Session number: 1 Response from server: MyServer1
    [java] Request: 225 Session number: 2 Response from server: MyServer3
    [java] Request: 226 Session number: 0 Response from server: MyServer1
    [java] Request: 227 Session number: 1 Response from server: MyServer1
    [java] Request: 228 Session number: 2 Response from server: MyServer3
    [java] Request: 229 Session number: 1 Response from server: MyServer1
    [java] Request: 230 Session number: 1 Response from server: MyServer1
    [java] Request: 231 Session number: 2 Response from server: MyServer3
    ...
```

By analysing the above output, y ou will see that the session 0 and
session 1 are always directed to MyServer1 whereas session 2 is always
directed to MyServer3. You will notice that none of the requests are
directed to MyServer2 and MyServer4. This is because MyServer2 and
MyServer4 are kept as backup servers by the failover endpoints.

When MyServer1 is stopped while running the sample, you will see that
all successive requests for session 0 are directed to MyServer2, which
is the backup server for MyServer1's failover group.

The output on the client console will be as follows:

``` java
    ...
    [java] Request: 529 Session number: 2 Response from server: MyServer3
    [java] Request: 530 Session number: 1 Response from server: MyServer1
    [java] Request: 531 Session number: 0 Response from server: MyServer1
    [java] Request: 532 Session number: 1 Response from server: MyServer1
    [java] Request: 533 Session number: 1 Response from server: MyServer1
    [java] Request: 534 Session number: 1 Response from server: MyServer1
    [java] Request: 535 Session number: 0 Response from server: MyServer2
    [java] Request: 536 Session number: 0 Response from server: MyServer2
    [java] Request: 537 Session number: 0 Response from server: MyServer2
    [java] Request: 538 Session number: 2 Response from server: MyServer3
    [java] Request: 539 Session number: 0 Response from server: MyServer2
    ...
```

By analysing the above output, y ou will see that all requests for
session 0 are directed to MyServer2 after the request 534. Therefore,
you can come to the conclusion that MyServer1 was stopped after request
534.

  

  

  

  
