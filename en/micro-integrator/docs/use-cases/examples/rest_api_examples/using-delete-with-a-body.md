# Using DELETE with a Body
## Example use case

Typically, DELETE is used to send a HTTP request that does not have data enclosed as a payload. However, if you send a DELETE request with a payload, and if the backend service accepts a DELETE request without a payload, you can drop the payload when the message flows through the Micro Integrator.

## Synapse configuration

Following is a sample REST Api configuration that we can used to implement this scenario.

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="DropPayloadFromDelete" context="/droppayloadfromdelete">
           <resource methods="DELETE OPTIONS POST PUT GET" url-mapping="/*">
              <inSequence>
                 <log level="full"/>
                 <property name="NO_ENTITY_BODY" value="true" scope="axis2" type="BOOLEAN"/>
                 <call>
                    <endpoint>
                       <address uri="http://localhost:8282/jaxrs_basic/services/customers/customerservice"/>
                    </endpoint>
                 </call>
                 <log level="full"/>
                 <respond/>
              </inSequence>
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

- Start TCPMon and make it listen to port 8282 of your local machine. It is also important to set the target host name and the port as required. In this case, the target port of the TCPMon needs to be set to 8280. This is the port where the backend service (i.e., the Micro Integrator) is running.

Open a terminal and execute the following command to test the connection by sending a DELETE message that includes a payload inside an HTML body:

```bash
curl -X DELETE --header 'Content-type: application/json' --header 'Accept: application/json' --header 'Transfer-Encoding: chunked' --data '{ "Animal": { "id": "10" }}' 'http://localhost:8280/droppayloadfromdelete'
```

The TCPMon output shows that the DELETE request sent from the Miro Integrator to the back-end service does not have the payload.