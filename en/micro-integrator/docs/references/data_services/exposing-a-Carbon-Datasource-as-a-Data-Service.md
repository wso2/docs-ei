# Exposing a Carbon Datasource as a Data Service

A Carbon datasource is an RDBMS or a custom datasource created using the
ESB profile of WSO2 Enterprise Integrator (WSO2 EI). You can simply use
that as the datasource for a data service. A Carbon datasource is
persistent, and can be used whenever required.

### Creating a Carbon datasource

See [Managing
Datasources](https://docs.wso2.com/display/ADMIN44x/Managing+Datasources)
for instructions on how to create a Carbon datasource. Be sure to copy
the JDBC driver relevant to the database engine to one of the locations
given below.

-   If the driver is a JAR file, add it to the
    `          <EI_HOME>/lib         ` directory, so that it will be
    converted to an OSGI bundle and copied to the
    `          dropins         ` directory during server startup. For
    example, if you are using MySQL, you would specify
    `          com.mysql.jdbc.Driver         ` as the driver and would
    copy `          mysql-connector-java-5.XX-bin.jar         ` to this
    directory.
-   If the driver is already an OSGI bundle, you can directly add it
    to the `          <EI_HOME>/dropins         ` directory.

If you do not copy the driver to the relevant directory, you will get an
exception similar to "Cannot load JDBC driver class
com.mysql.jdbc.Driver" when you create the datasource.

### Creating a data service using the Carbon datasource

Follow the instructions in [Exposing a Datasource as a Data
Service](https://docs.wso2.com/display/EI650/Exposing+a+Datasource+as+a+Data+Service)
and w hen you get to the **Add New Data Source** screen, select **Carbon
Data Source** as the data source type. Y ou get a drop-down list from
which an existing Carbon data source can be selected as shown below.  

![](attachments/119130762/119130763.png)

Also, see the samples in [Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .
