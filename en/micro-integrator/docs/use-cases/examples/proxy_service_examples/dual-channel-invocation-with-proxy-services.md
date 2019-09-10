# Dual Channel Invocation on Both Client Side and Server Side of Synapse with Proxy Services

Demonstrate the dual channel invocation with Synapse proxy
services.

```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="StockQuoteProxy">
            <target>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService">
                        <enableAddressing separateListener="true"/>
                    </address>
                </endpoint>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
    </definitions>
```

## Prerequisites

-   Start the Axis2 server and deploy the
    `          SimpleStockQuoteService         ` if not already done.

This sample shows the action of the dual channel invocation within
client and Synapse as well as within synapse and the actual server.

!!! Info
    If you want to enable dual channel invocation, you need to set the `         separateListener        ` attribute to true of the `         enableAddressing        ` element of the endpoint.

Execute the stock quote client by requesting for a stock quote on a dual
channel from the Proxy Service as follows:

``` java
    ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy -Dmode=dualquote
```

In the above example, the request received is forwarded to the sample
service hosted on Axis2 and the endpoint specifies to enable addressing
and do the invocation over a dual channel. If you observe this message
flow by using a TCPmon, you could see that on the channel you send the
request to synapse the response has been written as an HTTP 202
Accepted, where as the real response from synapse will come over a
different channel which cannot be observed unless you use tcpdump to
dump all the TCP level messages.

At the same time you can observe the behavior of the invocation between
synapse and the actual Axis2 service, where you can see a 202 Accepted
message being delivered to synapse as the response to the request. The
actual response will be delivered to synapse over a different channel.
