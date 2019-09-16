# Using POST with Query Parameters

Sending a POST message with query parameters is an unusual scenario, but
the ESB supports it with no additional configuration. The ESB
profile forwards the message like any other POST message and includes
the query parameters.

To test this scenario, use the same setup as above, but instead of
removing the data part from the request, add some query parameters to
the URL as follows:

`curl -v -H "Content-Type: text/xml" -d "<Customer><id>123</id><name>John</name></Customer>" 'http://localhost:8280/customerservice/customers?param1=value1&param2=value2'`

When you execute this command, you can see the following output in tcpmon:

```
POST /jaxrs_basic/services/customers/customerservice/customers?param1=value1&param2=value2 HTTP/1.1
    Content-Type: application/xml; charset=UTF-8
    Accept: */*
    Transfer-Encoding: chunked
    Host: 127.0.0.1:8282
    Connection: Keep-Alive
     
    32
    <Customer><id>123</id><name>John</name></Customer>
    0
```

As you can see, the query parameters are present in the REST request, demonstrating that the ESB profile sent them along with the message. You could write resource methods to support this type of a request. In this example, the resource method accessed by this request simply ignores the parameters.