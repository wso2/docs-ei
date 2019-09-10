# Sample 54: Session Affinity Load Balancing between Three Endpoints

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


-   [Introduction](#Sample54:SessionAffinityLoadBalancingbetweenThreeEndpoints-Introduction)
-   [Prerequisites](#Sample54:SessionAffinityLoadBalancingbetweenThreeEndpoints-Prerequisites)
-   [Building the
    sample](#Sample54:SessionAffinityLoadBalancingbetweenThreeEndpoints-Buildingthesample)
-   [Executing the
    sample](#Sample54:SessionAffinityLoadBalancingbetweenThreeEndpoints-Executingthesample)
-   [Analyzing the
    output](#Sample54:SessionAffinityLoadBalancingbetweenThreeEndpoints-Analyzingtheoutput)

### Introduction

This sample demonstrates how the ESB can handle load balancing with
session affinity using simple client sessions . The configuration used
here is the same as the configuration in sample 52, except that here the
session type is specified as `         simpleClientSession        ` .
This is a client initiated session, which means that the client
generates the session identifier and sends it with each request. In this
sample session type, the client adds a SOAP header named ClientID
containing the identifier of the client. The ESB binds this ID with a
server on the first request and sends all seccessive requests containing
that ID to the same server.

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
                </send><drop/>
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
    
            <header name="To" action="remove"/>
            <property name="RESPONSE" value="true"/>
            <send/>
        </sequence>
    </definitions>
```

This configuration file `         synapse_sample_54.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start three instances of the sample Axis2 server on HTTP ports 9001,
    9002 and 9003 and give unique names to each server. For instructions
    on starting the Axis2 server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

2.  Deploy the back-end service
    `           LoadbalanceFailoverService          ` . For instructions
    on deploying sample back-end services, see [Deploying sample
    back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .

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

  
