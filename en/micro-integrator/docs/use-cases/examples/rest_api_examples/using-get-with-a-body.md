# Using GET with a Body

Typically, a GET request does not contain a body, and the ESB
profile does not support these types of requests. When it receives a GET
request that contains a body, it drops the message body as it sends the
message to the endpoint. You can test this scenario using the same setup
as above, but this time the client command should look like this:

`curl -v -H "Content-Type: text/xml" -d "<Customer><id>123</id><name>John</name></Customer>" 'http://localhost:8280/jaxrs_basic/services/customers/customerservice/customers/123' -X GET`

The additional parameter `         -X        ` replaces the original
POST method with the specified method, which in this case is GET. This
will cause the client to send a GET request with a message similar to a
POST request. If you view the output in tcpmon, you will see that there
is no message body in the request.