# Using POST with Query Parameters
## Example use case

Sending a POST message with query parameters is an unusual scenario, but
the Micro Integrator supports it with no additional configuration. The Micro Integrator forwards the message like any other POST message and includes the query parameters.

## Synapse configuration 

Following is a sample REST Api configuration that we can used to implement this scenario. 

```xml      
<sequence name="fault">
    <log level="full">
       <property name="MESSAGE" value="Executing default sequence"/>
       <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
       <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
    </log>
    <drop/>
 </sequence>
 <sequence name="main">
    <log/>
    <drop/>
 </sequence>
 <api name="testAPI" context="/customerservice">
    <resource methods="POST" url-mapping="/customers">
       <inSequence>
          <send>
             <endpoint>
                <address uri="http://localhost:8282/jaxrs_basic/services/customers/customerservice"/>
             </endpoint>
          </send>
       </inSequence>
       <outSequence>
          <send/>
       </outSequence>
    </resource>
 </api>
```
## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create a REST Api artifact with the above configuration.
4. Create a mediation sequence with the above configuration.
5. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

- Start tcpmon and make it listen to port 8282 of your local machine. It is also important to set the target host name and the port as required. In this case, the target port needs to be set to 8280 (i.e. the port where the backend service is running).  We will now test the connection by sending a POST message that includes a payload inside an HTML body.

Add some query parameters to the URL and execute the following command:

```bash
curl -v -H "Content-Type: text/xml" -d "<Customer><id>123</id><name>John</name></Customer>" 'http://localhost:8280/customerservice/customers?param1=value1&param2=value2'
```

When you execute this command, you can see the following output in tcpmon:

```bash
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

As you can see, the query parameters are present in the REST request, demonstrating that the Micro Integrator sent them along with the message. You could write resource methods to support this type of a request. In this example, the resource method accessed by this request simply ignores the parameters.