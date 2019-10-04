# Using GET with a Body
## Example use case

Typically, a GET request does not contain a body, and the Micro Integrator does not support these types of requests. When it receives a GET
request that contains a body, it drops the message body as it sends the
message to the endpoint. 

## Synapse configuration

Following is a sample REST Api configuration and mediation Sequence that we can used to implement this scenario.

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="HealthcareService" context="/healthcare">
  <resource methods="GET POST" url-mapping="/appointment/reserve">
     <inSequence>
        <property name="REST_URL_POSTFIX" scope="axis2" action="remove"/>
        <send>
            <endpoint>
                <address uri="http://localhost:9090/grandoaks/categories/surgery/reserve"/>
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
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Send an invalid request to the back end as follows:
    
```bash
curl -v -H "Content-Type: application/json" -d @request.json http://localhost:8290/healthcare/appointment/reserve -X GET
```
where `        request.json        ` has the following content on the appointment:
    
```
{
    "patient": {
    "name": "John Doe",
    "dob": "1940-03-19",
    "ssn": "234-23-525",
    "address": "California",
    "phone": "8770586755",
    "email": "johndoe@gmail.com"
    },
    "doctor": "thomas collins",
    "hospital": "grand oak community hospital",
    "appointment_date": "2025-04-02"
}
```

The additional parameter `-X` replaces the original POST method with the specified method, which in this case is GET. This will cause the client to send a GET request with a message similar to a POST request. If you view the output in tcpmon, you will see that there is no message body in the request.