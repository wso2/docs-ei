# Generating a Data Service

This option automatically creates a data service using a given database
structure. When generating a data service, the server takes its table
structure according to the structure specified in the datasource and
automatically creates the `         SELECT        ` ,
`         INSERT        ` , `         UPDATE        ` , and
`         DELETE        ` operations.

-   [Step 1: Setting up a
    datasource](#GeneratingaDataService-Step1:Settingupadatasource)
-   [Step 2: Creating a Carbon
    datasource](#GeneratingaDataService-Step2:CreatingaCarbondatasource)
-   [Step 3: Generating a data
    service](#GeneratingaDataService-Step3:Generatingadataservice)
-   [Step 4: Verifying the generated CRUD operations
    (Optional)](#GeneratingaDataService-Step4:VerifyingthegeneratedCRUDoperations(Optional))
-   [Step 5: Invoking your data
    service](#GeneratingaDataService-Step5:Invokingyourdataservice)

------------------------------------------------------------------------

### Step 1: Setting up a datasource

Follow the steps given below to set up a MySQL database for this
tutorial.

1.  Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>               /Library/WSO2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>               C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\              </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>               /usr/lib/wso2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>               /usr/lib64/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    </tbody>
    </table>

2.  Install the MySQL server.
3.  Download the JDBC driver for MySQL from
    [here](http://dev.mysql.com/downloads/connector/j/) and copy it to
    your `          <EI_HOME>/lib         ` directory.
4.  Create the following database: `           AccountDetails          `

    ``` java
        CREATE DATABASE AccountDetails;
    ```

5.  Create the following table:

    ``` java
            USE AccountDetails;
        
        
            CREATE TABLE ACCOUNT(AccountID int NOT NULL,Branch varchar(255) NOT NULL, AccountNumber varchar(255),AccountType ENUM('CURRENT', 'SAVINGS') NOT NULL,
            Balance FLOAT,ModifiedDate DATE,PRIMARY KEY (AccountID)); 
    ```

6.  Enter the following data into the table:

    ``` java
            INSERT INTO ACCOUNT VALUES (1,"AOB","A00012","CURRENT",231221,'2014-12-02');
    ```

------------------------------------------------------------------------

### Step 2: Creating a Carbon datasource

!!! info

The ESB profile of WSO2 EI comes with the datasource management feature,
which allows users to create any RDBMS datasource or
custom datasource that is used by the server to connect to databases or
external data stores. See
[Managing Datasources](https://docs.wso2.com/display/ADMIN44x/Managing+Datasources)
in the WSO2 Administration Guide for instructions on how to use this
feature. Note that these pre-defined datasources can also be added to a
data service as
[Carbon datasources](_Exposing_a_Carbon_Datasource_as_a_Data_Service_) .


Follow the steps given below.

1.  Start the ESB profile.

    -   [**On MacOS/Linux/CentOS**](#130be383f9004380940b310639fc4439)
    -   [**On Windows**](#dd263ccdf08b4264adabaf38663edbc8)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Access the management console:
    `          https://localhost:9443/cabon         `
3.  Sign in using `          admin         ` as the username and
    password.
4.  Open the **Configure** tab and click **Datasources** .  
    This will open the **Datasources** screen.
5.  Click **Add Datasource** and enter the following details
    corresponding to the `           AccountDetails          ` database:

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource Type</td>
    <td>RDBMS</td>
    </tr>
    <tr class="even">
    <td>Name</td>
    <td>AccountDetails</td>
    </tr>
    <tr class="odd">
    <td>Datasource provider</td>
    <td>Leave <strong>Default</strong> selected.</td>
    </tr>
    <tr class="even">
    <td>Database Engine</td>
    <td>MySQL</td>
    </tr>
    <tr class="odd">
    <td>Driver</td>
    <td><code>               com.mysql.jdbc.Driver              </code></td>
    </tr>
    <tr class="even">
    <td>URL</td>
    <td><code>               jdbc:mysql://localhost:3306/AccountDetails              </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td>Enter your MySQL server's username.</td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    </tbody>
    </table>

    ![](attachments/119130818/119130819.png){width="500" height="290"}

        !!! tip
    
        Want to make sure that your datasource is connected to the database?
        Click **Test Connection** to find out.
    

6.  Save the datasource.

------------------------------------------------------------------------

### Step 3: Generating a data service

Once the datasource is created, follow the steps below to generate a
data service.

1.  Open the Main tab and click **Data Service \>** **Generate** .
2.  In the **Carbon Datasource** field, select the
    `                     AccountDetails                   ` datasource
    from the drop-down list.
3.  In the **Database Name** field, give the name of the database that
    you created, which is
    `                       AccountDetails                     ` .

        !!! note
    
        Note that in most cases the database name should be the same name
        used when creating the datasource. If you want to find the database
        name for the datasource you selected in step 2 above, go to the
        **Configure** tab and click **Datasources** in the navigator. You
        can then get the details (including the database name) of a
        datasource by clicking **View** .  
          
        However, some datasources such as Oracle and H2 may not require the
        exact database name.
    

4.  Click **Next** to open the **Customize Service Generation**
    screen.  
    The tables in the AccountDetails database will be listed. We only
    have one table (ACCOUNT) in the database as shown below.  
    ![](attachments/119130818/119130820.png){width="600" height="142"}
5.  Select the **ACCOUNT** table and click **Next** to open the
    **Service Generation** screen.
6.  You must select a service generation mode from the following two
    options:  
    For this guide select Multiple Services. A data service is created
    for the table in the AccountDetails datasource.  
    -   **Single Service** : Creates a single data service for
        operations of all tables.  
        If this option is selected, you need to provide a name for the
        Data Service you are creating.
    -   **Multiple Services** : Creates a service per table, which will
        contain isolated operations for each table.  
        This is only applicable if you have selected multiple tables in
        the previous step.
7.  Click **Next** to open the **Generated Services** screen. A data
    service by the name of ACCOUNT\_DataService is now generated.
8.  Click **Finish** to complete the data service creation process.  You
    will now be taken to the **Deployed Services** screen, which shows
    all the data services deployed on the server.

------------------------------------------------------------------------

### Step 4: Verifying the generated CRUD operations (Optional)

You can verify that the relevant queries and CRUD operations are
generated accurately:

1.  Click the **ACCOUNT\_DataService** service to open the service
    dashboard.
2.  Click **Edit Data Service (Wizard)** to open the service in the
    **Create New Data Service** wizard.
3.  Click **Next** until you get to the **Queries** screen. The queries
    corresponding to CRUD operations are automatically generated:  
    ![](attachments/119130818/119130821.png){width="739" height="240"}
4.  Click **Next** to get to the **Operations** screen. The CRUD
    operations connected to the queries:  
    ![](attachments/119130818/119130822.png){width="738" height="241"}
5.  Click **Finish** to go back to the service dashboard.

------------------------------------------------------------------------

### Step 5: Invoking your data service

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service** link for the **AccountDetails** data
    service. The TryIt Tool will open with the data service.  
    See that the CRUD operations are generated for the datasource.

3.  Execute the CRUD operations from the TryIt tool to see the result.
