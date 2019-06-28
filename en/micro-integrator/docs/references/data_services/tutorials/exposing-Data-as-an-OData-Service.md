# Exposing Data as an OData Service

In this tutorial, we will run through the process of exposing and RDBMS
as an OData service. When OData is enabled for a datasource, you do not
need to manually define CRUD operations. They are automatically
created.  

-   [Setting up an
    RDBMS](#ExposingDataasanODataService-SettingupanRDBMS)
-   [Expose the RDBMS as an OData
    service](#ExposingDataasanODataService-ExposetheRDBMSasanODataservice)
-   [Access the data service using CRUD
    operations](#ExposingDataasanODataService-AccessthedataserviceusingCRUDoperations)

!!! note

Note that the OData feature can only be used for RDBMS, Cassandra, and
MongoDB datasources .


------------------------------------------------------------------------

### Setting up an RDBMS

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
4.  Create a MySQL database named `           CompanyAccounts          `
    .  

    ``` java
        CREATE DATABASE CompanyAccounts;
    ```

5.  Create a table in the `           CompanyAccounts          `
    database as follows.

    ``` java
            CREATE TABLE ACCOUNT(AccountID int NOT NULL,Branch varchar(255) NOT NULL, AccountNumber varchar(255),AccountType ENUM('CURRENT', 'SAVINGS') NOT NULL,
            Balance FLOAT,ModifiedDate DATE,PRIMARY KEY (AccountID)); 
    ```

6.  Enter the following data into the table:  

    ``` java
            INSERT INTO ACCOUNT VALUES (1,"AOB","A00012","CURRENT",231221,'2014-12-02');
    ```

------------------------------------------------------------------------

### Expose the RDBMS as an OData service

Follow the steps given below.

1.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#215e61b38df14717aec6d3dbd798d961)
    -   [**On Windows**](#0ad841878e4742f8bda752d2493f2904)

    Open a terminal and execute the following command:

    ``` java
            sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Open the ESB profile's Management Console using
    `                     https://localhost:9443/carbon                   `
    , and log in using `          admin         ` as the username and
    the password.
3.  Click **Create** under **Data Service** .

4.  Enter `           ExposeMySQLDatabaseService          ` as the name
    of the data service, and click **Next** .

5.  Click **Add New Datasource** and enter details for the new
    datasource as follows.  
    ![](attachments/119133302/119133303.png){width="655"}

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>Datasource</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>RDBMS</td>
    </tr>
    <tr class="odd">
    <td>Datasource Type (Default/External)</td>
    <td>Leave <strong>Default</strong> selected.</td>
    </tr>
    <tr class="even">
    <td>Database Engine</td>
    <td>MySQL</td>
    </tr>
    <tr class="odd">
    <td>Driver Class</td>
    <td><code>               com.mysql.jdbc.Driver              </code></td>
    </tr>
    <tr class="even">
    <td>URL</td>
    <td><code>                               jdbc:mysql://localhost:3306/Company                             </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td>Enter your MySQL server's username. Example: root</td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    <tr class="odd">
    <td><strong>Enable OData</strong></td>
    <td>Select this check box.</td>
    </tr>
    </tbody>
    </table>

6.  Click **Finish** to save the data service.

7.  Go to the **Deployed Services** screen and click the data service
    that you created.  

------------------------------------------------------------------------

### Access the data service using CRUD operations

Open a command prompt execute the following CURL commands using CRUD
operations:

!!! note

Note that you should have privileges to perform CRUD operations on the
database. If not, the OData service will not work properly.


-   To get the service document:

    ``` java
        curl -X GET -H 'Accept: application/json' http://localhost:8280/odata/ExposeDatabaseService/Datasource
    ```

-   To get the metadata of the service:

    ``` java
            curl -X GET -H 'Accept: application/xml' http://localhost:8280/odata/ExposeDatabaseService/Datasource/$metadata
    ```

-   To read details from the ACCOUNT table:

    ``` java
            curl -X GET -H 'Accept: application/xml' http://localhost:8280/odata/ExposeDatabaseService/Datasource/ACCOUNT
    ```
