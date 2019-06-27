# Working with Endpoints

An **endpoint** defines an external destination for an outgoing message
through WSO2 Enterprise Integrator. Typically, the endpoint is the
address of a proxy service, which acts as the front end to the actual
service. For example, the endpoint for the simple stock quote sample is
`         http:        ` `         //localhost        `
`         :9000        `
`         /services/SimpleStockQuoteService        ` .

For detailed information on each available endpoint type, see [ESB
Endpoints](_ESB_Endpoints_) .

### Configuring endpoints

In the XML configuration, the \<endpoint\> element defines an endpoint
as follows:

``` html/xml
    <endpoint [name="string"] [key="string"]>
            address-endpoint | default-endpoint | wsdl-endpoint | load-balanced-endpoint | fail-over-endpoint
    </endpoint>
```

### Using named endpoints

You can use the `         name        ` attribute to create a named
endpoint. You can reuse a named endpoint by referencing it in another
endpoint using the `         key        ` attribute. For example, if
there is an endpoint named *foo* , you can reference the *foo* endpoint
in any other endpoint where you want to use *foo* :

    <endpoint key="foo"/>

This approach allows you to reuse existing endpoints in multiple places.

### Working with endpoints

You can either use the Enterprise Integrator tooling plug-in to create a
new endpoint and to import an existing endpoint, or you can manage
endpoints via the Enterprise Integrator Management Console. For detailed
information on how to work with endpoints, see [Working with Endpoints
via WSO2 Integration Studio](_Working_with_Endpoints_via_Tooling_) .

### Tracing and handling errors

Endpoints have a `         trace        ` attribute, which turns on
detailed trace information for messages being sent to the endpoint.
These are available in the `         trace.log        ` file, which is
configured in the
`         <PRODUCT_HOME>/repository/conf/log4j.properties        ` file.
Setting the trace log level to `         TRACE        ` logs detailed
trace information including message payloads. For more information on
endpoint states and handling errors, see [Endpoint Error
Handling](_Endpoint_Error_Handling_) .

### Changing an endpoint reference

Once the endpoint has been created, you can update it using any one of
the options listed below. The options below describe how you can update
the endpoint value for QA environment.

#### Option 1: Using WSO2 Integration Studio

1.  Open the `          HelloWorldEP.xml         ` file under
    **HelloWorldQAResources** project and replace the URL with the QA
    URL.
2.  Save all changes.

Your CApp can be deployed to your QA EI server. For details on how to
deploy the CApp project, see [Running the ESB profile via WSO2
Integration
Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaWSO2IntegrationStudio)
.

#### Option 2: From Command Line

1.  Open a Terminal window and navigate to
    `          <ESB_TOOLING_WORKSPACE>/HelloWorldQAResources/src/main/synapse_configendpoints/HelloWorldEP.xml         `
    file.
2.  Edit the HelloWorldEP.xml (e.g. using gedit or vi) under
    HelloWorldResources/QA and replace the URL with the QA one.

    ``` xml
            ...
            <address uri="http://192.168.1.110:9773/services/HelloService/"/>
            ...
    ```

3.  Navigate to
    `           <ESB_TOOLING_WORKSPACE>/HelloWorldQAResources          `
    and build the ESB Config project using the following command:

    ``` xml
            mvn clean install
    ```

4.  Navigate to
    `           <ESB_TOOLING_WORKSPACE>/HelloWorldQACApp          ` and
    build the CApp project using the following command:

    ``` xml
            mvn clean install
    ```

5.  The resulting CAR file can be deployed directly to the QA ESB
    server. For details, see [Running the ESB profile via WSO2
    Integration
    Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaWSO2IntegrationStudio)
    .

!!! note

-   To build the projects using the above commands, you need an active
    network connection.
-   Creating a Maven Multi Module project that contains the above
    projects, allows you to projects in one go by simply building the
    parent Maven Multi Module project.


#### Option 3: Using a Script

Alternatively you can have a CAR file with dummy values for the endpoint
URLs and use a customized shell script or batch script. The script
created would need to do the following:

1.  Extract the CAR file.
2.  Edit the URL values.
3.  Re-create the CAR file with new values.

The resulting CAR file can be deployed directly to the QA ESB server.
For details, see [Running the ESB profile via WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaWSO2IntegrationStudio)
.
