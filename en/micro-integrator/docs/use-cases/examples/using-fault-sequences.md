# Using fault sequences 

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

-   ERROR_CODE
-   ERROR_MESSAGE
-   ERROR_DETAIL
-   ERROR_EXCEPTION

Within the fault sequence, you can access these property values using
the `         get-property        ` XPath function. Sample 4 uses the
log mediator as follows to log the actual error message:

```
<log level="custom">  
    <property name="text" value="An unexpected error occured"/>
    <property name="message" expression="get-property('ERROR_MESSAGE')"/>
</log>
```

Note how the ERROR\_MESSAGE property is being used to get the error message text. If you want to customize the error message that is sent
back to the client, you can use the [Fault mediator](https://docs.wso2.com/display/EI650/Fault+Mediator) as demonstrated in [Sample 5: Creating SOAP Fault Messages and Changing the Direction of a Message](https://docs.wso2.com/display/ESB500/Sample+5%3A+Creating+SOAP+Fault+Messages+and+Changing+the+Direction+of+a+Message).  
