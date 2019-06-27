# Endpoint Error Handling

..This page describes error handling for endpoints. It contains the
following sections:

-   [Overview](#EndpointErrorHandling-Overview)
-   [Endpoint states](#EndpointErrorHandling-Endpointstates)
    -   [Active](#EndpointErrorHandling-ActiveActive)
    -   [Timeout](#EndpointErrorHandling-TimeoutTimeout)
    -   [Suspended](#EndpointErrorHandling-SuspendedSuspended)
-   [Configuring leaf
    endpoints](#EndpointErrorHandling-ConfiguringleafendpointsLeafEndpointConfigurations)
    -   ["Timeout"
        settings](#EndpointErrorHandling-%22Timeout%22settingstimeoutSettings)
    -   ["MarkForSuspension"
        settings](#EndpointErrorHandling-%22MarkForSuspension%22settingsmarkForSuspension)
    -   ["suspendOnFailure"
        settings](#EndpointErrorHandling-%22suspendOnFailure%22settingssuspendOnFailure)
-   [Dynamic endpoint failover
    management](#EndpointErrorHandling-Dynamicendpointfailovermanagement)
    -   [Disabling endpoint
        suspension](#EndpointErrorHandling-Disablingendpointsuspension)
    -   [Enabling the endpoint after
        suspension](#EndpointErrorHandling-Enablingtheendpointaftersuspension)
-   [Configuring retry](#EndpointErrorHandling-Configuringretry)
-   [Configuring a failover
    endpoint](#EndpointErrorHandling-ConfiguringafailoverendpointFailoverEndpointConfigurations)

### Overview

The last step of a message processing inside WSO2 Enterprise Service Bus
is to send the message to a service provider (see also [Working with
Mediators](https://docs.wso2.com/display/EI650/Working+with+Mediators) )
by sending the message to a listening service
[endpoint](_Working_with_Endpoints_) . During this process, transport
errors can occur. For example, the connection might time out, or it
might be closed by the actual service. Therefore, endpoint error
handling is a key part of any successful Enterprise
Integrator deployment.

Messages can fail or be lost due to various reasons in a real TCP
network. When an error occurs, if the Enterprise Integrator is not
configured to accept the error, it will mark the endpoint as failed,
which leads to a message failure. By default, the endpoint is marked as
failed for quite a long time, and due to this error, subsequent messages
can get lost.

To avoid lost messages, you configure error handling at the endpoint
level. You should also run a few long-running load tests to discover
errors and fine-tune the endpoint configurations for errors that can
occur intermittently due to various reasons.

For information on general error handling and error codes in the
Enterprise Integrator, see [Error
Handling](https://docs.wso2.com/display/EI650/Error+Handling) .

### Endpoint states

At any given time, the state of the endpoint can be one of the
following:

| State                                         | Description                                                                                                                       |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| [Active](#EndpointErrorHandling-Active)       | Endpoint is running and handling requests.                                                                                        |
| [Timeout](#EndpointErrorHandling-Timeout)     | Endpoint encountered an error but can still send and receive messages. If it continues to encounter errors, it will be suspended. |
| [Suspended](#EndpointErrorHandling-Suspended) | Endpoint encountered errors and cannot send or receive messages. Incoming messages to a suspended endpoint result in a fault.     |
| OFF                                           | Endpoint is not active. To put an endpoint into the OFF state, or to move it from OFF to Active, you must use JMX.                |

##### Active

When WSO2 Enterprise Service Bus starts, endpoints are in the "Active"
state and ready to handle messages. If the user does not put the
endpoint into the OFF state, it will be in the "Active" state until an
error occurs.

The endpoint can be configured to stay in the "Active" state or to go to
"Timeout" or "Suspended" based on the error codes you configure for
those states. When an error occurs, the endpoint checks to see whether
it is a "Timeout" error first, and if not, it checks to see whether it
is a "Suspended" error. If the error is not defined for either "Timeout"
or "Suspended," the error will be ignored and the endpoint will stay
Active.

##### Timeout

When an endpoint is in the "Timeout" state, it will continue to attempt
to receive messages until one message succeeds or the maximum retry
setting has been reached. If the maximum is reached at which point the
endpoint is marked as "Suspended." If one message succeeds, the endpoint
is marked as "Active."

For example, let's assume the number of retries is set to 3. When an
error occurs and the endpoint is set to the "Timeout" state, the
Enterprise Integrator can try to send up to three more messages to the
endpoint. If the next three messages sent to this endpoint result in an
error, the endpoint is put in the "Suspended" state. If one of the
messages succeeds before the retry maximum is met, the endpoint will be
marked as "Active."

##### Suspended

A "Suspended" endpoint cannot send or receive messages. When an endpoint
is put into this state, the Enterprise Integrator waits until after an
initial duration has elapsed (default is 30 seconds) before attempting
to send messages to this endpoint again. If the message succeeds, the
endpoint is marked as "Active." If the next message fails, the endpoint
is marked as "Suspended" or "Timeout" depending on the error, and the
Enterprise Integrator waits before retrying messages using the following
formula:

`         Min(current suspension duration * progressionFactor, maximumDuration)        `

You configure the initial suspension duration, progression factor, and
maximum duration as part of the [suspendOnFailure
settings](#EndpointErrorHandling-suspendOnFailure) . On each retry, the
suspension duration increases, up to the maximum duration.

### Configuring leaf endpoints

The following is the configuration for the [address
endpoint](_Address_Endpoint_) . Since we all are only interested in
error configurations, the same applies for WSDL endpoints as well. The
error handling configuration are as follows:

-   **[Timeout settings](#EndpointErrorHandling-timeoutSettings)**
-   **[MarkForSuspension
    settings](#EndpointErrorHandling-markForSuspension)**
-   **[suspendOnFailure
    settings](#EndpointErrorHandling-suspendOnFailure)**

``` java
    <address uri="endpoint address" [format="soap11|soap12|pox|get"]
        [optimize="mtom|swa"] [encoding="charset encoding"]
        [statistics="enable|disable"] [trace="enable|disable"]>
        <enableRM [policy="key"]/>?
            <enableSec [policy="key"]/>?
            <enableAddressing [version="final|submission"] [separateListener="true|false"]/>?
    
            <timeout>
                    <duration>timeout duration in seconds</duration>
                    <responseAction>discard|fault|never</responseAction>
            </timeout>?
    
            <markForSuspension>
                    [<errorCodes>xxx,yyy</errorCodes>]
                    <retriesBeforeSuspension>m</retriesBeforeSuspension>
                    <retryDelay>d</retryDelay>
            </markForSuspension>
    
            <suspendOnFailure>
                [<errorCodes>xxx,yyy</errorCodes>]
                    <initialDuration>n</initialDuration>
                    <progressionFactor>r</progressionFactor>
                    <maximumDuration>l</maximumDuration>
            </suspendOnFailure>
    </address>
```

##### "Timeout" settings

| Name           | Values                        | Default | Description                                                                                                                                                                                                               |
|----------------|-------------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| duration       | Miliseconds/ XPATH expression | 60000   | Connection timeout interval. If the remote endpoint does not respond in this time, it will be marked as "Timeout." This can be defined as a static value or as a [dynamic value](#EndpointErrorHandling-dynamicTimeout) . |
| responseAction | discard, fault, never         | never   | When a response comes to a timed out request, specifies whether to discard it or invoke the fault handler. If you select "never", the endpoint remains in the "Active" state.                                             |

##### "MarkForSuspension" settings

<table>
<thead>
<tr class="header">
<th><p>Name</p></th>
<th><p>Values</p></th>
<th><p>Default</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>errorCodes</p></td>
<td><p>Comma separatedlist of <a href="https://docs.wso2.com/display/EI650/Error+Handling#ErrorHandling-codes">error codes</a></p></td>
<td><p>101504, 101505</p></td>
<td><div class="content-wrapper">
<p>The defined error codes put the endpoint into the "Timeout" state marking it for suspension. After the number of defined <code>               retriesBeforeSuspension              </code> exceeds, the endpoint will be suspended.</p>
!!! tip
<p>If you do not specify these error codes, or if you do not specify any error codes under <code>               suspendOnFailure              </code> , by default, the "HTTP Connection Closed" (i.e., 101504) and "HTTP Connection Timeout" (i.e., 101505) errors act as "Timeout" errors, and all other errors will put the endpoint into the "Suspended" state after retrying.</p>

</div></td>
</tr>
<tr class="even">
<td><p>retriesBeforeSuspension</p></td>
<td><p>Integer</p></td>
<td><p>0</p></td>
<td><p>In the "Timeout" state this number of requests minus one can be tried and fail before the endpoint is marked as "Suspended". This setting is per endpoint, not per message, so several messages can be tried in parallel and fail and the remaining retries for that endpoint will be reduced.</p></td>
</tr>
<tr class="odd">
<td><p>retryDelay</p></td>
<td><p>milliseconds</p></td>
<td><p>0</p></td>
<td><p>The time to wait between the last retry attempt and the next retry.</p></td>
</tr>
</tbody>
</table>

##### "suspendOnFailure" settings

<table>
<thead>
<tr class="header">
<th><p>Name</p></th>
<th><p>Values</p></th>
<th><p>Default</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>errorCodes</p></td>
<td><p>Comma separated list of <a href="https://docs.wso2.com/display/EI650/Error+Handling#ErrorHandling-codes">error codes</a></p></td>
<td><p>All the errors except the errors specified in <code>              markForSuspension             </code></p></td>
<td><div class="content-wrapper">
<p>Only these defined error codes will directly send the endpoint into the "Suspended" state . Any other error code, which is not specified under <code>               MarkForSuspension              </code> will keep the endpoint in the "Active" state without suspending.</p>
!!! tip
<p>If you do not specify these error codes, by default, a ll the errors except the errors specified in <code>               markForSuspension              </code> will suspend the endpoint.</p>

</div></td>
</tr>
<tr class="even">
<td><p>initialDuration</p></td>
<td><p>milliseconds</p></td>
<td><p>30000</p></td>
<td><p>After an endpoint gets "Suspended," it will wait for this amount of time before trying to send the messages coming to it. All the messages coming during this time period will result in fault sequence activation.</p></td>
</tr>
<tr class="odd">
<td><p>progressionFactor</p></td>
<td><p>Integer</p></td>
<td><p>1</p></td>
<td><p>The endpoint will try to send the messages after the <code>              initialDuration             </code> . If it still fails, the next duration is calculated as:</p>
<p><code>              Min(current suspension duration * progressionFactor, maximumDuration)             </code></p></td>
</tr>
<tr class="even">
<td><p>maximumDuration</p></td>
<td><p>milliseconds</p></td>
<td><p>Long.MAX_VALUE</p></td>
<td><p>Upper bound of retry duration.</p></td>
</tr>
</tbody>
</table>

**Sample Configuration:**

``` java
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
```

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

**Sample Configuration for Endpoint Dynamic Timeout:**

Let's look at a sample configuration where you have dynamic timeout for
the endpoint.

In this, the timeout value is defined using a [Property
mediator](https://docs.wso2.com/display/EI650/Property+Mediator) outside
the endpoint configuration. The timeout parameter in the endpoint
configuration is then evaluated against an XPATH expression that is used
to reference and read the timeout value. Using this timeout values can
be configured without having to change the endpoint configuration.

``` xml
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

You also have the option of defining a dynamic timeout for the endpoint
as a [local
entry](https://docs.wso2.com/display/EI650/Working+with+Local+Registry+Entries)
:

``` xml
    <localEntry key="timeout"><![CDATA[20000]]>
       <description/>
    </localEntry>
```

For more information about error codes, see [Error
Codes](https://docs.wso2.com/display/EI650/Error+Handling#ErrorHandling-codes)
.

### Dynamic endpoint failover management

If a dynamic URL is used as the endpoint and if one URL fails, the
endpoint is suspended even though the URL is changed dynamically. Follow
the steps given below to avoid suspension or to re-enable the endpoint.

#### Disabling endpoint suspension

If you do not want the endpoint to be suspended at all, you can
configure the `         Timeout        ` ,
`         MarkForSuspension        ` , and
`         suspendOnFailure        ` settings as shown in the following
example.

-   Use `          <errorCodes>-1</errorCodes>         ` to disable
    suspension for the endpoint under the
    `          MarkForSuspension         ` and
    `          suspendOnFailure         ` settings.
-   Use `          <responseAction>fault</responseAction>         `
    under the `          <timeout>         ` setting.
-   Define the `          <initialDuration>         ` and
    `          <maximumDuration>         ` properties as
    `          0         ` under the
    `          suspendOnFailure         ` setting.

Example:

``` xml
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
```

#### Enabling the endpoint after suspension

Follow any of the options given below to re-enable an endpoint that is
suspended.

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
-   If the endpoint is defined under the endpoints list of the
    Management Console, manually activate the endpoint by clicking
    **Switch On** .

### Configuring retry

You can configure the Enterprise Integrator to enable or disable retry
for an endpoint when a specific error code occurs. For example:

``` html/xml
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

### Configuring a failover endpoint

For information on configuring a failover endpoint to handle errors, see
[Configuring Failover Endpoints](_Configuring_Failover_Endpoints_) .
