# Using DELETE with a Body

Typically, DELETE is used to send a HTTP request that does not have data
enclosed as a payload. However, if you send a DELETE request with a
payload, and if the backend service accepts a DELETE request without a
payload, you can drop the payload when the message flows through WSO2
EI.

Follow the steps below to implement this scenario.

1.  Start the ESB profile and create the API that is defined in the following configuration.
    For instructions, see Creating an API.

    **DropPayloadFromDelete API**

    ``` xml
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

2.  Start TCPMon and make it listen to port 8282 of your local machine. It is also important to set the target host name and the port as required. In this case, the target port of the TCPMon needs to be set to 8280. This is the port where the backend service (i.e., WSO2 EI) is running.
3.  Open a Terminal and execute the following command to test the connection by sending a DELETE message that includes a payload inside an HTML body:

    ``` actionscript3
    curl -X DELETE --header 'Content-type: application/json' --header 'Accept: application/json' --header 'Transfer-Encoding: chunked' --data '{ "Animal": { "id": "10" }}' 'http://localhost:8280/droppayloadfromdelete'
    ```

    The TCPMon output shows that the DELETE request sent from WSO2 EI to
    the backend service does not have the payload.