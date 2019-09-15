# Using POST with No Body

Typically, POST is used to send a message that has data enclosed as a payload inside an HTML body. However, you can also use POST without a payload. WSO2 Enterprise Integrator (WSO2 EI) treats it as a normal message and forwards it to the endpoint without any extra configuration.

The following diagram depicts a scenario where a REST client communicates with a REST service using the ESB profile. Apache Tcpmon is used solely for monitoring the communication between the ESB profile and the back-end service and has no impact on the messages passed between the ESB profile and back-end service. For this particular scenario, the cURL client is used as the REST client, and the [basic JAX-RS sample](https://docs.wso2.com/display/EI650/JAX-RS+Basics) is used as the back-end REST service.  

To implement this scenario:

1.  Configure and deploy the JAX-RS Basics sample by following the
    instructions for b uilding and running the sample on the [JAX-RS
    Basics](https://docs.wso2.com/display/EI650/JAX-RS+Basics) page.
2.  [Start the ESB
    profile](https://docs.wso2.com/display/EI650/Running+the+Product)
    and change the configuration as follows:

    ```
    <definitions xmlns="http://ws.apache.org/ns/synapse">
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
    </definitions>
    ```

    In this proxy configuration, testAPI intercepts messages that are sent to the relative URL `/customerservice/customers` and sends them to the relevant endpoint by appending the url-mapping of the resource tag to the end of the endpoint URL.
3.  Start tcpmon and make it listen to port 8282 of your local machine. It is also important to set the target host name and the port as required. In this case, the target port needs to be set to 8280 (i.e. the port where the backend service is running).  We will now test the connection by sending a POST message that includes a payload inside an HTML body. 
4.  Go to the terminal and issue the following command: 
    `curl -v -H "Content-Type: application/xml" -d "<Customer><id>123</id><name>John</name></Customer>" http://localhost:8280/customerservice/customers`  
5.  The following reply message appears in the console:

    ```
    <Customer>
        <id>132</id>
        <name>John</name>
    </Customer> 
    ```

6.  Now send the same POST message but without the enclosed data as follows:  
    `curl -v -H "Content-Type: application/xml" -d "" http://localhost:8280/customerservice/customer`

    !!! Note
        You would need to configure the backend service to handle such requests, if not the theESB profile will throw exceptions.

The tcpmon output shows the same REST request that was sent by the client, demonstrating that the ESB profile handled the POST message regardless of whether it included a payload.