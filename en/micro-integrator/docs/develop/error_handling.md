# Error Handling

The main role of WSO2 Enterprise Integrator (WSO2 EI) is to act as the
backbone of an organization’s service-oriented architecture. It is the
spine through which all the systems and applications within the
enterprise (and external applications that integrate with the
enterprise) communicate with each other. For example, an ESB (which is
contained in WSO2 EI) often has to deal with many wire-level protocols,
messaging standards, and remote APIs. But applications and networks can
be full of errors. Applications crash. Network routers and links get
into states where they cannot pass messages through with the expected
efficiency. These error conditions are very likely to cause a fault or
trigger a runtime exception in the ESB.

### Using fault sequences 

WSO2 EI provides fault sequences for dealing with errors. A fault
sequence is a collection of mediators just like any other sequence, and
it can be associated with another sequence or a proxy service. When the
sequence or the proxy service encounters an error during mediation or
while forwarding a message, the message that triggered the error is
delegated to the specified fault sequence. Using the available mediators
it is possible to log the erroneous message, forward it to a special
error-tracking service, and send a SOAP fault back to the client
indicating the error or even send an email to the system admin.

It is not mandatory to associate each sequence and proxy service with a
fault sequence. In situations where a fault sequence is not specified
explicitly, a default fault sequence will be used to handle errors.
[Sample 4: Specifying a Fault Sequence with a Regular Mediation
Sequence](https://docs.wso2.com/display/ESB500/Sample+4%3A+Specifying+a+Fault+Sequence+with+a+Regular+Mediation+Sequence)
shows how to specify a fault sequence with a regular mediation sequence.

Whenever an error occurs in WSO2 EI, the mediation engine attempts to
provide as much information as possible on the error to the user by
initializing the following properties on the erroneous message:

-   ERROR\_CODE
-   ERROR\_MESSAGE
-   ERROR\_DETAIL
-   ERROR\_EXCEPTION

Within the fault sequence, you can access these property values using
the `         get-property        ` XPath function. Sample 4 uses the
log mediator as follows to log the actual error message:  
`         <log level="custom">        `  
`         <property name="text" value="An unexpected error occured"/>        `  
`         <property name="message" expression="get-property('ERROR_MESSAGE')"/>        `  
`         </log>        ` `        `

Note how the ERROR\_MESSAGE property is being used to get the error
message text. If you want to customize the error message that is sent
back to the client, you can use the [Fault
mediator](https://docs.wso2.com/display/EI650/Fault+Mediator) as
demonstrated in [Sample 5: Creating SOAP Fault Messages and Changing the
Direction of a
Message](https://docs.wso2.com/display/ESB500/Sample+5%3A+Creating+SOAP+Fault+Messages+and+Changing+the+Direction+of+a+Message)
.  

### Error codes

This section describes error codes and their meanings.

#### Transport error codes

|                |                                                                                                       |
|----------------|-------------------------------------------------------------------------------------------------------|
| **Error Code** | **Detail**                                                                                            |
| 101000         | Receiver input/output error sending                                                                   |
| 101001         | Receiver input/output error receiving                                                                 |
| 101500         | Sender input/output error sending                                                                     |
| 101501         | Sender input/output error receiving                                                                   |
| 101503         | Connection failed                                                                                     |
| 101504         | Connection timed out (no input was detected on this connection over the maximum period of inactivity) |
| 101505         | Connection closed                                                                                     |
| 101506         | NHTTP protocol violation                                                                              |
| 101507         | Connection canceled                                                                                   |
| 101508         | Request to establish new connection timed out                                                         |
| 101509         | Send abort                                                                                            |
| 101510         | Response processing failed                                                                            |

If the HTTP PassThrough transport is used, and a connection-level error
occurs, the error code is calculated using the following equation:

``` java
    Error code = Base error code + Protocol State
```

The sender side of the transport matches the protocol state, which
changes according to the phase of the message.

Following are the possible protocol states and the description for each:

| Protocol State     | Description                                                |
|--------------------|------------------------------------------------------------|
| REQUEST\_READY (0) | Connection is at the initial stage ready to send a request |
| REQUEST\_HEAD(1)   | Sending the request headers through the connection         |
| REQUEST\_BODY(2)   | Sending the request body                                   |
| REQUEST\_DONE(3)   | Request is completely sent                                 |
| RESPONSE\_HEAD(4)  | The connection is reading the response headers             |
| RESPONSE\_BODY(5)  | The connection is reading the response body                |
| RESPONSE\_DONE(6)  | The response is completed                                  |
| CLOSING(7)         | The connection is closing                                  |
| CLOSED(8)          | The connection is closed                                   |

Since there are several possible protocol states in which a request can
time out, you can calculate the error code accordingly using the values
in the table above. For example, in a scenario where you send a request
and the request is completely sent to the backend, but a timeout happens
before the response headers are received, the error code is calculated
as follows:

In this scenario, the base error code is
`         CONNECTION_TIMEOUT(101504)        ` and the protocol state is
`         REQUEST_DONE(3).        `

Therefore,

Error code = 101504 + 3 = 101507

#### Endpoint failures

This section describes the error codes for endpoint failures. For more
information on handling endpoint errors, see [Endpoint Error
Handling](https://docs.wso2.com/display/EI650/Endpoint+Error+Handling) .

**General errors**

|                |                                               |
|----------------|-----------------------------------------------|
| **Error Code** | **Detail**                                    |
| 303000         | Load Balance endpoint is not ready to connect |
| 303000         | Recipient List Endpoint is not ready          |
| 303000         | Failover endpoint is not ready to connect     |
| 303001         | Address Endpoint is not ready to connect      |
| 303002         | WSDL Address is not ready to connect          |

**Failure on endpoint in the session**

|                |                                                               |
|----------------|---------------------------------------------------------------|
| **Error Code** | **Detail**                                                    |
| 309001         | Session aware load balance endpoint, No ready child endpoints |
| 309002         | Session aware load balance endpoint, Invalid reference        |
| 309003         | Session aware load balance endpoint, Failed session           |

**Non-fatal warnings**

|                |                                                |
|----------------|------------------------------------------------|
| **Error Code** | **Detail**                                     |
| 303100         | A failover occurred in a Load balance endpoint |
| 304100         | A failover occurred in a Failover endpoint     |

**Referring real endpoint is null**

|                |                             |
|----------------|-----------------------------|
| **Error Code** | **Detail**                  |
| 305100         | Indirect endpoint not ready |

**Callout operation failed**

|                |                                                                                                 |
|----------------|-------------------------------------------------------------------------------------------------|
| **Error Code** | **Detail**                                                                                      |
| 401000         | Callout operation failed (from the Callout mediator)                                            |
| 401001         | Blocking call operation failed (from the Call mediator when you have enabled blocking in it).   |
| 401002         | Blocking sender operation failed (from the Call mediator when you have enabled blocking in it). |

#### Custom error codes

|                |                                                                                                                                                                                                                                                                                    |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Error Code** | **Detail**                                                                                                                                                                                                                                                                         |
| 500000         | Endpoint Custom Error - This error is triggered when the endpoint is prefixed by `              <property name="FORCE_ERROR_ON_SOAP_FAULT" value="true"/>             ` , which enhances the failover logic by marking an endpoint as suspended when the response is a SOAP fault. |
