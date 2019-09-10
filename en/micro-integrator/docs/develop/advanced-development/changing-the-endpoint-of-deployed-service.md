# Changing the Endpoint of a Deployed Proxy Service

The below sections describe how you can change the endpoint reference of
a deployed proxy service without changing its own configuration. For
example, in this scenario, you have two endpoints to manage two
environments (i.e., Dev and QA). The endpoint URLs for the services
hosted in the Dev and QA environments respectively are as follows:

-   Dev environment:
    [http://localhost:8280/services/echo](https://www.google.com/url?q=http://localhost:8280/services/echo&sa=D&source=hangouts&ust=1533987796246000&usg=AFQjCNHGkW_-21LrrGTq7bZTCOqRn_23uw)

-   QA environment:
    [http://localhost:8281/services/echo](https://www.google.com/url?q=http://localhost:8280/services/echo&sa=D&source=hangouts&ust=1533987796246000&usg=AFQjCNHGkW_-21LrrGTq7bZTCOqRn_23uw)


## Creating the endpoint reference projects

Follow the steps below to create two ESB Config Projects containing the
two endpoint values for the Dev and QA environments.

1.  Open **WSO2 Integration Studio** and click **Miscelleneous → Create
    New Config **Project**** in the **Getting Started** tab.  
    ![](attachments/119130841/119133600.png){width="800" height="397"}

2.  In the **New ESB Project** dialog that opens, select **New ESB
    Config Project** and click **Next** .

3.  Enter **HelloWorldDevResources** as the project name and click
    **Finish** .

4.  Right-click the **HelloWorldResources** project in the **Project
    Explorer** and go to **New -\> Endpoint** .
5.  Select **Create a New Endpoint** and click **Next** .

6.  Fill in the information as in the table below and click **Finish** .

    | Field         | Value                                                                                                                                                                                    |
    |---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Endpoint Name | HelloWorldEP                                                                                                                                                                             |
    | Endpoint Type | Address Endpoint                                                                                                                                                                         |
    | Address       | [http://localhost:8280/services/echo](https://www.google.com/url?q=http://localhost:8280/services/echo&sa=D&source=hangouts&ust=1533987796246000&usg=AFQjCNHGkW_-21LrrGTq7bZTCOqRn_23uw) |

7.  Similarly, to create resources for the QA environment, create
    another ESB Config project named **HelloWorldQAResources** and
    create an endpoint named **HelloWorldEP** . Provide the following
    endpoint address:  
    [http://localhost:8280/services/echo](https://www.google.com/url?q=http://localhost:8280/services/echo&sa=D&source=hangouts&ust=1533987796246000&usg=AFQjCNHGkW_-21LrrGTq7bZTCOqRn_23uw)

## Creating the proxy service

In this section, you will create an ESB Solutions Project containing the
Proxy Service configuration.

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create
    New ** in the **Getting Started** tab as shown below.

    ![](attachments/119130841/119130843.png){width="640" height="250"}

2.  In the **New ESB Solution Project** dialog that opens, enter
    **HelloWorldServices** as the project name and click **Finish** .

3.  Right-click the **HelloWorldServices** project in the **Project
    Explorer** and go to **New -\> Proxy Service.**

4.  In the **New Proxy Service** dialog that opens, select **Create a
    New Proxy Service** and click **Next** . Fill in the details as
    specified in the table below:

    | Field              | Value                                                                                              |
    |--------------------|----------------------------------------------------------------------------------------------------|
    | Proxy Service Name | HelloWorldProxy                                                                                    |
    | Proxy Service Type | Select Pass Through Proxy                                                                          |
    | Endpoint           | Select HelloWorldEP (You need to select **Predefined Endpoint** from the endpoint options listed.) |

The projects setup is now complete. You now need to create CApp projects
for each of the Composite Applications you want to generate. The ESB
proxy service and the Dev endpoint must go in its own CApp, and you
create a separate CApp for the ESB proxy service and the QA endpoint.

## Creating the composite application project

1.  Right-click the **Project Exporer** and go to **File -\> New -\>
    Composite Application Project** .
2.  Name the project **HelloWorldDevCApp** .
3.  Select the **HelloWorldServices** and **HelloWorldDevResources**
    projects from the list of dependancies, and click **Finish** .  
    ![](attachments/119130841/119130852.png)
4.  Similarly, create **HelloWorldQACApp** containing the projects
    **HelloWorldServices** and **HelloWorldQAResources** .

Your CApp projects are now ready to be deployed to your ESB servers.

## Deploying the Dev composite application

See [Running the ESB profile via WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaTooling)
for instructions on deploying the applications and starting the server.

## Testing the Dev environment

1.  Log in to the management console using admin as the username and
    password.
2.  Click **Services \> List** , and click on the **Try This Service**
    link for the **HelloWorldProxy** , which you just deployed using the
    CApp.  
    ![](attachments/119130841/119130851.png)
3.  Use the following request to invoke the service:

    ``` xml
        <body>
          <p:echoInt xmlns:p="http://echo.services.core.carbon.wso2.org">
             <!--0 to 1 occurrence-->
             <in>50</in>
          </p:echoInt>
        </body>
    ```

    ![](attachments/119130841/119130850.png)

4.  Click **Send** . You view the response from the **HelloWorldProxy**
    hosted in WSO2 EI as seen in the image below.  
    ![](attachments/119130841/119130849.png)

## Changing the endpoint reference

Follow the steps below to change the endpoint reference of the
**HelloWorldProxy** you deployed, to point it to the QA environment,
without changing its configuration.

1.  Set a port offset by changing the following configuration in the
    `           <EI_HOME>/conf/carbon.xml          ` file.

    ``` xml
             <Ports>
                    <Offset>1</Offset>
            ........
        
        
            </Ports>
    ```

2.  Undeploy the **HelloWorldDevCApp,** deploy the **HelloWorldQACApp**
    and re-start the ESB Profile of WSO2 EI. For instructions, see
    [Running the ESB profile via WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaTooling)
    .  
    ![](attachments/119130841/119130846.png)

## Testing the QA environment

1.  Log in to the Management Console using admin as the username and
    password.
2.  Click **Services \> List** , and click on the **Try This Service**
    link for the **HelloWorldProxy** , which you just deployed using the
    CApp.

        !!! info
    
        If you click on the **HelloWorldProxy** in the **Deployed Services**
        list, you view that the port offset has been applied and the
        endpoint URL has been changed to point to the QA environment.
    
    ![](attachments/119130841/119130851.png)

3.  Use the following request to invoke the service:

    ``` xml
        <body>
          <p:echoInt xmlns:p="http://echo.services.core.carbon.wso2.org">
             <!--0 to 1 occurrence-->
             <in>100</in>
          </p:echoInt>
        </body>
    ```

    ![](attachments/119130841/119130847.png)

4.  Click **Send** . You view the response from the **HelloWorldProxy**
    hosted in WSO2 EI as seen in the image below.

    ![](attachments/119130841/119130845.png)

## Changing an endpoint reference

Once the endpoint has been created, you can update it using any one of
the options listed below. The options below describe how you can update
the endpoint value for QA environment.

### Option 1: Using WSO2 Integration Studio

1.  Open the `          HelloWorldEP.xml         ` file under
    **HelloWorldQAResources** project and replace the URL with the QA
    URL.
2.  Save all changes.

Your CApp can be deployed to your QA EI server. For details on how to
deploy the CApp project, see [Running the ESB profile via WSO2
Integration
Studio](https://docs.wso2.com/display/EI650/Running+the+Product#RunningtheProduct-RunningtheESBprofileviaWSO2IntegrationStudio)
.

### Option 2: From Command Line

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

!!! Note
    -   To build the projects using the above commands, you need an active network connection.
    -   Creating a Maven Multi Module project that contains the above projects, allows you to projects in one go by simply building the parent Maven Multi Module project.

### Option 3: Using a Script

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

