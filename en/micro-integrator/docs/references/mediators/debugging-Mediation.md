# Debugging Mediation

Message mediation mode is one of the operational modes of the ESB
profile where the ESB profile functions as an intermediate message
router. W hen operating in this mode, i t can filter, transform, drop or
forward messages to an endpoint based on the given parameters. A unit of
the mediation flow is a mediator. Sequences define the message mediation
behavior of the ESB profile. A sequence is a series of mediators, where
each mediator is a unit entity that can input a message, carry out a
predefined processing task on the message, and output the message for
further processing.

-   [What is debugging with respect to
    mediation](#DebuggingMediation-Whatisdebuggingwithrespecttomediation)
-   [Instant debugging using Micro
    Integrator](#DebuggingMediation-InstantdebuggingusingMicroIntegrator)
-   [Debugging with external WSO2 EI
    server](#DebuggingMediation-DebuggingwithexternalWSO2EIserver)
    -   [Creating the artifact](#DebuggingMediation-Creatingtheartifact)
    -   [Enabling mediation
        debugging](#DebuggingMediation-Enablingmediationdebugging)
    -   [Information provided by the Debugger
        Tool](#DebuggingMediation-InformationprovidedbytheDebuggerTool)
    -   [Changing the property
        values](#DebuggingMediation-Changingthepropertyvalues)
    -   [Viewing wire logs](#DebuggingMediation-Viewingwirelogs)

## What is debugging with respect to mediation

Debugging is where you want to know if these units, which function as
separate entities are operating as intended, or if a combination of
these units are operating as a whole as intended. The ESB profile packs
the **Mediation debugger** that enables you to debug the ESB
profile message mediation flow in the server. Tooling support for the
Mediation debugger is provided by the **WSO2 Integration Studio Plugin**
which comes out of the box with WSO2 Integration Studio.

First f ollow the steps below to create a sample the ESB
profile artifact, which you will debug .

1.  Install the plugin and run it. For instructions, see [Installing
    WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
    .  
2.  Create the ESB profile artifact using the WSO2 Integration Studio .
    For instructions, see [WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/WSO2+Integration+Studio)
    .

There are two ways to debug a developed mediation flow.

1.  Instant debugging using Micro Integrator ( a light version of ESB
    profile) packaged with WSO2 Integration Studio.
2.  Deploy artifacts to an external WSO2 EI server and debug  
      

Above two approaches are discussed in detail below.

## Instant debugging using Micro Integrator

  

1.  When project artifacts are ready, on the project panel select the
    project you want to debug and click on **Run** \> **Debug** .  
    ![](attachments/119131423/119131448.png?effects=drop-shadow){width="200"}  
      
2.  It will ask to choose the artifacts those needs to be deployed to
    Embedded Micro EI server. Internally WSO2 Integration Studio will
    generate a CAR application with choosed artifacts and deploy.  
    ![](attachments/119131423/119131447.png?effects=drop-shadow){width="500"}
3.  On the console of WSO2 Integration Sturdio, notice that Micro
    Integrator is started with artifacts deployed. HTTP traffic is
    listened on 8290 port.
4.  Add some breakpoints in the flow as below. You can mark a particular
    mediator as a breakpoint.  
    ![](attachments/119131423/119131439.png?effects=drop-shadow){width="800"}
5.  Invoke the service using SOAP UI or some external client. As soon as
    a request comes to the proxy service fisrt brak point will hit.  
    ![](attachments/119131423/119131446.png){width="800" height="431"}  
      
    Note that you can view the payload that comes into the mediator and
    the properties that you can access on the message context.  
      
6.  Click on " **Continue** " button. Then the message will be sent to
    the backend by call mediator and next breakpoint, the log mediator
    will hit.  
    ![](attachments/119131423/119131445.png){width="800" height="424"}  
      
    Note that response can be viewed on **Message Envelope** tab. The
    property set before calling the endpoint is also accessible in the
    context.  
      
7.  Click on  " **Continue** " button again. Response will be received
    by the client.  
    ![](attachments/119131423/119131444.png?effects=drop-shadow){width="800"}

  

## Debugging with external WSO2 EI server

### Creating the artifact

1.  Deploy the artifact you created on the ESB profile.
    For instructions, see [Packaging your Artifacts into Composite
    Applications](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
    .

### Enabling mediation debugging

Follow the steps below to enable debugging with respect to mediation.

1.  Click **Run** in the top menu of the WSO2 Integration Studio, and
    then click **Debug Configurations** .  
    ![](attachments/119131423/119131442.png?effects=drop-shadow){width="200"}
2.  Enter the details to create a new configuration as shown in the
    example below. You need to define two port numbers and a hostname t
    o connect the ESB profile with WSO2 Integration Studio in the
    mediation debug mode. Note that you need to specify debug mode as
    **Remote** .  
    ![](attachments/119131423/119131441.png?effects=drop-shadow){width="500"}  
      
3.  Add the new configuration to debug menu as below. Then you can
    access the configuation easily.  
    ![](attachments/119131423/119131438.png?effects=drop-shadow){width="500"}  
      
4.  Execute the following commands to s tart WSO2 EI server in the debug
    mode by passing a system variable at start up:  
    -   On Windows:
        `            <EI_HOME>\bin\integrator.bat --run            -Desb.debug=true           `
    -   On Linux/Solaris:
        `            sh <EI_HOME>/bin/            integrator            .sh            -Desb.debug=true           `
5.  Click **downward arrow beside** **Debug** in the WSO2 Integration
    Studio, and select the new profile above created when the Console
    indicates the following.

        !!! note
    
        You have approximately a one-minute time span to connect WSO2
        Integration Studio with the EI server for the execution of the above
        created debug configuration. Otherwise, the EI server will stop
        listening and start without connecting with the debugger tool.
    

    ![](attachments/119131423/119131440.png?effects=drop-shadow){width="900"}  
    ![](attachments/119131423/119131437.png?effects=drop-shadow){width="200"}

6.  In WSO2 Integration Studio, r ight-click and add breakpoints or skip
    points on the desired mediators to start debugging as shown in the
    example below.

    ![](attachments/119131423/119131439.png?effects=drop-shadow){height="250"}

      

        !!! info
        You can add the following debugging options on the mediators using
        the right click context menu.
    
        -   **Toggle Breakpoint:** Adds a breakpoint to the selected
            mediator
        -   **Toggle Skip Point:** Adds a skip point to the selected
            mediator
        -   **Resend EI Debug Points:** I f you re-start the the ESB
            profile, or i f you re-deploy the proxy service after changing
            its Synapse configuration, you need to re-send the information
            on breakpoints to the WSO2 EI server. T his re-s ends all
            registered debugging points to the EI Server.
        -   **Delete All EI Debug Points:** Deletes all registered debug
            points from the EI Server and WSO2 Integration Studio.
    

7.  Now you can send a request to the external WSO2 EI server and debug
    the flow as discussed under "Instant debugging using Micro
    Integrator".

### Information provided by the Debugger Tool

When your target artifact gets a request message and when the mediation
flow reaches a mediator marked as a breakpoint, the message mediation
process suspends at that point. A tool tip message of the suspended
mediator displays the message envelope of the message payload at that
point as shown in the example below .

![](attachments/119131423/119131436.png?effects=drop-shadow){width="650"}

You can view the message payload at that point of the message flow also
in the **Message Envelope** tab as shown below.

![](attachments/119131423/119131435.png?effects=drop-shadow){width="650"}  

Also, you can view the message mediation properties in the **Variables**
view as shown in the example below.

The **Variable** view contains properties of the following property
scopes.

-   Axis2-Client Scope Properties

-   Axis2 Scope Properties

-   Operation Scope Properties

-   Synapse Scope Properties

-   Transport Scope Properties

You can have a list of selected properties out of the above, in the
properties table of the **Message Envelope** tab, and view information
on the property keys and values of them as shown below.

![](attachments/119131423/119131433.png?effects=drop-shadow){width="650"}

Click **Add Property** , specify the context and name of the property,
and then click **OK** , to add that property to the properties table in
the **Message Envelope** tab as shown below.

!!! tip

Click **Clear Property** , to remove a property from the properties
table.


![](attachments/119131423/119131432.png?effects=drop-shadow){width="650"}

### Changing the property values

There are three operations that you can perform on message mediation
property values as described below.

#### Injecting new properties

Follow the steps below to inject new properties to the ESB profile while
debugging.

1.  Right click on the **Variable** view, click **Inject/Clear
    Property** , and then click **Inject Property** as shown below.  
    ![](attachments/119131423/119131431.png?effects=drop-shadow){width="650"}
2.  Enter the details about the property you prefer to add as shown in
    the example below.  
    ![](attachments/119131423/119131430.png?effects=drop-shadow){width="500"}
3.  Click **OK** .
4.  When the next debug point is hit, you will see the property is set
    to the specified context.  
    ![](attachments/119131423/119131429.png){height="250"}

#### Clearing a property

Follow the steps below to c lear an existing property from the the ESB
profile.

1.  Right click on the **Variable** view, click **Inject/Clear
    Property** , and then click **Clear Property** as shown below.  
    ![](attachments/119131423/119131428.png?effects=drop-shadow){width="650"}
2.  Enter the details about the property you want to clear as shown in
    the example below.  
    ![](attachments/119131423/119131427.png?effects=drop-shadow){width="500"}
3.  Click **OK** .

#### Modifying a property

1.  Click on the value section of the preferred property and change the
    value in the **Variable** view as shown in the example below, to
    modify it .  
    ![](attachments/119131423/119131426.png?effects=drop-shadow){width="650"}  
2.  You will see the property is changed on the property view.  
    ![](attachments/119131423/119131425.png?effects=drop-shadow){width="650"}  

  

### Viewing wire logs

While debugging a Synapse flow, you can view the the actual HTTP
messages at the entry point of the ESB profile via wire logs. For
example, you can view wire logs of the incoming flow and the final
response of a proxy service. Also, you can view wire logs for points,
where it goes out from the ESB profile. For example, you can see
the outgoing and incoming wire logs for specific mediators (i.e. Call
mediator, Send mediator etc.). Wire logs are useful to troubleshoot
unexpected issues, which occurr while integrating miscallaneous systems.
You can use wire logs to verify whether the message payload is properly
going out from the server, whether the HTTP headers such as the
content-type is properly set in the outgoing message etc.

  

!!! note

It is recommended to enable wire logs only for troubleshooting purposes.
Running productions systems with wire logs enabled is not recommended.


  

#### Enabling wire logs

The passthrough HTTP transport is the main transport, which handles
HTTP/HTTPS messages in WSO2 EI. Un-comment the following entry in the
`         <EI_HOME/conf/log4j.properties        ` file to enable wire
logs for the passthrough HTTP transport:
`         log4j.logger.org.apache.synapse.transport.http.wire=DEBUG        `

!!! info

Callout mediator uses the Axis2 `         CommonsHTTPSender        ` to
invoke services. It does not leverage the non-blocking NHTTP/passthrough
transports. Therefore, you need to add the following entries to the
`         <EI_HOME/conf/log4j.properties        ` file to enable wire
logs for the callout mediator.

  

``` text
    log4j.logger.httpclient.wire.header=DEBUG
    log4j.logger.httpclient.wire.content=DEBUG
```
    

Following is a sample wirelog.

``` text
    [2013-09-22 19:47:57,797] DEBUG - wire >> "POST /services/StockQuoteProxy HTTP/1.1[\r][\n]"
    [2013-09-22 19:47:57,798] DEBUG - wire >> "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:57,798] DEBUG - wire >> "SOAPAction: "urn:getQuote"[\r][\n]"
    [2013-09-22 19:47:57,799] DEBUG - wire >> "User-Agent: Axis2[\r][\n]"
    [2013-09-22 19:47:57,799] DEBUG - wire >> "Host: localhost:8280[\r][\n]"
    [2013-09-22 19:47:57,799] DEBUG - wire >> "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:57,800] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:57,800] DEBUG - wire >> "215[\r][\n]"
    [2013-09-22 19:47:57,800] DEBUG - wire >> "http://localhost:8280/services/StockQuoteProxyurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f67urn:getQuoteIBM[\r][\n]"
    [2013-09-22 19:47:57,801] DEBUG - wire >> "0[\r][\n]"
    [2013-09-22 19:47:57,801] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:57,846]  INFO - TimeoutHandler This engine will expire all callbacks after : 120 seconds, irrespective of the timeout action, after the specified or optional timeout
    [2013-09-22 19:47:57,867] DEBUG - wire << "POST /services/SimpleStockQuoteService HTTP/1.1[\r][\n]"
    [2013-09-22 19:47:57,867] DEBUG - wire << "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:57,867] DEBUG - wire << "SOAPAction: "urn:getQuote"[\r][\n]"
    [2013-09-22 19:47:57,867] DEBUG - wire << "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "Host: localhost:9000[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "Connection: Keep-Alive[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "User-Agent: Synapse-PT-HttpComponents-NIO[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "215[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "http://localhost:8280/services/StockQuoteProxyurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f67urn:getQuoteIBM[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "0[\r][\n]"
    [2013-09-22 19:47:57,869] DEBUG - wire << "[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "HTTP/1.1 200 OK[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Date: Sun, 22 Sep 2013 14:17:57 GMT[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Connection: Keep-Alive[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:58,014] DEBUG - wire << "HTTP/1.1 200 OK[\r][\n]"
    [2013-09-22 19:47:58,015] DEBUG - wire << "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:58,015] DEBUG - wire << "Date: Sun, 22 Sep 2013 14:17:58 GMT[\r][\n]"
    [2013-09-22 19:47:58,015] DEBUG - wire << "Server: WSO2-PassThrough-HTTP[\r][\n]"
    [2013-09-22 19:47:58,016] DEBUG - wire << "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:58,016] DEBUG - wire << "[\r][\n]"
    [2013-09-22 19:47:58,016] DEBUG - wire >> "4d8[\r][\n]"
    [2013-09-22 19:47:58,017] DEBUG - wire >> "urn:getQuoteResponseurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f673.827143922330303-8.819296796724336-170.50810412063595170.73218944560944Sun Sep 22 19:47:57 IST 2013-170.472077024782785.562077973231586E7IBM Company178.0616712932281324.9438904049222641.9564266653777567195.61908401976004IBM6216[\r][\n]"
    [2013-09-22 19:47:58,017] DEBUG - wire >> "0[\r][\n]"
    [2013-09-22 19:47:58,018] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:58,021] DEBUG - wire << "4d8[\r][\n]"
    [2013-09-22 19:47:58,022] DEBUG - wire << "urn:getQuoteResponseurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f673.827143922330303-8.819296796724336-170.50810412063595170.73218944560944Sun Sep 22 19:47:57 IST 2013-170.472077024782785.562077973231586E7IBM Company178.0616712932281324.9438904049222641.9564266653777567195.61908401976004IBM6216[\r][\n]"
    [2013-09-22 19:47:58,022] DEBUG - wire << "0[\r][\n]"
    [2013-09-22 19:47:58,022] DEBUG - wire << "[\r][\n]
```

There are two incoming messages and two outgoing messages in the above
log. First part of the wire logs of a message contains the HTTP headers
and it is followed by the message payload. You need to identify the
message direction as shown below to read wire logs.

-   `           DEBUG - wire >>          ` - This represents a message,
    which is coming into WSO2 EI from the wire

-   `           DEBUG - wire <<          ` - This represents a message,
    which goes to the wire from WSO2 EI

#### Viewing wire logs of a specific mediator

You need to put a debug point to the mediator, to view wire logs of
it. When debugging is finished (or while debugging), right click on the
mediator, and click **Show WireLogs** , to view wire logs for a specific
mediator.

!!! info

You can only view wire logs for **a whole proxy service, call mediator,
send mediator, or other API resources** . However, you cannot view a
wire log of a Synapse config (e.g. sequences), because there would not
be anything written to wire, when the flow comes to the sequence etc.
Hence, you can only view them in wire entry points.


![](attachments/119131423/119131424.png?effects=drop-shadow){width="400"}

##### Viewing wire logs while debugging

If you view wire logs while debugging, you view only the wire logs of
mediators, whose execution is already completed as shown in the example
below.

![view wire logs while
debugging](attachments/119131423/119131451.png "view wire logs while debugging"){width="800"}

##### Viewing wire logs of a mediator after debugging execution of it

When you view wire logs of a mediator (e.g. send mediator) after
debugging, you can view the request and response wire logs as shown in
the example below.

![view wire logs of a mediator after
debugging](attachments/119131423/119131450.png "view wire logs of a mediator after debugging"){width="800"}

##### Viewing wire logs of a proxy service after debugging

If you view wire logs of a proxy service after debugging finished, you
view the request wire log and final response wire log of that proxy as
shown in the example below.

![view wire logs of a proxy after
debugging](attachments/119131423/119131449.png "view wire logs of a proxy after debugging"){width="800"}  
