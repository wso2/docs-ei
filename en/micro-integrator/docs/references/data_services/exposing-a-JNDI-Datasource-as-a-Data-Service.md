# Exposing a JNDI Datasource as a Data Service

Java Naming and Directory Interface (JNDI) is a Java API (application
programming interface) providing naming and directory functionality for
Java software clients to discover and look up data and objects via a
name. WSO2 Enterprise Integrator (WSO2 EI) supports the JNDI
InitialContext implementation by inheriting the JNDI implementation of
Tomcat (
<http://tomcat.apache.org/tomcat-7.0-doc/jndi-resources-howto.html> ).
JNDI helps decouple object creation from the object look-up . When you
have registered a datasource with JNDI, others can discover it through a
JNDI lookup and use it.

Follow the topics below. Also, see the samples in [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Start the Create New Data Service
    wizard](#ExposingaJNDIDatasourceasaDataService-StarttheCreateNewDataServicewizard)
-   [Add a
    JNDI datasource](#ExposingaJNDIDatasourceasaDataService-AddaJNDIdatasource)
-   [Define a query for the
    datasource](#ExposingaJNDIDatasourceasaDataService-Defineaqueryforthedatasource)
-   [Define an operation to invoke the
    query](#ExposingaJNDIDatasourceasaDataService-Defineanoperationtoinvokethequery)
-   [Finish creating the data
    service](#ExposingaJNDIDatasourceasaDataService-Finishcreatingthedataservice)

------------------------------------------------------------------------

### Start the Create New Data Service wizard

Now, let's start creating the data service from scratch:

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
    <td><code>                /Library/WSO2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>                C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\               </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>                /usr/lib/wso2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>                /usr/lib64/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    </tbody>
    </table>

2.  Start the ESB profile:

    -   [**On MacOS/Linux/CentOS**](#0af8a6a513604534a6c118b17795cd42)
    -   [**On Windows**](#1ccb894e33d5427e81410ea3e50b2369)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Log in to the management console using the following URL on your
    browser: <https://localhost:9443/carbon/> .

4.  Click **Create** under **Data Service** to open the **Create Data
    Service** wizard.

5.  Enter a name for the data service. Leave the default values for the
    other fields.

6.  Click **Next** to go to the **Datasources** screen.

### Add a JNDI datasource

You can add a JNDI datasource by following the steps given below.

1.  Click **Add New Datasource** to open the following screen:  
    ![](attachments/119130785/119130786.png){width="650" height="159"}

2.  Select **JNDI Datasource** as the data source type. The
    JNDI-specific options will be available for editing as shown
    below.  
    ![](attachments/16844921/17214853.png){width="550"}  
    The options in the above window are explained below:

    -   **JNDI Context Class** : The corresponding context factory
        class.

    <!-- -->

    -   **Provider URL** : The URL that specifies the location of a
        resource on the Web.

    <!-- -->

    -   **Resource Name** : The name of the JNDI resource.

  

!!! note

You can also expose an RDBMS Carbon datasource as a JNDI datasource
using the **Configure** **\> Datasources** menu of the management
console. For instructions, see [Managing
Datasources](https://docs.wso2.com/display/ADMIN44x/Managing+Datasources)
.


### Define a query for the datasource

Now let's start writing a query for getting data from the datasource.
The query will specify the data that should be fetched by this query,
and the format that should be used to display data when the query is
invoked.

Click **Add New Query** to open the **Add New Query** screen.

Enter the following values:  

-   **Query ID** : Enter an ID for the query.
-   **Datasource:** Select the datasource for which you are going to
    write a query. Select the **JNDI** datasource that you created
    previously.  
-   **SQL:** In this field, enter the SQL statement describing the data
    that should be retrieved from the RDBMS datasource.

**Add input mappings** : Input mappings allow you to add parameters to a
query so that you can set the parameter value when executing the query.

**Add output mappings** : Out mapping is used to specify how the data
that is fetched from your query will be shown in the response.

Click **Next** to open the **Operations** screen.

### Define an operation to invoke the query

Data service operations are written to invoke queries.

1.  Click **Add New Operation** to open **Add New Operation** screen.
2.  Add a name for your operation in the **Operation Name** field.
3.  In the **Query ID** field, select the query that you defined
    previously.
4.  Save the operation.

### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.
