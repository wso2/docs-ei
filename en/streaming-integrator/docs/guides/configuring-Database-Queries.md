# Configuring Database Queries

Database queries used in the following features and can configured for
new database or override the default queries for the new database
version. The relevant configurations should be added to the
`         <SP_Home>/conf/<Runtime>/deployment.yaml        ` file.

-   [Business
    Rules](#ConfiguringDatabaseQueries-BusinessRulesBusinessRule)
-   [Status
    Dashboard](#ConfiguringDatabaseQueries-StatusDashboardStatusDashboard)
-   [Dashboard](#ConfiguringDatabaseQueries-DashboardDashboard)
-   [RDBMS Data
    Provider](#ConfiguringDatabaseQueries-RDBMSDataProviderRDBMSDataProvider)

The following are sample database query configurations that override the
MySQL default version queries in the RDBMS Data Provider Feature by
MySQL new version 5.7.0 and adding new queries for new database type
MariaDB with version 10.0.20 respectiveley:

``` xml
    wso2.rdbms.data.provider:
      queries:
        -
         mappings:
           record_delete: "DELETE FROM {{TABLE_NAME}} ORDER BY {{INCREMENTAL_COLUMN}} ASC LIMIT {{LIMIT_VALUE}}"
           total_record_count: "SELECT COUNT(*) FROM {{TABLE_NAME}}"
           record_limit: "{{CUSTOM_QUERY}} ORDER BY {{INCREMENTAL_COLUMN}} DESC LIMIT {{LIMIT_VALUE}}"
           record_greater_than: "SELECT * FROM ({{CUSTOM_QUERY}} ORDER BY {{INCREMENTAL_COLUMN}} DESC ) AS A_TABLE WHERE {{INCREMENTAL_COLUMN}} > {{LAST_RECORD_VALUE}}"
        type: MySQL
        version: 5.7.0
        -
         mappings:
           record_delete: "DELETE FROM {{TABLE_NAME}} ORDER BY {{INCREMENTAL_COLUMN}} ASC LIMIT {{LIMIT_VALUE}}"
           total_record_count: "SELECT COUNT(*) FROM {{TABLE_NAME}}"
           record_limit: "{{CUSTOM_QUERY}} ORDER BY {{INCREMENTAL_COLUMN}} DESC LIMIT {{LIMIT_VALUE}}"
           record_greater_than: "SELECT * FROM ({{CUSTOM_QUERY}} ORDER BY {{INCREMENTAL_COLUMN}} DESC ) AS A_TABLE WHERE {{INCREMENTAL_COLUMN}} > {{LAST_RECORD_VALUE}}"
        type: MariaDB
        version: 10.0.20
```

###  Business Rules

To add or override database queries for Business Rules you need to
configure the \<SP\_Home\>/conf/dashboard/deployment.yaml file as
indicated in the above example:

1.  You can find the avaialble default queries in :
    <https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.business.rules.core/src/main/resources/queries.yaml>
2.  Copy the queries with the above structure under the '
    **wso2.business.rules.manager** ' namespace.

### Status Dashboard

To add or override database queries for Status Dashbord you need to
configure the \<SP\_Home\>/conf/dashboard/deployment.yaml file as
indicated in the above example:

1.  You can find the avaialble default queries in :
    <https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.status.dashboard.core/src/main/resources/queries.yaml>
2.  Copy the queries with the above structure under the '
    **wso2.status.dashboard** ' namespace.

### Dashboard

To add or override database queries for Dashbord you need to configure
the \<SP\_Home\>/conf/dashboard/deployment.yaml file as indicated in the
above example:

1.  You can find the avaialble default queries in :
    <https://github.com/wso2/carbon-dashboards/blob/v4.0.8/components/dashboards/org.wso2.carbon.dashboards.core/src/main/resources/sql-queries.yaml>
2.  Copy the queries with the above structure under the '
    **wso2.dashboard** ' namespace.

### RDBMS Data Provider

To add or override database queries for RDBMS Data Provider you need to
configure the \<SP\_Home\>/conf/dashboard/deployment.yaml file as
indicated in the above example:

1.  You can find the avaialble default queries in :
    <https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.data.provider/src/main/resources/queries.yaml>
2.  Copy the queries with the above structure under the '
    **wso2.rdbms.data.provider'** namespace.  
      

!!! info

**Note** : Please note that in order to apply the above changes, you
need to restart the server runtime.


  
