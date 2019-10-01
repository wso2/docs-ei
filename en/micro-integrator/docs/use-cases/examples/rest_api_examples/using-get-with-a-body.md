# Using GET with a Body
## Example use case

Typically, a GET request does not contain a body, and the Micro Integrator does not support these types of requests. When it receives a GET
request that contains a body, it drops the message body as it sends the
message to the endpoint. 

## Synapse configuration

Following is a sample REST Api configuration and mediation Sequence that we can used to implement this scenario.

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

........

Send an invalid request to the back end as follows:
    
```bash
curl -v -H "Content-Type: text/xml" -d "<Customer><id>123</id><name>John</name></Customer>" 'http://localhost:8280/jaxrs_basic/services/customers/customerservice/customers/123' -X GET
```

The additional parameter `-X` replaces the original POST method with the specified method, which in this case is GET. This will cause the client to send a GET request with a message similar to a POST request. If you view the output in tcpmon, you will see that there is no message body in the request.