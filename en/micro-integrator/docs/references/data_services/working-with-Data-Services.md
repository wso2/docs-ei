# Working with Data Services

Data integration is an important part of an integration process. For
example, consider the following scenario, where you have a typical
integration process that is managed using the ESB profile of WSO2 EI. In
this scenario, data stored in various, disparate datasources are
required in order to complete the integration use case. The data
services functionality that is embedded in the ESB profile allows you to
manage this integration scenario by decoupling the data from the
datasource layer and exposing them as data services. The main
integration flow defined in the ESB will then have the capability of
managing the data through the data service. Once the data service is
defined, you can manipulate the data stored in the datasources by
invoking the relevant operation defined in the data service. For
example, you can perform the basic CRUD operations as well as other
advanced operations.

!!! note

The data services feature was previously provided by WSO2 as a separate
product (WSO2 Data Services Server). With the introduction of WSO2 EI,
you no longer require a separate data services server to manage the
datasources required for your integration scenario.


See the following topics for more information:

-   [Creating a Data Service from
    Scratch](_Creating_a_Data_Service_from_Scratch_)
-   [Uploading a Created Data
    Service](_Uploading_a_Created_Data_Service_)
-   [Generating a Data Service](_Generating_a_Data_Service_)
-   [Elements of a Data Service](_Elements_of_a_Data_Service_)
-   [Managing Data Integration Artifacts via
    Tooling](_Managing_Data_Integration_Artifacts_via_Tooling_)
-   [Writing Queries](_Writing_Queries_)
-   [Input Validators](_Input_Validators_)
-   [Filtering Responses by User
    Role](_Filtering_Responses_by_User_Role_)
-   [Using Namespaces](_Using_Namespaces_)
-   [RDBMS Datasources](_RDBMS_Datasources_)
-   [Sample Queries](_Sample_Queries_)

  
