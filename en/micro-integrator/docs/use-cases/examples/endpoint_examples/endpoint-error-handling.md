# Endpoint Error Handling

The last step of a message processing inside WSO2 Micro Integrator
is to send the message to a service provider (see also [Working with
Mediators](../../../../references/mediators/about-mediators))
by sending the message to a listening service
[endpoint](../../../../references/synapse-properties/endpoint-properties) . During this process, transport
errors can occur. For example, the connection might time out, or it
might be closed by the actual service. Therefore, endpoint error
handling is a key part of any successful Micro
Integrator deployment.

Messages can fail or be lost due to various reasons in a real TCP
network. When an error occurs, if the Micro Integrator is not
configured to accept the error, it will mark the endpoint as failed,
which leads to a message failure. By default, the endpoint is marked as
failed for quite a long time, and due to this error, subsequent messages
can get lost.

To avoid lost messages, you can configure error handling at the endpoint
level. You should also run a few long-running load tests to discover
errors and fine-tune the endpoint configurations for errors that can
occur intermittently due to various reasons.

## Example 1

### Example use case

In this example, the errors 101504 and 101505 move the endpoint into the
"Timeout" state. At that point, three requests can fail for one of these
errors before the endpoint is moved into the "Suspended" state.
Additionally, errors 101500, 101501, 101506, 101507, and 101508 will put
the endpoint directly into the "Suspended" state. If a 101503 error
occurs, the endpoint will remain in the "Active" state as you have not
specified it under `         suspendOnFailure        ` . The default
setting to suspend the endpoint for all error codes except the ones
specified under `         markForSuspension        ` will apply only if
you do not specify error codes under `         suspendOnFailure        `
.

When the endpoint is first suspended, the retry happens after one
second. Because the progression factor is 2, the next suspension
duration before retry is two seconds, then four seconds, then eight, and
so on until it gets to sixty seconds, which is the maximum duration we
have configured. At this point, all subsequent suspension periods will
be sixty seconds until the endpoint succeeds and is back in the Active
state, at which point the initial duration will be used on subsequent
suspensions.

### Synapse configuration

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="TestAPI" context="/test">
   <resource methods="GET">
      <inSequence>
         <call>
            <endpoint name="Sample_First" statistics="enable" >
                <address uri="http://localhost/myendpoint" statistics="enable" trace="disable">
                    <timeout>
                        <duration>60000</duration>
                    </timeout>
            
                    <markForSuspension>
                        <errorCodes>101504, 101505</errorCodes>
                        <retriesBeforeSuspension>3</retriesBeforeSuspension>
                        <retryDelay>1</retryDelay>
                    </markForSuspension>
            
                    <suspendOnFailure>
                        <errorCodes>101500, 101501, 101506, 101507, 101508</errorCodes>
                        <initialDuration>1000</initialDuration>
                        <progressionFactor>2</progressionFactor>
                        <maximumDuration>60000</maximumDuration>
                    </suspendOnFailure>
            
                </address>
            </endpoint>
         </call>
        <respond/>
      </inSequence>
   </resource>
</api>
```

### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the integration artifacts shown above.
4. Deploy the artifacts in your Micro Integrator.

Invoke the Micro Integrator:

Invoke the sample API by executing the following command:

```bash
curl -v -X GET "http://localhost:8290/test"
```

## Example 2: Configuration for Endpoint Dynamic Timeout

### Example use case

Let's look at a sample configuration where you have dynamic timeout for
the endpoint.

In this, the timeout value is defined using a [Property
mediator](../../../../references/mediators/property-Mediator) outside
the endpoint configuration. The timeout parameter in the endpoint
configuration is then evaluated against an XPATH expression that is used
to reference and read the timeout value. Using this timeout values can
be configured without having to change the endpoint configuration.

### Synapse configuration

```xml
<sequence name=dynamic_sequence xmlns="http://ws.apache.org/ns/synapse">
  <property name="timeout" scope="default" type="INTEGER" velue=20000>
  <send>
      <endpoint>
          <address uri="http://localhost/myendpoint">
          <timeout>
            <duration>{get-property('timeout')}</duration>
            <responseAction>discard</responseAction>
          </timeout>
      </endpoint>
  </send>
</sequence>
```

You also have the option of defining a dynamic timeout for the endpoint as a [local entry](https://docs.wso2.com/display/EI650/Working+with+Local+Registry+Entries):

```xml
<localEntry key="timeout"><![CDATA[20000]]>
   <description/>
</localEntry>
```

### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the integration artifacts shown above.
4. Deploy the artifacts in your Micro Integrator.

Invoke the Micro Integrator:

## Example 3: Dynamic endpoint failover management
### Example use case

If a dynamic URL is used as the endpoint and if one URL fails, the
endpoint is suspended even though the URL is changed dynamically. Follow
the steps given below to avoid suspension or to re-enable the endpoint.

- Disabling endpoint suspension: If you do not want the endpoint to be suspended at all, you can configure the `Timeout` , `MarkForSuspension` , and `suspendOnFailure` settings as shown in the following example.
  -   Use `<errorCodes>-1</errorCodes>` to disable suspension for the endpoint under the
      `MarkForSuspension` and `suspendOnFailure` settings.
  -   Use `<responseAction>fault</responseAction>`
      under the `<timeout>         ` setting.
  -   Define the `          <initialDuration>         ` and
      `          <maximumDuration>         ` properties as
      `          0         ` under the
      `          suspendOnFailure         ` setting.

### Synapse configuration

```xml
<api xmlns="http://ws.apache.org/ns/synapse" name="TestAPI" context="/test">
   <resource methods="GET">
      <inSequence>
         <call>
            <endpoint name="NoSuspendEndpoint"> 
               <address uri="http://localhost:9000/services/SimpleStockQuoteService"> 
                   <timeout> 
                       <duration>30000</duration> 
                       <responseAction>fault</responseAction> 
                   </timeout> 
                   <suspendOnFailure> 
                       <errorCodes>-1</errorCodes> 
                       <initialDuration>0</initialDuration> 
                       <progressionFactor>1.0</progressionFactor> 
                       <maximumDuration>0</maximumDuration> 
                   </suspendOnFailure> 
                   <markForSuspension> 
                       <errorCodes>-1</errorCodes> 
                   </markForSuspension> 
               </address> 
            </endpoint>
         </call>
         <respond/>
      </inSequence>
   </resource>
</api>
```

Follow any of the options given below to re-enable an endpoint that is suspended.

-   Define the error codes that cause endpoint failure.  
    For example, use
    `          <errorCodes>101504, 101505</errorCodes>         ` to
    exclude the error codes from `          suspendOnFailure         `
    and `          markForSuspension         ` under endpoint
    configuration, so that the endpoint does not get suspended for these
    error codes.
-   If the endpoint is defined as a registry resource, activate the
    endpoint through the Java Management Extension (JMX).  
    For example, use the `          switchOn         ` operation for
    that particular endpoint in the JConsole, which comes under
    **MBeans \> org.apache.synapse \> Endpoint** . This activates the
    endpoint again.

### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the integration artifacts shown above.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

* Download the [stockquote_service.jar](
https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar)

* Run the mock service using the following command
```
$ java -jar stockquote_service.jar
```

Invoke the Micro Integrator:

Invoke the sample API by executing the following command:

```bash
curl -v -X GET "http://localhost:8290/test"
```

You will not observe any endpoint suspended logs for the above API call.

## Example 6: Configuring retry
### Example use case

You can configure the Micro Integrator to enable or disable retry for an endpoint when a specific error code occurs.

### Synapse configuration

```xml
<endpoint>
  <address uri="http://localhost:9001/services/LBService1">
    <retryConfig>
      <disabledErrorCodes>101503</disabledErrorCodes>
    </retryConfig>
  </address>
</endpoint>
<endpoint>
  <address uri="http://localhost:9002/services/LBService1">
    <retryConfig>
      <enabledErrorCodes>101503</enabledErrorCodes>
    </retryConfig>
  </address>
</endpoint>
```

In this example, if the error code 101503 occurs when trying to connect
to the first endpoint, the endpoint is not retried, whereas in the
second endpoint, the endpoint is always retried if error code 101503
occurs. You can specify enabled or disabled error codes (but not both)
for a given endpoint.

<!--
### Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the integration artifacts shown above.
4. Deploy the artifacts in your Micro Integrator.

Configure the ActiveMQ broker.

Set up the back-end service.

Invoke the Micro Integrator:
-->