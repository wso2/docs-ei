# Content negotiation

Content negotiation is a mechanism by which a client and a server
communicate with each other and decide on a content format to use for
data transfer. In our samples so far, we used XML (application/xml) as
the primary means of data transfer. However, we could use other content
formats such as plain text and JSON to achieve the same result.

Real-world client applications usually have preferred content types. For
example:

-   A web browser usually prefers HTML.
-   A Java-based desktop application usually prefers plain-old XML (POX).
-   A mobile application usually prefers JSON.

To maintain interoperability, the server-side applications should be prepared to serve content using any of these formats. Using content negotiation, the client can indicate its content type preferences to the server, and the server can serve the requests using a content type preferred by the client.

The HTTP specification provides the basic elements to build a powerful content negotiation framework. The HTTP client can indicate its content type preferences by sending the Accept header along with the requests. First, we need to retrieve the value of the Accept header sent by the client. We do this in the in-sequence using the `         $trp        ` XPath variable, as follows:

Now in the out-sequence we can run a few comparisons to see which
content type to use for sending the response. We use the
`         $ctx        ` XPath variable to specify the

```
<case regex=".*atom.*">
    <send/>
</case>
<case regex=".*text/html.*">
    <send/>
</case>
<case regex=".*json.*">
    <send/>
</case>
<case regex=".*application/xml.*">
    <send/>
</case>
<default>
    <send/>
</default>
</switch>
```
    
The above configuration results in the following priority order of content types.
    
1.  Atom (Also used as default)
2.  HTML
3.  JSON
4.  POX
    
To try this out, invoke the following Curl commands and see how the response format changes according to the value of the Accept header.

```
curl -v http://localhost:8280/orders
curl -v -H "Accept: application/xml" http://localhost:8280/orders
curl -v -H "Accept: application/json" http://localhost:8280/orders
curl -v -H "Accept: text/html" http://localhost:8280/orders
```