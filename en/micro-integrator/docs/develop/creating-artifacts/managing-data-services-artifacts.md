# Managing Data Integration Artifacts via Tooling

A **data service** provides a Web service interface to access data that
is stored in relational databases, CSV files, Microsoft Excel sheets,
Google spreadsheets, and more. The following sections describe how you
can use WSO2 Integration Studio to work with data services' artifacts.

-   [Prerequisites](#ManagingDataIntegrationArtifactsviaTooling-Prerequisites)
-   [Creating a data
    service](#ManagingDataIntegrationArtifactsviaTooling-Creatingadataservice)
    -   [Step 1: Creating a data service
        project](#ManagingDataIntegrationArtifactsviaTooling-Step1:Creatingadataserviceproject)
    -   [Step 2: Creating
        the datasource connection](#ManagingDataIntegrationArtifactsviaTooling-Step2:Creatingthedatasourceconnection)
    -   [Step 3: Creating a
        query](#ManagingDataIntegrationArtifactsviaTooling-Step3:Creatingaquery)
    -   [Step 4: Creating a resource to invoke the
        query](#ManagingDataIntegrationArtifactsviaTooling-Step4:Creatingaresourcetoinvokethequery)
    -   [Step 5: Testing the data
        service](#ManagingDataIntegrationArtifactsviaTooling-Step5:Testingthedataservice)
-   [Importing a data
    service](#ManagingDataIntegrationArtifactsviaTooling-Importingadataservice)
-   [Applying security to a data
    service](#ManagingDataIntegrationArtifactsviaTooling-secure_data_serviceApplyingsecuritytoadataservice)
    -   [Step 1: Creating a registry resource
        project](#ManagingDataIntegrationArtifactsviaTooling-Step1:Creatingaregistryresourceproject)
    -   [Step 2: Creating a security policy as a registry
        resource](#ManagingDataIntegrationArtifactsviaTooling-Step2:Creatingasecuritypolicyasaregistryresource)
    -   [Step 3: Adding the security policy to the data
        service](#ManagingDataIntegrationArtifactsviaTooling-Step3:Addingthesecuritypolicytothedataservice)
    -   [Step 4: Testing the data service with the security
        policy](#ManagingDataIntegrationArtifactsviaTooling-Step4:Testingthedataservicewiththesecuritypolicy)
-   [Creating a custom
    validator](#ManagingDataIntegrationArtifactsviaTooling-Creatingacustomvalidator)
    -   [Creating a new custom
        validator](#ManagingDataIntegrationArtifactsviaTooling-Creatinganewcustomvalidator)
    -   [Importing a validator
        project](#ManagingDataIntegrationArtifactsviaTooling-Importingavalidatorproject)
-   [Using an encrypted datasource
    password](#ManagingDataIntegrationArtifactsviaTooling-Usinganencrypteddatasourcepassword)
    -   [Step 1: Encrypting the datasource
        password](#ManagingDataIntegrationArtifactsviaTooling-Step1:Encryptingthedatasourcepassword)
    -   [Step 2: Using the encrypted password in the data
        service](#ManagingDataIntegrationArtifactsviaTooling-Step2:Usingtheencryptedpasswordinthedataservice)
    -   [Step 5: Testing the data
        service](#ManagingDataIntegrationArtifactsviaTooling-Step5:Testingthedataservice.1)

### Prerequisites

-   Install WSO2 Integration Studio using the instructions in
    [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
    .
-   To demonstrate how data services work, we will use a MySQL database
    as the datasource. Follow the steps given below to set up a MySQL
    database:

    1.  Install the MySQL server.
    2.  Download the JDBC driver for MySQL from
        [here](http://dev.mysql.com/downloads/connector/j/) and copy it
        to your `             <EI_HOME>/lib            ` directory.

                !!! note
        
                If the driver class does not exist in the relevant folders when
                you create the datasource, you will get an exception such as '
                `             Cannot load JDBC driver class com.mysql.jdbc.Driver            `
                '.
        

    3.  Create a database named `             Employees            ` .

        ``` java
                CREATE DATABASE Employees;
        ```

    4.  Create the Employee table inside the Employees database:

        ``` java
                    USE Employees;
                    CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
        ```

### Creating a data service

Follow the steps given below to create a new data service.

#### Step 1: Creating a data service project

All the data services' artifacts that you create should be stored in a
Data Service project. Follow the steps given below to create a project:

1.  Open **WSO2 Integration Studio,** and click **DS Project → Create
    New ** in the **Getting Started** tab as shown below.  
    ![](attachments/119130577/119135178.png){width="750"}

2.  In the **New Data Service Project** dialog that opens, give a name
    for the project and click **Next** .
3.  If required, change the Maven information about the project.
4.  Click **Finish** . The new project will be listed in the project
    explorer.

#### Step 2: Creating the datasource connection

Follow the steps given below to create the data service file:

1.  Select the already created **Data Service Project** in the project
    navigator, right click and go to **New -\> Data Service** .  
    The **New Data Service** window will open as shown below.  
    ![](attachments/119130577/119130578.png){width="400" height="396"}
2.  To start creating a data service from scratch, select **Create New
    Data Service** and click **Next** .

3.  Enter a name for the data service:

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 49%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Data Service Name</td>
    <td>RDBMSDataService</td>
    </tr>
    </tbody>
    </table>

4.  Click **Next** and start adding the datasource connection details
    given below.

    |                                    |                                       |
    |------------------------------------|---------------------------------------|
    | Datasource ID                      | Datasource                            |
    | Datasource Type                    | RDBMS                                 |
    | Datasource Type (Default/External) | Leave **Default** selected.           |
    | Database Engine                    | MySQL                                 |
    | Driver Class                       | com.mysql.jdbc.Driver                 |
    | URL                                | jdbc:mysql://localhost:3306/Employees |
    | User Name                          | root                                  |

5.  Save the data service.

A data service file (DBS file) will now be created in your data service
project. Shown below is the project directory.

![](attachments/119130577/119130593.png){width="550" height="452"}

#### Step 3: Creating a query

Let's write an SQL query to GET data from the MySQL datasource that you
configured in the previous step:

1.  Select the data service you created in the previous step.
2.  Right-click and click **Add Query** .  
    ![](attachments/119130577/119130591.png){width="780" height="250"}
3.  Enter the following query details:

    |            |                    |
    |------------|--------------------|
    | Query ID   | GetEmployeeDetails |
    | Datasource | Datasource         |

4.  Save the query. The query element is now added to the data
    service:  
    ![](attachments/119130577/119130590.png){width="800" height="320"}
5.  Right-click the **GetEmployeeDetails** query and click **Add SQL**
    to add the following SQL statement:

    ``` java
            select EmployeeNumber, FirstName, LastName, Email from Employees where EmployeeNumber=:EmployeeNumber
    ```

6.  Save the SQL statement.
7.  Right-click the query again and click **Add Input Mapping** .

8.  Enter the following input mapping details:

    |                |                |
    |----------------|----------------|
    | Mapping Name   | EmployeeNumber |
    | Parameter Type | SCALAR         |
    | SQL Type       | STRING         |

9.  Save the input mapping.
10. Right-click the query again and click **Add Output Mapping** .

11. Enter the following value to group the output mapping:

    <table>
    <colgroup>
    <col style="width: 36%" />
    <col style="width: 63%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Grouped by Element</td>
    <td>Employees</td>
    </tr>
    </tbody>
    </table>

12. Save the output mapping.

13. Right-click the output mapping and go to **Add Output Mapping → Add
    Element** to create an element.

14. Enter the following element details.

    <table>
    <colgroup>
    <col style="width: 39%" />
    <col style="width: 60%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Datasource Type</td>
    <td>column</td>
    </tr>
    <tr class="even">
    <td>Output Field Name</td>
    <td>EmployeeNumber</td>
    </tr>
    <tr class="odd">
    <td>Datasource Column Name</td>
    <td>EmployeeNumber</td>
    </tr>
    <tr class="even">
    <td>Schema Type</td>
    <td>String</td>
    </tr>
    </tbody>
    </table>

15. Save the element.
16. Follow the same steps to create the following output elements:

    | Datasource Type | Output Field Name | Datasource Column Name | Schema Type |
    |-----------------|-------------------|------------------------|-------------|
    | column          | FirstName         | FirstName              | string      |
    | column          | LastName          | LastName               | string      |
    | column          | Email             | Email                  | string      |

17. Save the output elements.

The data service should now have the query element added as shown below.

![](attachments/119130577/119130589.png){width="1300" height="278"}

#### Step 4: Creating a resource to invoke the query

Now, let's create a REST resource that can be used to invoke the query.

1.  Right-click the data service and click **Add Resource** . Add the
    following resource details.

    <table>
    <colgroup>
    <col style="width: 45%" />
    <col style="width: 54%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Resource Method</td>
    <td>GET</td>
    </tr>
    <tr class="even">
    <td>Resource Path</td>
    <td>Employee/{EmployeeNumber}</td>
    </tr>
    </tbody>
    </table>

2.  Expand the GET resource, and click the **GetEmployeeDetails
    (call-query)** . Connect the query to the resource by adding the
    following:

    <table>
    <colgroup>
    <col style="width: 43%" />
    <col style="width: 56%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>GetEmployeeDetails</td>
    </tr>
    </tbody>
    </table>

3.  Save the resource.

The data service should now have the resource added as shown below.

![](attachments/119130577/119130588.png){width="900" height="326"}

#### Step 5: Testing the data service

You can test the data service artifacts by following the steps given
below.

1.  Package the data service file (.dbs file) into a **Composite
    Application (CApp)** . See the instructions in [Packaging Artifacts
    into Composite
    Applications](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
    .
2.  Add your WSO2 EI product instance to WSO2 Integration Studio, and
    deploy the CApp in the server. See the instructions in [Deploying
    Composite Applications in the
    Server](https://docs.wso2.com/display/ADMIN44x/Deploying+Composite+Applications+in+the+Server)
    .
3.  Start the ESB profile. The data service is now deployed in WSO2 EI.
4.  Send a GET request to invoke the service. You can use **curl** as
    shown below.

    ``` java
            curl -X GET http://localhost:8280/services/RDBMSDataService.HTTPEndpoint/Employee/3
    ```

    You will receive the employee's details in the response.

### Importing a data service

Follow these steps to import an existing data service descriptor file
(DBS file) to a **Data Service project** in WSO2 Integration Studio.
Alternatively, you can [create a new data
service](#ManagingDataIntegrationArtifactsviaTooling-Creatingadataservice)
.

1.  Select the **Data Service Project** in the project navigator, right
    click, and go to **New -\> Data Service** .
2.  Select **Import Data Service** and click **Next** .
3.  Specify the data service descriptor (DBS) file by typing its full
    pathname or by clicking **Browse** and navigating to the file.

        !!! tip
    
        You can test this using the [sample data service
        file](attachments/119130577/119130595.dbs) .
    

4.  Optionally, specify the location and working sets for this project.
5.  A Maven POM file will be generated automatically for this project.
    If you want to customize the Maven options (such as including parent
    POM information in the file from another project in this workspace),
    click **Next** and specify the options.
6.  Click **Finish** . The data service is added and is open in the
    editor. You can now right-click the data service in the outline and
    add queries, operations, additional data sources, and so on.

### Applying security to a data service

WSO2 supports WS-Security, WS-Policy, and WS-Security Policy
specifications. These specifications define a behavioral model for Web
services. To enable a security policy for a data service, you need to
first create a security policy file, and then add it to the data
service.

#### Step 1: Creating a registry resource project

Registry artifacts (such as security policy files) should be stored in a
**Registry Resource** project. Follow the steps given below to create a
project:

1.  Open **WSO2 Integration Studio** and click **Miscelleneous → Create
    New Registry **Project**** in the **Getting Started** tab as shown
    below.

    ![](attachments/119130577/119135181.png){width="750" height="371"}

2.  Enter a name for the project and click **Next** .
3.  Enter the Maven information about the project and click **Finish** .
4.  The new project will be listed in the project explorer.

#### Step 2: Creating a security policy as a registry resource

1.  Right-click the registry resource project in the left navigation
    panel, click **New** , and then click **Registry Resource** . This
    will open the **New Registry Resource** window.
2.  Select the **From existing template** option as shown below and
    click **Next** .  
    ![](attachments/119130577/119130583.png){width="500" height="535"}
3.  Enter the following details:

    |               |                |
    |---------------|----------------|
    | Resource Name | Sample\_Policy |
    | Artifact Name | Sample\_Policy |
    | Template      | WS-Policy      |
    | Registry      | gov            |
    | Registry path | ws-policy/     |

4.  Click **Finish** and the policy file will be listed in the
    navigator.
    1.  Let's use the **Design View** to enable the required security
        scenario. For example, enable the **Sign and Encyrpt** security
        scenario as shown below.

                !!! tip
        
                Click the icon next to the scenario to get details of the
                scenario.
        

          
        ![](attachments/119130577/119130596.png){width="600"}

    2.  You can also provide encryption properties, signature
        properties, and advanced rampart configurations as shown below.

        -   [**Encryption/Signature
            Properties**](#264fd3cfb5784aec8eeedfa715ebbe1b)
        -   [**Rampart Properties**](#aa02a1b5372144a78b4d30cb68dceac9)

        ![](attachments/119130577/119130620.png){width="320"
        height="374"}

        ![](attachments/119130577/119130621.png){width="530"
        height="400"}

                !!! info
        
                Using role-based permissions?
        
                For certain scenarios, you can specify user roles. After you
                select the scenario, scroll to the right to see the **User
                Roles** button.  
                ![](attachments/119130577/119130622.png){width="650"
                height="415"}
        
                Either define the user roles inline or retrieve the user roles
                from the server.
        
                -   [**Define Inline**](#079371f083fd4a6f9a14e555e1e23e2a)
                -   [**Get from the server**](#1b700b3bca434259a9ad3d9e90dccbb3)
        
                ![](attachments/119130577/119130605.png){width="519"
                height="376"}
        
                ![](attachments/119130577/119130606.png){width="517"
                height="375"}
        

5.  Save the policy file.

#### Step 3: Adding the security policy to the data service

Once you have configured the policy file, you can add the security
policy to the data service as explained below.

1.  If you have already created a data service using WSO2 Integration
    Studio, select the file from the Project Explorer. Alternatively,
    you can download this [sample data service
    file](attachments/119130577/119130595.dbs) and import it to your
    development environment.

        !!! tip
    
        Be sure to update your database credentials in the dataservice file.
    

2.  Once you have select the data service file, click the **browse**
    icon for the **Policy** field.

    ![](attachments/119130577/119130582.png){width="900" height="348"}

3.  Click **workspace** , to add the security policy from the current
    workspace. You can select the path to the
    `          sample_policy.         ` xml file that you created in the
    previous steps.  
    ![](attachments/119130577/119130581.png){width="900" height="350"}
4.  Click **OK** , and the security policy will be added to the data
    service.  
    ![](attachments/119130577/119130579.png){width="900" height="349"}
5.  Save the data service.

#### Step 4: Testing the data service with the security policy

Follow the steps given below.

1.  Package the data service file (.dbs file) and the security policy
    file into a **Composite Application (CApp)** . See the instructions
    in [Packaging Artifacts into Composite
    Applications](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
    .

        !!! warning
    
        Once you create the CApp project, note that the information about
        each of the projects and artifacts that you packaged into the CApp
        will be listed (under **Dependencies** in the **Composite
        Application Project POM Editor** ).
    
        **Be sure** to set the server role to **Enterprise Service Bus** for
        the security policy file as shown below
    
        **![](attachments/119130577/119130610.png){width="886"
        height="222"}**
    

2.  Add your WSO2 EI product instance to WSO2 Integration Studio, and
    deploy the CApp in the server. See the instructions in [Deploying
    Composite Applications in the
    Server](https://docs.wso2.com/display/ADMIN44x/Deploying+Composite+Applications+in+the+Server)
    .
3.  Start the ESB profile. The data service and the security policy is
    now deployed in WSO2 EI.

### Creating a custom validator

An **input validator** allows a data service to validate the input
parameters in a request and stop the execution of the request if the
input doesn’t meet the required criteria. In addition to the default
validators provided, you can create your own custom validators by
creating a Java class that implements the
`         org.wso2.carbon.dataservices.core.validation.Validator        `
interface. You can [create a new custom
validator](#ManagingDataIntegrationArtifactsviaTooling-Creatinganewcustomvalidator)
or [import an existing validator
project](#ManagingDataIntegrationArtifactsviaTooling-Importingavalidatorproject)
.

#### Creating a new custom validator

Follow these steps to create a new custom validator. Alternatively, you
can [import an existing validator
project](#ManagingDataIntegrationArtifactsviaTooling-Importingavalidatorproject)
.

1.  Go to **File-\> New -\> Other -\> Data Services Validator Project**
    to open the **New Data Services Validated Artifact Creation Wizard**
    .
2.  Select **Create New Data Services Validator Project** and click
    **Next** .
3.  Type a unique name for the project and specify the package and class
    name for this validator.
4.  Optionally, specify the location and working set for this project.
5.  A Maven POM file will be generated automatically for this project.
    If you want to customize the Maven options (such as including parent
    POM information in the file from another project in this workspace),
    click **Next** and specify the options.
6.  Click **Finish** . The project is created, and the new validator
    class is open in the editor where you can add your validation logic.

#### Importing a validator project

Follow these steps to import an existing custom validator project.
Alternatively, you can [create a new custom
validator](#ManagingDataIntegrationArtifactsviaTooling-Creatinganewcustomvalidator)
.

1.  Go to **File-\> New -\> Other -\> Data Services Validator Project**
    to open the **New Data Services Validated Artifact Creation Wizard**
    .
2.  Select **Import Project From Workspace** and click **Next** .
3.  Select the existing validator project, and optionally specify the
    location and working sets for the new project.
4.  A Maven POM file will be generated automatically for this project.
    If you want to customize the Maven options (such as including parent
    POM information in the file from another project in this workspace),
    click **Next** and specify the options.
5.  Click **Finish** . The project is imported, and the validator class
    is open in the editor, where you can modify the validation logic as
    needed.

### Using an encrypted datasource password

When you create a data service for an RDBMS datasource, you have the
option of encrypting the datasource connection password. This ensures
that the password is encrypted in the configuration file (.dbs file) of
the data service.

#### Step 1: Encrypting the datasource password

Any plain text password can be encrypted using the Cipher Tool that is
shipped with your WSO2 product. This tool uses the secure vault
implementation in the product to perform the encryption.

1.  Start by initiating the Cipher Tool. Execute the following command
    from the `          <EI_HOME>/bin         ` directory.  
    -   Linux: `            sh           ` ciphertool
        `            .sh -           ` Dconfigure
    -   Windows: ciphertool.bat -Dconfigure
2.  To encrypt your password, execute the same command without
    -Dconfigure as shown below.  
    -   Linux: sh ciphertool.sh
    -   Windows: ciphertool.bat

    This command will prompt you to enter the KeyStore Password of the
    running Carbon instance. The default is
    `          wso2carbon         ` .
3.  Specify the plain text password that is used to log in to your
    database and execute the command. The encrypted password will be
    returned.

        !!! tip
    
        Be sure to use the password to the RDBMS that is used in your data
        service.
    

4.  Add the encrypted password and an alias to the
    `           cipher-text.properties          ` file (stored in the
    `           <PRODUCT_HOME>/repository/conf/security/          `
    directory) as shown below. Note that you can use a name of your
    preference for the password alias. In this example, we have used
    `           DB_Password_Alias          ` .

    ``` java
        DB_Password_Alias=<encrypted_value>
    ```

5.  Save the file.

#### Step 2: Using the encrypted password in the data service

Once you have encrypted the datasource password, you can update the data
service as explained below.

1.  Select the data service from the project explorer, right click and
    go to **-\> Open With -\> Text Editor** . This will open the data
    service in text form.

        !!! tip
    
        If you don't have an already created data service in WSO2
        Integration Studio, you can [create a new data
        service](#ManagingDataIntegrationArtifactsviaTooling-Creatingadataservice)
        or [import an existing data
        service](#ManagingDataIntegrationArtifactsviaTooling-Importingadataservice)
        file.
    

2.  Update the datasource configuration by adding the secret alias of
    the password. See the example given below.

    ``` java
        <config id="Datasource">
                <property name="org.wso2.ws.dataservice.user">root</property>
                <property name="org.wso2.ws.dataservice.password" svns:secretAlias="DB_Password_Alias"></property>
                <property name="org.wso2.ws.dataservice.protocol">jdbc:mysql://localhost:3306/Employees</property>
                <property name="org.wso2.ws.dataservice.driver">com.mysql.jdbc.Driver</property>
                <property name="org.wso2.ws.dataservice.minpoolsize"/>
                <property name="org.wso2.ws.dataservice.maxpoolsize"/>
                <property name="org.wso2.ws.dataservice.validation_query"/>
        </config>
    ```

3.  Add the "http://org.wso2.securevault/configuration" namespace
    configuration to the datasource as follows:

    ``` java
            <data enableBatchRequests="true" name="RDBMSDataService" serviceNamespace="http://org.wso2.securevault/configuration">
    ```

4.  Save the data service file.

#### Step 5: Testing the data service

You can test the data service artifacts by following the steps given
below.

1.  Package the data service file (.dbs file) into a **Composite
    Application (CApp)** . See the instructions in [Packaging Artifacts
    into Composite
    Applications](https://docs.wso2.com/display/ADMIN44x/Packaging+Artifacts+into+Composite+Applications)
    .
2.  Add your WSO2 EI product instance to WSO2 Integration Studio and
    deploy the CApp in the server. See the instructions in [Deploying
    Composite Applications in the
    Server](https://docs.wso2.com/display/ADMIN44x/Deploying+Composite+Applications+in+the+Server)
    .
3.  Start the ESB profile. The data service is now deployed in WSO2 EI.
