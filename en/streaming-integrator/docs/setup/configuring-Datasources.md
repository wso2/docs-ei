# Configuring Datasources

In WSO2 SP, there are datasources specific to each runtime (i.e.,
worker, editor, manager, and dashboard runtimes). The datasources of
each runtime are defined in the
`         <SP_HOME>/conf/<runtime>/deployment.yaml        ` file. e.g.,
To configure a datasource in the worker runtime, the relevant
configurations need to be added in the
`         <SP_Home>/conf/worker/deployment.yaml        ` file.

To view a sample datasource confuguration for each database type
supported, click on the following links:

!!! info

If the database driver is not an OSGI bundle, then it should be
converted to OSGI (using jartobundle.sh) before placing it in the
`         <SP_HOME>/lib        ` directory. For detailed instructions,
see [Adding Third Party Non OSGi
Libraries](_Adding_Third_Party_Non_OSGi_Libraries_) .

e.g.,
`         sh WSO2_SP_HOME/bin/jartobundle.sh ojdbc6.jar WSO2_SP_HOME/lib/        `

The database should be tuned to handle the total **maxPoolSize** ( The
maximum number of threads that should be reserved at any given time to
handle events ) which defined in deployment.yaml


![](images/icons/grey_arrow_down.png){.expand-control-image} MySQL

``` xml
    wso2.datasources:
     dataSources:
     description: The datasource used for test database
     jndiConfig:
     definition:
       type: RDBMS
       configuration: 
         jdbcUrl: jdbc:mysql://hostname:port/testdb
         username: root
         password: root
         driverClassName: com.mysql.jdbc.Driver
         minIdle: 5
         maxPoolSize: 50
         idleTimeout: 60000
         connectionTestQuery: SELECT 1
         validationTimeout: 30000
         isAutoCommit: false
```
    
    ![](images/icons/grey_arrow_down.png){.expand-control-image} POSTGRES
    
``` xml
wso2.datasources:
 dataSources:
     description: The datasource used for test database
     jndiConfig:
     definition:
       type: RDBMS
      configuration:
        jdbcUrl: jdbc:postgresql://hostname:port/testdb
        username: root
        password: root
        driverClassName: org.postgresql.Driver
        minIdle: 5
        maxPoolSize: 50
        idleTimeout: 60000
        connectionTestQuery: SELECT 1
        validationTimeout: 30000
        isAutoCommit: false
```
    
    ![](images/icons/grey_arrow_down.png){.expand-control-image} Oracle
    
    There are two ways to configure this database type. If you have a System
    Identifier (SID), use this (older) format:
    
    `           jdbc:oracle:thin:@[HOST][:PORT]:SID          `
    
``` xml
wso2.datasources:
 dataSources:
     description: The datasource used for test database
     jndiConfig:
     definition:
       type: RDBMS
       configuration:
         jdbcUrl: jdbc:oracle:thin:@hostname:port:SID
         username: testdb
         password: root
         driverClassName: oracle.jdbc.driver.OracleDriver
         minIdle: 5
         maxPoolSize: 50
         idleTimeout: 60000
         connectionTestQuery: SELECT 1
         validationTimeout: 30000
         isAutoCommit: false
```
    
    If you have an Oracle service name, use this (newer) format:
    
    `           jdbc:oracle:thin:@//[HOST][:PORT]/SERVICE          `
    
``` xml
wso2.datasources:
 dataSources:
     description: The datasource used for test database
     jndiConfig:
     definition:
       type: RDBMS
       configuration:
         jdbcUrl: jdbc:oracle:thin:@hostname:port/SERVICE
         username: testdb
         password: root
         driverClassName: oracle.jdbc.driver.OracleDriver
         minIdle: 5
         maxPoolSize: 50
         idleTimeout: 60000
         connectionTestQuery: SELECT 1
         validationTimeout: 30000
         isAutoCommit: false
```
    
    The Oracle driver need to be converted to OSGi (using
    `           jartobundle.sh          ` ) before put into
    `           WSO2_SP_HOME/lib          ` directory. For detailed
    instructions, see [Adding Third Party Non OSGi
    Libraries](https://docs.wso2.com/display/SP420/Adding+Third+Party+Non+OSGi+Libraries)
    .
    
      
    
    ![](images/icons/grey_arrow_down.png){.expand-control-image} MSSQL
    
``` xml
wso2.datasources:
 dataSources:
     description: The datasource used for test database
     jndiConfig:
     definition:
       type: RDBMS
       configuration: 
         jdbcUrl: jdbc:sqlserver://hostname:port;databaseName=testdb
         username: root
         password: root
         driverClassName: com.microsoft.sqlserver.jdbc.SQLServerDriver
         minIdle: 5
         maxPoolSize: 50
         idleTimeout: 60000
         connectionTestQuery: SELECT 1
         validationTimeout: 30000
         isAutoCommit: false
```
    
    The following sections explain the default datasources configured in
    various WSO2 SP components for different purposes, and how to change
    them.
    
    -   [RDBMS data
        provider](#ConfiguringDatasources-RDBMSdataproviderRDBMSDataProvider)
    -   [Carbon
        coordination](#ConfiguringDatasources-CarboncoordinationCarbonCoordination)
    -   [Stream Processor core -
        persistence](#ConfiguringDatasources-StreamProcessorcore-persistenceStreamProcessorCore-Persistence)
    -   [Stream Processor - Status
        Dashboard](#ConfiguringDatasources-StreamProcessor-StatusDashboardStreamProcessor-StatusDashboard)
    -   [Siddhi RDBMS
        store](#ConfiguringDatasources-SiddhiRDBMSstoreSiddhiRDBMSStore)
    -   [Carbon
        Dashboards](#ConfiguringDatasources-CarbonDashboardsCarbonDashboards)
    -   [Business Rules](#ConfiguringDatasources-BusinessRulesBusinessRules)
    -   [IdP client](#ConfiguringDatasources-IdPclientIdPClient)
    -   [Permission
         provider](#ConfiguringDatasources-PermissionproviderPermissionProvider)
    -   [Distributed Message
        Tracer](#ConfiguringDatasources-DistributedMessageTracerDistributedMessageTracer)
    
    #### RDBMS data provider
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement<br />
    </td>
    <td>The RDBMS provider publishes records from RDBMS tables into generated widgets. It can also be configured to purge records in tables. In order to carry out these actions, this provider requires access to read and delete records in user defined tables of the database. For more information about the RDBMS data provider, see <a href="https://docs.wso2.com/display/SP440/Generating+Widgets">Generating Widgets</a> .</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>This is required if you select a datasource when generating the widget or use existing widgets that connect to the RDBMS data provider when you run the dashboard profile of WSO2 SP.</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td><code>             SAMPLE_DB            </code></td>
    </tr>
    <tr class="even">
    <td>Default Database</td>
    <td>The default <code>             H2            </code> database location is <code>             &lt;SP_HOME&gt;/wso2/dashboard/database/SAMPLE_DB            </code> .</td>
    </tr>
    <tr class="odd">
    <td>Tables</td>
    <td>The default database shipped with a sample table named <code>             TRANSACTION_TABLE            </code> .</td>
    </tr>
    <tr class="even">
    <td>Schemas and Queries</td>
    <td><p>The schema for the sample table is <code>              TRANSACTIONS_TABLE (creditCardNo VARCHAR(50), country VARCHAR(50), transaction VARCHAR(50), amount INT)             </code></p>
    <p>The default queries can be viewed <a href="https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.data.provider/src/main/resources/queries.yaml">here</a> .</p></td>
    </tr>
    <tr class="odd">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Postgres, Mssql, Oracle 11g</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Carbon coordination
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>Carbon coordination supports zookeeper and RDBMS based coordination. In RDBMS coordination, database access is required for updating the heartbeats of the nodes. In addition, database access is required to update the coordinator and the other members in the cluster. For more information, see <a href="_Configuring_Cluster_Coordination_">Configuring Cluster Coordination</a> .</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>This is required. However, you can also use Zookeeper coordination instead of RDBMS.</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>The carbon datasources are used. The default datasource varies depending on the deployment as follows:
    <ul>
    <li>If SP is deployed in as a <a href="https://docs.wso2.com/display/SP440/Minimum+High+Availability+Deployment">minimum HA cluster</a> , <code>               WSO2_CARBON_DB              </code> is the default database.</li>
    <li>If SP is deployed as a <a href="https://docs.wso2.com/display/SP430/Fully+Distributed+Deployment">fully distributed cluster</a> , <code>               SP_MGT_DB              </code> is the default database.</li>
    </ul></td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td><code>             LEADER_STATUS_TABLE            </code> , <code>             MEMBERSHIP_EVENT_TABLE            </code> , <code>             REMOVED_MEMBERS_TABLE            </code> , <code>             CLUSTER_NODE_STATUS_TABLE            </code></td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p>Information about the default queries and the schema can be viewed <a href="https://github.com/wso2/carbon-coordination/blob/v2.0.12/components/cluster-coordinator/org.wso2.carbon.cluster.coordinator/org.wso2.carbon.cluster.coordinator.rdbms/src/main/java/org/wso2/carbon/cluster/coordinator/rdbms/util/RDBMSConstants.java">here</a> .</p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>MySQL, Postgres, Mssql, Oracle 11g</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Stream Processor core - persistence
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>This involves persisting the state of Siddhi Applications periodically in the database. State persistence is enabled by selecting the <code>             org.wso2.carbon.stream.processor.core.persistence.DBPersistenceStore            </code> class in the <code>             state.persistence            </code> section of the <code>             &lt;SP_Home&gt;/conf/&lt;worker/manager&gt;/deployment.yaml            </code> file. For more information, see <a href="_Configuring_Database_and_File_System_State_Persistence_">Configuring Database and File System State Persistence</a> .</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>This is optional. WSO2 is configured to persist the state of Siddhi applications by default.</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>N/A. If state persistence is required, you need to configure the datasource in the <code>             &lt;SP_Home&gt;/conf/&lt;worker/manager&gt;/deployment.yaml            </code> file under <code>             state.persistence            </code> &gt; <code>             config            </code> &gt; <code>             datasource            </code> .</td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>N/A. If state persistence is required, you need to specify the table name to be used when persisting the state in the <code>             &lt;SP_Home&gt;/conf/&lt;worker/manager&gt;/deployment.yaml            </code> file under <code>             state.persistence            </code> &gt; <code>             config            </code> &gt; <code>             table            </code> .</td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p>Information about the default queries and schema can be viewed <a href="https://github.com/wso2/carbon-analytics/blob/master/components/org.wso2.carbon.stream.processor.core/src/main/resources/queries.yaml">here</a> .</p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Postgres, Mssql, Oracle 11g</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Stream Processor - Status Dashboard
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>To display information relating to the status of your SP deployment, the Status Dashboard needs to retrive carbon metrics data, registered SP worker details and authentication details within the cluster from the database. For more information, see <a href="https://docs.wso2.com/display/SP440/Monitoring+Stream+Processor">Monitoring Stream Processor</a> .</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Required</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td><code>             WSO2_STATUS_DASHBOARD_DB            </code> , <code>             WSO2_METRICS_DB            </code></td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td><code>             METRIC_COUNTER            </code> , <code>             METRIC_GAUGE            </code> , <code>             METRIC_HISTOGRAM,            </code> <code>             METRIC_METER            </code> , <code>             METRIC_TIMER            </code> , <code>             WORKERS_CONFIGURATIONS            </code> , <code>             WORKERS_DETAILS            </code></td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p>Information about the default queries and schema: <a href="https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.status.dashboard.core/src/main/resources/queries.yaml">https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.status.dashboard.core/src/main/resources/queries.yaml</a></p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Mssql, Oracle 11g ( Postgres is tested with Carbon-Metrics only)</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Siddhi RDBMS store
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>It gives the capability of creating the tables at the siddhi app runtime and access the existing tablesif a user defined carbon datasource or JNDI property in a siddhi app. Documentation can be found in <a href="https://wso2-extensions.github.io/siddhi-store-rdbms/api/4.0.15/">https://wso2-extensions.github.io/siddhi-store-rdbms/api/4.0.15/</a></td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Optional</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>No such default Datasource. User has to create the datasource in the Siddhi app</td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>No such default tables. User has to define the tables</td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p>Information about the default queries and schema: <a href="https://github.com/wso2-extensions/siddhi-store-rdbms/blob/v4.0.15/component/src/main/resources/rdbms-table-config.xml">https://github.com/wso2-extensions/siddhi-store-rdbms/blob/v4.0.15/component/src/main/resources/rdbms-table-config.xml</a></p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Mssql, Oracle 11g, DB2, PostgreSQL</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Carbon Dashboards
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>Carbon Dashboard feature uses its datasource to persist the dashboard related information</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Optional</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>WSO2_DASHBOARD_DB</td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>DASHBOARD_RESOURCE</td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p><a href="https://github.com/wso2/carbon-dashboards/tree/master/features/org.wso2.carbon.dashboards.api.feature/src/main/resources/sql">https://github.com/wso2/carbon-dashboards/tree/master/features/org.wso2.carbon.dashboards.api.feature/src/main/resources/sql</a></p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Postgres</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Business Rules
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>Business Rules feature uses database to persist the derived business rules</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Mandatory</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>BUSINESS_RULES_DB<br />
    </td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>BUSINESS_RULES, RULES_TEMPLATES</td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p><a href="https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.business.rules.core/src/main/resources/queries.yaml">https://github.com/wso2/carbon-analytics/blob/v2.0.250/components/org.wso2.carbon.business.rules.core/src/main/resources/queries.yaml</a></p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Oracle 11g</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### IdP client
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>IdP client access the DB layer to persist the client id and the client secret of dynamic client registration</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Mandatory for external IdP client</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>DB_AUTH_DB</td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>OAUTH_APPS</td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p><a href="https://github.com/wso2/carbon-analytics-common/blob/v6.0.52/components/authentication/org.wso2.carbon.analytics.idp.client/src/main/resources/queries.yaml">https://github.com/wso2/carbon-analytics-common/blob/v6.0.52/components/authentication/org.wso2.carbon.analytics.idp.client/src/main/resources/queries.yaml</a></p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Oracle 11g</td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Permission  provider
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>Permission provider will access the DB to p ersist permissions and role - permission mappings.</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Mandatory, default is in H2</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>PERMISSIONS_DB</td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>PERMISSIONS, ROLE_PERMISSIONS<br />
    </td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td><p><a href="https://github.com/wso2/carbon-analytics-common/blob/v6.0.52/components/permission-provider/org.wso2.carbon.analytics.permissions/src/main/resources/queries.yaml">https://github.com/wso2/carbon-analytics-common/blob/v6.0.52/components/permission-provider/org.wso2.carbon.analytics.permissions/src/main/resources/queries.yaml</a></p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Mssql, Oracle 11g , Postgres<br />
    </td>
    </tr>
    </tbody>
    </table>
    
      
    
    #### Distributed Message Tracer
    
    <table>
    <colgroup>
    <col style="width: 13%" />
    <col style="width: 86%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Database Access Requirement</td>
    <td>The Siddhi application and the dashbioard configured for the <a href="https://docs.wso2.com/display/SP440/Distributed+Message+Tracer">Distributed Message Tracer</a> solution that is shipped with WSO2 SP by default access this database to both read and write data.</td>
    </tr>
    <tr class="even">
    <td>Required/Optional</td>
    <td>Optional, default is in H2. This database is only needed when <a href="https://docs.wso2.com/display/SP440/Distributed+Message+Tracer">Distribution Message Tracing</a> is enabled.</td>
    </tr>
    <tr class="odd">
    <td>Default Datasource Name</td>
    <td>Message_Tracing_DB<br />
    </td>
    </tr>
    <tr class="even">
    <td>Tables</td>
    <td>SpanTable<br />
    </td>
    </tr>
    <tr class="odd">
    <td>Schemas and Queries</td>
    <td>This database uses Siddhi Queries to insert data. It reads data from <a href="https://github.com/wso2/analytics-solutions/blob/089b98ee6d7d6f7574f9547c8b30db140eec38c4/components/sp-solutions/org.wso2.analytics.solutions.sp.message.tracer/widgets/OpenTracingList/src/resources/widgetConf.json#L19">TracingListGadget</a> , <a href="https://github.com/wso2/analytics-solutions/blob/089b98ee6d7d6f7574f9547c8b30db140eec38c4/components/sp-solutions/org.wso2.analytics.solutions.sp.message.tracer/widgets/OpenTracingSearch/src/resources/widgetConf.json#L18">TracingSearchGadget</a> , <a href="https://github.com/wso2/analytics-solutions/blob/089b98ee6d7d6f7574f9547c8b30db140eec38c4/components/sp-solutions/org.wso2.analytics.solutions.sp.message.tracer/widgets/OpenTracingVisTimeline/src/resources/widgetConf.json#L19">TracingTimelineGadget</a>
    <p><br />
    </p></td>
    </tr>
    <tr class="even">
    <td>Tested Database Types</td>
    <td>H2, MySQL, Mssql, Oracle 11g<br />
    </td>
    </tr>
    </tbody>
    </table>
