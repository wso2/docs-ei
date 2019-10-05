# Using POST with No Body
## Example use case

Typically, POST is used to send a message that has data enclosed as a payload inside an HTML body. However, you can also use POST without a payload. WSO2 Micro Integrator treats it as a normal message and forwards it to the endpoint without any extra configuration.

The following diagram depicts a scenario where a REST client communicates with a REST service using the ESB profile. Apache Tcpmon is used solely for monitoring the communication between the ESB profile and the back-end service and has no impact on the messages passed between the ESB profile and back-end service. For this particular scenario, the cURL client is used as the REST client, and the [basic JAX-RS sample](https://docs.wso2.com/display/EI650/JAX-RS+Basics) is used as the back-end REST service. 

## Synapse configuration 

Following is a sample REST Api configuration that we can used to implement this scenario. 

In this proxy configuration, testAPI intercepts messages that are sent to the relative URL `/customerservice/customers` and sends them to the relevant endpoint by appending the url-mapping of the resource tag to the end of the endpoint URL.

```xml      
<api xmlns="http://ws.apache.org/ns/synapse" name="HealthcareService" context="/healthcare">
  <resource methods="POST" url-mapping="/appointment/reserve">
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

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project)
3. [Create the rest api](../../../../develop/creating-artifacts/creating-an-api) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Set up the back-end service.

- Start tcpmon and make it listen to port 8292 of your local machine. It is also important to set the target host name and the port as required. In this case, the target port needs to be set to 8290 (i.e. the port where the backend service is running).  We will now test the connection by sending a POST message that includes a payload inside an HTML body.

Invoke the REST Api:

1.  Open a terminal and issue the following command: 
    
    ```bash
    curl -v -H "Content-Type: application/json" -d @request.json http://localhost:8290/healthcare/appointment/reserve -X POST
    ```
    where `        request.json        ` has the following content on the appointment:
        
    ```json
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

    The following reply message appears in the console:

    ```json
    {
       "appointmentNumber": 1,
       "doctor": {
          "name": "thomas collins",
          "hospital": "grand oak community hospital",
          "category": "surgery",
          "availability": "9.00 a.m - 11.00 a.m",
          "fee": 7000
       },
       "patient": {
          "name": "John Doe",
          "dob": "1940-03-19",
          "ssn": "234-23-525",
          "address": "California",
          "phone": "8770586755",
          "email": "johndoe@gmail.com"
       },
       "fee": 7000,
       "confirmed": false,
       "appointmentDate": "2025-04-02"
    }
    ```

2.  Now send the same POST message but without the enclosed data as follows: 

    ```bash
    curl -v -H "Content-Type: application/json" -d '' http://localhost:8290/healthcare/appointment/reserve -X POST
    ```

    !!! Note
        You would need to configure the backend service to handle such requests, if not the Micro Integrator will throw exceptions.

The tcpmon output shows the same REST request that was sent by the client, demonstrating that the Micro Integrator handled the POST message regardless of whether it included a payload.