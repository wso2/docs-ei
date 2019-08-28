# Local Transport Example

There are three proxy services in WSO2 EI named
`         LocalTransportProxy        ` , `         SecondProxy        `
and `         StockQuoteProxy        ` . The invocation of services take
place in the following order:

1.  1.  The stockquote client invokes
        `            LocalTransportProxy           ` .
    2.  The message received is forwarded to
        `            SecondProxy           ` .
    3.  The message is forwarded to
        `            StockQuoteProxy           ` .
    4.  `            StockQuoteProxy           ` invokes the backend
        service.
    5.  `            StockQuoteProxy           ` receives a response.
    6.  The response is returned by
        `            StockQuoteProxy           ` to
        `            SecondProxy           ` .
    7.  The response is returned by `            SecondProxy           `
        to `            LocalTransportProxy           ` .
    8.  The response is returned by
        `            LocalTransportProxy           ` to the client.

When local transport is not used, the messages sent by one proxy service
to another (i.e. flows b, c, f and g) goes through the network as
shown in the following diagram.

![](attachments/119130361/119130363.png)

Local transport can be used as shown below to prevent message flows
between proxy services from going through the network. When local
transport calls are in JVM calls, the time taken for communication
between proxy services will be reduced since no network overhead will be
introduced.

![](attachments/119130361/119130362.png)

To use this transport, configure an endpoint with the `local://` prefix. For example, to make an in-VM call to the HelloService, use `local://services/HelloService`. Note that the local transport cannot be used to send REST API calls, which require the HTTP/S transports.