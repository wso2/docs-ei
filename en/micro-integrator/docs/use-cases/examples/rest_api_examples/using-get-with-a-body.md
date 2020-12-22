# GET request with a Message Body
Typically, a GET request does not contain a body, and the Micro Integrator will not consume the payload even if there is one.

## Synapse configuration

Following is a sample REST API configuration that we can use to implement this scenario. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="HealthcareService" context="/healthcare">
  <resource methods="GET POST" url-mapping="/appointment/reserve">
     <inSequence>
        <property name="REST_URL_POSTFIX" scope="axis2" action="remove"/>
        <log level="full"/>
        <respond/>
     </inSequence>
     <outSequence/>
  </resource>
</api>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create an ESB Solution project](../../../../develop/creating-projects/#esb-config-project).
3. [Create the rest api](../../../../develop/creating-artifacts/creating-an-api) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.

Send a GET request with a message body as follows:
    
```bash
curl -v -H "Content-Type: application/json" -d @request.json http://localhost:8290/healthcare/appointment/reserve -X GET
```

The `request.json` file has the following content on the appointment:
    
```json
[ 
   { 
      "name":"thomas collins",
      "hospital":"grand oak community hospital",
      "category":"surgery",
      "availability":"9.00 a.m - 11.00 a.m",
      "fee":7000.0
   },
   { 
      "name":"anne clement",
      "hospital":"clemency medical center",
      "category":"surgery",
      "availability":"8.00 a.m - 10.00 a.m",
      "fee":12000.0
   },
   { 
      "name":"seth mears",
      "hospital":"pine valley community hospital",
      "category":"surgery",
      "availability":"3.00 p.m - 5.00 p.m",
      "fee":8000.0
   }
]
```

You will see that there is no message body printed in the logs and you will get an empty response back.