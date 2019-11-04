# Using POST with Query Parameters
Sending a POST message with query parameters is an unusual scenario, but the Micro Integrator supports it with no additional configuration. The Micro Integrator forwards the message like any other POST message and includes the query parameters.

## Synapse configuration 

Following is a sample REST Api configuration that we can used to implement this scenario. 

```xml      
<api xmlns="http://ws.apache.org/ns/synapse" name="HealthcareService" context="/healthcare">
  <resource methods="GET POST" url-mapping="/reserve">
     <inSequence>
        <send>
            <endpoint>
                <address uri="http://localhost:9090/grandoaks/categories/surgery"/>
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

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. [Create the rest api](../../../../develop/creating-artifacts/creating-an-api) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service:

1. Download the [Hospital-Service-2.0.0-EI7.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0-EI7.jar)
2. Open a terminal, navigate to the location of the downloaded service, and run it (hospital service) using the following command:

    ```bash
    java -jar Hospital-Service-2.0.0-EI7.jar
    ```
    
3. Start tcpmon and configure it to listen to port 8292 of your local machine. It is also important to set the target host name and port as required. In this case, the target port needs to be set to 8290 (i.e. the port where the back-end service is running). We will now test the connection by sending a POST message that includes a payload inside an HTML body.

Add some query parameters to the URL and execute the following command:

```bash
curl -v -H "Content-Type: application/json" -d @request.json http://localhost:8290/healthcare/reserve?param1=value1&param2=value2 -X POST
```

When you execute this command, you can see the following output:

```bash
> POST /healthcare/reserve?param1=value1 HTTP/1.1
> Host: localhost:8290
> User-Agent: curl/7.54.0
> Accept: */*
> Content-Type: application/json
```

As you can see, the query parameters are present in the REST request, demonstrating that the Micro Integrator sent them along with the message. You could write resource methods to support this type of a request. In this example, the resource method accessed by this request simply ignores the parameters.