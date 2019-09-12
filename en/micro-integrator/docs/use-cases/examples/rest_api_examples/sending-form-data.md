# Sending form data

In this scenario, an API is used to send form data to a REST service, which accepts data of the `x-www-form-urlencoded`
content type.

### Setting up the backend

Configure an endpoint as follows for the REST back end service to which the form data should be sent. Note that the endpoint has to be an HTTP endpoint. See [Adding an Endpoint](https://docs.wso2.com/display/EI650/Working+with+Endpoints+via+Tooling) for further instructions.

```
<endpoint xmlns="http://ws.apache.org/ns/synapse" name="FormDataReceiver">
       <http uri-template="http://www.eaipatterns.com/MessageEndpoint.html" method="post">
          <suspendOnFailure>
             <progressionFactor>1.0</progressionFactor>
          </suspendOnFailure>
          <markForSuspension>
             <retriesBeforeSuspension>0</retriesBeforeSuspension>
             <retryDelay>0</retryDelay>
          </markForSuspension>
       </http>
</endpoint>
```

### Configuring the API

Configure the API as follows:

```
<api xmlns="http://ws.apache.org/ns/synapse" name="FORM" context="/Service">
    <resource methods="POST">
          <inSequence>
             <log level="full"></log>
             <property name="name" value="Mark" scope="default" type="STRING"></property>
             <property name="company" value="wso2" scope="default" type="STRING"></property>
             <property name="country" value="US" scope="default" type="STRING"></property>
             <payloadFactory media-type="xml">
                <format>
                   <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
                      <soapenv:Body>
                         <root xmlns="">
                            <name>$1</name>
                            <company>$2</company>
                            <country>$3</country>
                         </root>
                      </soapenv:Body>
                   </soapenv:Envelope>
                </format>
                <args>
                   <arg evaluator="xml" expression="$ctx:name"></arg>
                   <arg evaluator="xml" expression="$ctx:company"></arg>
                   <arg evaluator="xml" expression="$ctx:country"></arg>
                </args>
             </payloadFactory>
             <log level="full"></log>
             <property name="messageType" value="application/x-www-form-urlencoded" scope="axis2" type="STRING"></property>
             <property name="DISABLE_CHUNKING" value="true" scope="axis2" type="STRING"></property>
             <call>
                <endpoint key="FormDataReceiver"></endpoint>
             </call>
             <respond></respond>
          </inSequence>
       </resource>
</api>
```

In this configuration, `name` , `company` and `country` are defined as
properties to be sent as form data using the **Property mediator**. These properties are set as key value pairs by the [PayloadFactory mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator) and sent to the the ESB profile. Then the messageType property is set to `         application/x-www-form-urlencoded        ` to enable the ESB profile to identify these key value pairs as form data. The
DISABLE\_CHUNKING property is set to `         true        ` t remove the chunking from the outgoing message. Then the **Call
mediator** (https://docs.wso2.com/display/EI650/Call+Mediator) is used to send the message to the endpoint defined for the REST backend service.

### Executing the sample

Send a request to the backend as follows:

`curl -v -X POST -d @request -H "Content-Type: text/xml" http://localhost:8280/Service`