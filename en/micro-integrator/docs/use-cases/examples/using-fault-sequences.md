# Using fault sequences 

WSO2 Micro Integrator provides fault sequences for dealing with errors. Whenever an error occurs, the mediation engine attempts to provide as much information as possible on the error to the user by initializing the following properties on the erroneous message:

-	ERROR_CODE
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
