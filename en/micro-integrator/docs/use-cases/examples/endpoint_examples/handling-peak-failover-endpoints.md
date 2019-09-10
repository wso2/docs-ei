# Sample 53: Using Failover Endpoints to Handle Peak Loads

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


-   [Introduction](#Sample53:UsingFailoverEndpointstoHandlePeakLoads-Introduction)
-   [Prerequisites](#Sample53:UsingFailoverEndpointstoHandlePeakLoads-Prerequisites)
-   [Building the
    sample](#Sample53:UsingFailoverEndpointstoHandlePeakLoads-Buildingthesample)
-   [Executing the
    sample](#Sample53:UsingFailoverEndpointstoHandlePeakLoads-Executingthesample)
-   [Analyzing the
    output](#Sample53:UsingFailoverEndpointstoHandlePeakLoads-Analyzingtheoutput)

### Introduction

This sample demonstrates how you can handle peak load scenarios using
failover endpoints. Here three failover endpoints are used in order to
handle the requests that are coming in.

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
                        <failover>
                            <endpoint>
                                <address uri="http://localhost:9001/services/LBService1">
                                    <enableAddressing/>
                                    <suspendOnFailure>
                                      <initialDuration>60000</initialDuration>
                                      <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                            </endpoint>
                            <endpoint>
                                <address uri="http://localhost:9002/services/LBService1">
                                    <enableAddressing/>
                                    <suspendOnFailure>
                                      <initialDuration>60000</initialDuration>
                                      <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                            </endpoint>
                            <endpoint>
                                <address uri="http://localhost:9003/services/LBService1">
                                    <enableAddressing/>
                                    <suspendOnFailure>
                                      <initialDuration>60000</initialDuration>
                                      <progressionFactor>1.0</progressionFactor>
                                    </suspendOnFailure>
                                </address>
                            </endpoint>
                        </failover>
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

This configuration file `         synapse_sample_53.xml        ` is
available in the `         <ESB_HOME>/repository/samples        `
directory.

**To build the sample**

1.  Start three instances of the sample Axis2 server on HTTP ports 9001,
    9002 and 9003 respectively and give unique names to each server. For
    instructions on starting the Axis2 server, see [Starting the Axis2
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
    `           <ESB_HOME>/samples/axis2Client          ` directory,
    to send infinite requests.

    ``` bash
            ant loadbalancefailover
    ```

2.  While the client is running, stop the server named MyServer1.
3.  Stop the server named MyServer2.
4.  Stop the server named MyServer3.

### Analyzing the output

According to the configuration file for this sample, messages are sent
with the failover behavior. Initially the server at port 9001 is treated
as the primary server and the other two are treated as back up servers.
Messages will always be directed only to the primary server. If the
primary server fails, the next listed server is selected as the primary
server. Therefore, messages are sent successfully as long as there is at
least one active server. To test this, run the loadbalancefailover
client to send infinite requests as follows:

``` bash
    ant loadbalancefailover
```

When you run the client, infinite requests will be sent and you will see
that all requests are processed by MyServer1. This is because the
requests are always directed only to the primary server.

Here, the server at port 9001 is treated as the primary server and the
other two are treated as back up servers. Therefore, all requests are
processed by the server at port 9001 which is named as MyServer1.

Once you stop MyServer1 and analyze the log output on the client
console, you will see that all subsequent requests are processed by
MyServer2.

If MyServer1 is stopped after the request 127, the log output will be as
follows:

``` java
    ...
    [java] Request: 125 ==> Response from server: MyServer1
    [java] Request: 126 ==> Response from server: MyServer1
    [java] Request: 127 ==> Response from server: MyServer1
    [java] Request: 128 ==> Response from server: MyServer2
    [java] Request: 129 ==> Response from server: MyServer2
    [java] Request: 130 ==> Response from server: MyServer2
    ...
```

When all servers are stopped, the error sequence will be activated and
you will see the following fault message on the client console.

``` java
    [java] COULDN'T SEND THE MESSAGE TO THE SERVER.
```

When a server failure is detected, the failed server will be added once
again to the active servers list after 60 seconds. This is because
`         <suspendDurationOnFailure>        ` is set to 60 in the
configuration file. Therefore, if you have restarted any of the stopped
servers and have stopped all other servers, messages will be directed to
the server that is restarted.

  
