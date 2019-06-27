# Forget-me Tool Overview

The Forget-me tool is shipped with WSO2 SP by default in the
`         <SP_HOME>/wso2/tools/identity-anonymization-tool-x.x.x        `
directory. If required, you can change the default location of the
configurations of this tool or make changes to the default
configurations. You can also run the Forget-me tool in the standalone
mode.

-   [Changing the default configurations
    location](#Forget-meToolOverview-Changingthedefaultconfigurationslocation)
-   [Changing the default configurations of the
    tool](#Forget-meToolOverview-Changingthedefaultconfigurationsofthetool)
-   [Running the Forget-me tool in the standalone
    mode](#Forget-meToolOverview-RunningtheForget-metoolinthestandalonemode)

## Changing the default configurations location

You can change the default location of the tool configurations if
desired. You may want to do this if you are working with a multi-product
environment where you want to manage configurations in a single location
for ease of use. Note that this is **optional** .

To change the default configurations location for the embedded tool, do
the following:

1.  Open the `           forgetme.sh          ` file found inside the
    `           <SP_HOME>/bin          ` directory.

2.  The location path is the value given after
    `                       -d                     ` within the
    following line. Modify the value after
    `                       -d                     ` to change the
    location.

        !!! info
    
        The default location path is
        `           $CARBON_HOME/repository/components/tools/forget-me/conf.          `
    

    ``` java
        sh $CARBON_HOME/repository/components/tools/identity-anonymization-tool/bin/forget-me -d $CARBON_HOME/repository/components/tools/identity-anonymization-tool/conf -carbon $CARBON_HOME $@
    ```

## Changing the default configurations of the tool

All configurations related to this tool can be found inside the
`         <SP_HOME>/wso2/tools/identity-anonymization-tool/conf        `
directory. The default configurations are set up as follows:

-   **Read Logs:** `          <SP_HOME>/wso2/<PROFILE>/logs         `

<!-- -->

-   **Read Datasource:**
    `          <SP_HOME>/conf/<PROFILE>deployment.yaml         ` file

<!-- -->

-   **Default datasources:**
    `          WSO2_CARBON_DB, WSO2_METRICS_DB         ` ,
    `          WSO2_PERMISSIONS_DB         ` ,
    `          WSO2_DASHBOARD_DB         ` ,
    `          BUSINESS_RULES_DB         ` ,
    `          SAMPLE_DB         ` ,
    `          WSO2_STATUS_DASHBOARD_DB         `
-   **Log file name regex** : The regex patterns defined in all the
    files in the
    `          <SP_HOME>/wso2/tools/identity-anonymization-tool/conf/log-config         `
    directory are considered.

For information on changing these configurations, see [Configuring the
config.json
file](https://docs.wso2.com/display/ADMIN44x/Removing+References+to+Deleted+User+Identities+in+WSO2+Products#RemovingReferencestoDeletedUserIdentitiesinWSO2Products-Configuringtheconfig.jsonfile)
in the Product Administration Guide.

## Running the Forget-me tool in the standalone mode

This tool can run standalone and therefore cater to multiple products.
This means that if you are using multiple WSO2 products and need to
delete the user's identity from all products at once, you can do so by
running the tool in standalone mode.  
For information on how to build and run the Forget-Me tool, see
[Removing References to Deleted User Identities in WSO2
Products](https://docs.wso2.com/display/ADMIN44x/Removing+References+to+Deleted+User+Identities+in+WSO2+Products)
in the WSO2 Administration Guide.
