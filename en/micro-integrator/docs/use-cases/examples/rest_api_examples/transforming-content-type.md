# Transforming the content type
## Example use case  
This section describes how you can transform the content type of a message using an API. In this scenario, the API exposes a REST back-end service that accepts and returns XML and JSON messages for HTTP methods as follows:
    
-   GET - response is in JSON format
-   POST - accepts JSON request and returns response in XML format
-   DELETE - empty request body should be sent and returns response in XML format 
    
## Synapse configuration
    
Create an API using the following configuration:
    
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
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
    <resource methods="GET" uri-template="/appointments/{appointmentNo}">
        <inSequence>
            <send>
                <endpoint>
                    <address uri="http://localhost:9090/healthcare"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/json" scope="axis2"/>
            <send/>
        </outSequence>
    </resource>
    <resource methods="DELETE" uri-template="/appointments/{appointmentNo}">
        <inSequence>
            <send>
                <endpoint>
                    <address uri="http://localhost:9090/healthcare"/>
                </endpoint>
            </send>
        </inSequence>
        <outSequence>
            <log level="full"/>
            <property name="messageType" value="application/xml" scope="axis2"/>
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
  
Following is the cURL command to send an HTTP POST request to the API:

!!! Tip
    The context of the API is ‘/healthcare’. For every HTTP method, a url-mapping or uri-template is defined, and the URL to call the methods differ with the defined mapping or template.
    
`curl -v -H "Content-Type: application/json" -X POST -d @request.json http://localhost:8290/healthcare/appointment/reserve`where `request.json` has the following content on the appointment:
    
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
The response from backend to Micro Integrator will be:

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

The Micro Integrator transform the response to XML and send it back to client as:

```xml
<jsonObject>
   <appointmentNumber>1</appointmentNumber>
   <doctor>
      <name>thomas collins</name>
      <hospital>grand oak community hospital</hospital>
      <category>surgery</category>
      <availability>9.00 a.m - 11.00 a.m</availability>
      <fee>7000.0</fee>
   </doctor>
   <patient>
      <name>John Doe</name>
      <dob>1940-03-19</dob>
      <ssn>234-23-525</ssn>
      <address>California</address>
      <phone>8770586755</phone>
      <email>johndoe@gmail.com</email>
   </patient>
   <fee>7000.0</fee>
   <confirmed>false</confirmed>
   <appointmentDate>2025-04-02</appointmentDate>
</jsonObject>
```

Following is the CURL command to send a GET request to the API:
    
` curl -v -X GET http://localhost:8290/healthcare/appointments/1                        `
    
The response for the request will be:
    
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

Following is the cURL command for sending an HTTP DELETE request:
    
`curl -v -X DELETE http://localhost:8290/healthcare/appointments/1                            `
    
This request will be sent to the back end, and the order with the specified ID will be deleted.

The response to the Micro Integrator from backend will be,

```json
{"status":"Appointment is successfully removed"}
```

The Micro Integrator transform the response to XML and sends it back to client as:

```xml
<jsonObject><status>Appointment is successfully removed</status></jsonObject>
```