# Routing a Message to a Static List of Recipients
## Example use case

Demonstrate message routing to a set of static
endpoints.

## Synapse configuration

This configuration routes a cloned copy of a message to each recipient defined within the static recipient list. The Micro Integrator will create cloned copies of the message and route to the three endpoints mentioned in the configuration. SimpleStockQuoteService prints the details of the placed order. 

```xml tab='Main Sequence'
<sequence name="main" onError="errorHandler">
    <in>
        <send>
            <endpoint>
                <!--List of Recipients (static)-->
                <recipientlist>
                    <endpoint>
                        <address uri="http://localhost:9001/services/SimpleStockQuoteService"/>
                    </endpoint>
                    <endpoint>
                        <address uri="http://localhost:9002/services/SimpleStockQuoteService"/>
                    </endpoint>
                    <endpoint>
                        <address uri="http://localhost:9003/services/SimpleStockQuoteService"/>
                    </endpoint>
                </recipientlist>
            </endpoint>
        </send>
        <drop/>
    </in>
    <out>
        <log level="full"/>
        <drop/>
    </out>
</sequence>
```

```xml tab='Error Handling Sequence'
<sequence name="errorHandler">
    <makefault response="true">
        <code xmlns:tns="http://www.w3.org/2003/05/soap-envelope" value="tns:Receiver"/>
        <reason value="COULDN'T SEND THE MESSAGE TO THE SERVER."/>
    </makefault>
    <send/>
</sequence>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the integration artifacts shown above.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service.

Invoke the Micro Integrator:

To test this, run
the StockQuote client to send an out-only message as follows:

```bash
ant stockquote -Dmode=placeorder -Dtrpurl=http://localhost:8280/
```

If you examine the console output of
each server, you can see that requests are processed by the three
servers as follows:

```bash
Accepted order #1 for : 15738 stocks of IBM at $ 185.51155223506518
```

Now shutdown MyServer1 and resend the request. You will observe that requests are still processed by MyServer2 and MyServer3.