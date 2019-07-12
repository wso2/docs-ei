# Configuring Failover Endpoints

-   [XML configuration](#ConfiguringFailoverEndpoints-XMLconfiguration)
-   [Handling errors in failover
    endpoints](#ConfiguringFailoverEndpoints-Handlingerrorsinfailoverendpoints)
    -   [Sample failover with one address
        endpoint](#ConfiguringFailoverEndpoints-Samplefailoverwithoneaddressendpoint)
    -   [Sample failover with multiple address
        endpoints](#ConfiguringFailoverEndpoints-Samplefailoverwithmultipleaddressendpoints)

With leaf endpoints, if an error occurs during a message transmission
process, that message will be lost. The failed message will not be
retried again. These errors occur very rarely, but still message
failures can occur. With some applications these message losses are
acceptable, but if even rare message failures are not acceptable, use
the failover endpoint.

A **Failover Group** is a list of leaf endpoints grouped together for
the purpose of passing an incoming message from one endpoint to another
if a failover occurs. The first endpoint in failover group is considered
the primary endpoint. An incoming message is first directed to the
primary endpoint, and all other endpoints in the group serve as
back-ups.

If the primary endpoint fails, the next active endpoint is selected as
the primary endpoint, and the failed endpoint is marked as
inactive. Thus, failover group ensures that a message is delivered as
long as there is at least one active endpoint among the listed
endpoints. The ESB switches back to the primary endpoint as soon as it
becomes available. This behaviour is known as dynamic failover.

!!! info

An endpoint failure occurs when an endpoint is unable to invoke a
service. An endpoint, which responds with an error is not considered a
failed endpoint.


  

------------------------------------------------------------------------

[XML configuration](#ConfiguringFailoverEndpoints-XMLconfiguration) \|
[Handling errors in failover
endpoints](#ConfiguringFailoverEndpoints-Handlingerrorsinfailoverendpoints)

------------------------------------------------------------------------

### XML configuration

``` html/xml
    <failover>
        <endpoint .../>+
    </failover>
```

### Handling errors in failover endpoints

An endpoint can be in one of the following states:
`         Active        ` , T `         imeout        ` or
`         Suspended        ` . You can handle errors and configure the
looping between the failover child endpoints via the
`         <timeout>        ` , `         <suspendOnFailure>        ` and
`         <markForSuspension>        ` properties based on these states.
For more information about these states and properties, see [Endpoint
Error Handling](_Endpoint_Error_Handling_) .

#### Sample failover with one address endpoint

When message failure is not tolerable even though there is only one
service endpoint, then failovers are possible with a single endpoint as
shown in the below configuration.

``` java
    <endpoint name="SampleFailover">
        <failover>
            <endpoint name="Sample_First" statistics="enable" >
                <address uri="http://localhost/myendpoint" statistics="enable" trace="disable">
                    <timeout>
                        <duration>60000</duration>
                    </timeout>
    
                    <markForSuspension>
                        <errorCodes>101504, 101505, 101500</errorCodes>
                        <retriesBeforeSuspension>3</retriesBeforeSuspension>
                        <retryDelay>10</retryDelay>
                    </markForSuspension>
    
                    <suspendOnFailure>
                        <initialDuration>1000</initialDuration>
                        <progressionFactor>2</progressionFactor>
                        <maximumDuration>64000</maximumDuration>
                    </suspendOnFailure>
    
                </address>
            </endpoint>
        </failover>
    </endpoint>
```

In the above example, the `         Sample_First        ` endpoint is
marked as `         Timeout        ` if a connection times out, closes,
or sends IO errors after retrying for `         60000        `
miliseconds.

When one of the errors of the specified codes occurs (i.e.,
`         101504, 101505        ` and `         101500)        ` , the
failover will retry using the first non-suspended endpoint. In this
case, it is the same endpoint ( `         Sample_First        ` ). It
will retry until the retry count (i.e. 3 in the above example) becomes 0
with a delay as specified by the `         <retryDelay>        `
property (i.e., `         10        ` miliseconds in the above example).

For all the other errors, it will be marked as
`         Suspended        ` . For more information about these states
and properties, see [Endpoint Error Handling](_Endpoint_Error_Handling_)
.

!!! info

The retry count is per endpoint, not per message. The retry happens in
parallel. Since messages come to this endpoint via many threads, the
same message may not be retried three times. Another message may fail
and can reduce the retry count.


In this configuration, we assume that these errors are rare and if they
happen once in a while, it is okay to retry again. If they happen
frequently and continuously, it means that it requires immediate
attention to get it back to normal state.

#### Sample failover with multiple address endpoints

When a message reaches a failover endpoint with multiple address
endpoints, it will go through its list of endpoints to pick the first
one in `         Active        ` or `         Timeout        ` state
(not in the `         Suspended        ` state). Then, it will send the
message using that particular endpoint.

If a failure occurs with the first endpoint within the failover group
and if this error does not put the first endpoint into
`         Suspended        ` state, the retry will happen using the same
endpoint.

However, if the first endpoint is suspended or if an error occurs while
sending the message with the first endpoint, the failover endpoint will
go through the endpoint list again from the beginning and will try to
send the requests using the next endpoint, which is in the
`         Active        ` or `         Timeout        ` state.
Neverthless, when the first endpoint becomes ready to send again, it
will try again on the first endpoint, even though the second endpoint is
still active. For more information about these states and properties,
see [Endpoint Error Handling](_Endpoint_Error_Handling_) .

The following is an example failover endpoint configuration with
multiple address endpoints.

``` java
    <endpoint>
        <failover>
            <endpoint name="fooEP">
                <http uri-template="http://localhost:8080/foo">
                <timeout>
                    <duration>10000</duration>
                    <responseAction>fault</responseAction>
                </timeout>
                <suspendOnFailure>
                    <errorCodes>101503,101504,101505,101507</errorCodes>
                    <initialDuration>100</initialDuration>
                    <progressionFactor>1.0</progressionFactor>
                    <maximumDuration>30000</maximumDuration>
                </suspendOnFailure>
            </http>
        </endpoint>
        <endpoint name="barEP">
            <http uri-template="http://localhost:8080/bar">
            <timeout>
                <duration>10000</duration>
                <responseAction>fault</responseAction>
            </timeout>
            <suspendOnFailure>
                <errorCodes>101503,101504,101505,101507</errorCodes>
                <initialDuration>100</initialDuration>
                <progressionFactor>1.0</progressionFactor>
                <maximumDuration>30000</maximumDuration>
            </suspendOnFailure>
            <retryConfig>
                <disabledErrorCodes>101507,101504</disabledErrorCodes>
            </retryConfig>
        </http>
    </endpoint>
    </failover>
    </endpoint>
```

!!! note

The `         <retryConfig>        ` property configures the last child
endpoint to stop retying by ending the loop (i.e. to make the endpoint
respond back to the service), after attempting to send requests to all
the child endpoints and when all the attempts fail.

``` java
    <retryConfig>
            <disabledErrorCodes>101507,101504</disabledErrorCodes>
    </retryConfig>
```
    
    Thus, in the above configuration, erros of the codes
    `         101504        ` and `         101507        ` stop retrying
    and put the endpoint to the `         Timeout        ` state.
    
