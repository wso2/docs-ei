# Configuring the Status Dashboard

The following sections cover the configurations that need to be done in
order to view statistics relating to the performance of your WSO2 SP
deployment in the status dashboard.

-   [Assigning unique carbon IDs to
    nodes](#ConfiguringtheStatusDashboard-AssigninguniquecarbonIDstonodes)
-   [Setting up the
    database](#ConfiguringtheStatusDashboard-Settingupthedatabase)
-   [Configuring
    metrics](#ConfiguringtheStatusDashboard-Configuringmetrics)
-   [Configuring cluster
    credentials](#ConfiguringtheStatusDashboard-Configuringclustercredentials)
-   [Configuring
    permissions](#ConfiguringtheStatusDashboard-Configuringpermissions)

## Assigning unique carbon IDs to nodes

Carbon metrics uses the carbon ID as the source ID for metrics.
Therefore, all the worker nodes are required to have a **unique carbon
ID** defined in the `         wso2.carbon:        ` section of the
`         <SP_HOME>/conf/worker/deployment.yaml        ` file as shown
in the extract below.

!!! info

You need to ensure that the carbon ID of each node is unique because it
is required for the Status dashboard to identify the worker nodes and
display their statistics accordingly.


``` xml
    wso2.carbon:
        # value to uniquely identify a server
      id: wso2-sp
        # server name
      name: WSO2 Stream Processor
        # ports used by this server
```

  

## Setting up the database

To monitor statistics in the Status Dashboard, you need a shared metrics
database that stores the metrics of all the nodes.

Set up a database of the required type by following the steps below. In
this section, a MySQL database is created as an example.

!!! info

The Status Dashboard is only supported with H2, MySQL, MSSQL and Oracle
database types. It is configured with the H2 database type by default.
If you want to continue to use H2, skip this step.


1.  Download and install the required database type. For this example,
    let's download and install [MySQL
    Server](http://dev.mysql.com/downloads/) .
2.  Download the required database driver. For this example, download
    the [MySQL JDBC driver](http://dev.mysql.com/downloads/connector/j/)
    .

3.  Unzip the database driver you downloaded, copy its JAR file (
    `           mysql-connector-java-x.x.xx-bin.jar          ` in this
    example), and place it in the `           <SP_HOME>/lib          `
    directory.

4.  Enter the following command in a terminal/command window, where
    `          username         ` is the username you want to use to
    access the databases.  
    `          mysql -u username -p         `
5.  When prompted, specify the password you are using to access the
    databases with the username you specified.
6.  Create two databases named `           WSO2_METRICS_DB          `
    (to store metrics data) and
    `           WSO2_STATUS_DASHBOARD_DB          ` (to store
    statistics) with tables. To create MySQL databases and tables for
    this example, run the following commands.

    ``` java
        mysql> create database WSO2_METRICS_DB;
        mysql> use WSO2_METRICS_DB;
        mysql> source <SP_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;
        mysql> grant all on WSO2_METRICS_DB.* TO username@localhost identified by "password";
    ```

    ``` java
            mysql> create database WSO2_STATUS_DASHBOARD_DB;
            mysql> use WSO2_STATUS_DASHBOARD_DB;
            mysql> source <SP_HOME>/wso2/editor/dbscripts/metrics/mysql.sql;
            mysql> grant all on WSO2_STATUS_DASHBOARD_DB.* TO username@localhost identified by "password";
    ```

7.  Create two datasources named `           WSO2_METRICS_DB          `
    and `           WSO2_STATUS_DASHBOARD_DB          ` by adding the
    following datasource configurations under the
    `           wso2.datasources:          ` section of the
    `           <SP_HOME>/conf/worker/deployment.yaml          ` file.

        !!! info
    
        The names of the datasources must be thesame as the names of the
        database tables you created for metrics and statistics.  
        You need to change the values for the
        `           username          ` and `           password          `
        parameters to the username and password that you are using to access
        the MySQL database.
    
        For detailed information about datasources, see
        [carbon-datasources](https://github.com/wso2/carbon-datasources/) .
    

      

    -   `                           WSO2_METRICS_DB             `

        ``` xml
                   - name: WSO2_METRICS_DB
                      description: The datasource used for dashboard feature
                      jndiConfig:
                        name: jdbc/WSO2MetricsDB
                      definition:
                        type: RDBMS
                        configuration:
                          jdbcUrl: 'jdbc:mysql://localhost/WSO2_METRICS_DB?useSSL=false'
                          username: root
                          password: root
                          driverClassName: com.mysql.jdbc.Driver
                          maxPoolSize: 50
                          idleTimeout: 60000
                          connectionTestQuery: SELECT 1
                          validationTimeout: 30000
                          isAutoCommit: false
        ```

    -   `                           WSO2_STATUS_DASHBOARD_DB                           `

        ``` xml
                        - name: WSO2_STATUS_DASHBOARD_DB
                          description: The datasource used for dashboard feature
                          jndiConfig:
                            name: jdbc/wso2_status_dashboard
                            useJndiReference: true
                          definition:
                            type: RDBMS
                            configuration:
                              jdbcUrl: 'jdbc:mysql://localhost/WSO2_STATUS_DASHBOARD_DB?useSSL=false'
                              username: root
                              password: root
                              driverClassName: com.mysql.jdbc.Driver
                              maxPoolSize: 50
                              idleTimeout: 60000
                              connectionTestQuery: SELECT 1
                              validationTimeout: 30000
                              isAutoCommit: false
        ```

        `                                        `

        The following are sample configurations for database tables when
        you use other database types supported.  

        ![](images/icons/grey_arrow_down.png){.expand-control-image}
        Click here to view the sample datasource configurations

        <table>
        <thead>
        <tr class="header">
        <th>Database Type</th>
        <th>Metrics Datasource</th>
        <th>Dashboard Datasource</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>MSSQL</td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb1-1"><a href="#cb1-1"></a>name: WSO2_METRICS_DB</span>
        <span id="cb1-2"><a href="#cb1-2"></a>  description: The datasource used for dashboard feature</span>
        <span id="cb1-3"><a href="#cb1-3"></a>  jndiConfig:</span>
        <span id="cb1-4"><a href="#cb1-4"></a> </span>
        <span id="cb1-5"><a href="#cb1-5"></a>               name: jdbc/WSO2MetricsDB</span>
        <span id="cb1-6"><a href="#cb1-6"></a>  definition:</span>
        <span id="cb1-7"><a href="#cb1-7"></a> </span>
        <span id="cb1-8"><a href="#cb1-8"></a>               type: RDBMS</span>
        <span id="cb1-9"><a href="#cb1-9"></a>    configuration:</span>
        <span id="cb1-10"><a href="#cb1-10"></a> </span>
        <span id="cb1-11"><a href="#cb1-11"></a>               jdbcUrl: </span>
        <span id="cb1-12"><a href="#cb1-12"></a>              &#39;jdbc:sqlserver://localhost;databaseName=wso2_metrics&#39;</span>
        <span id="cb1-13"><a href="#cb1-13"></a> </span>
        <span id="cb1-14"><a href="#cb1-14"></a>               </span>
        <span id="cb1-15"><a href="#cb1-15"></a>              username: root</span>
        <span id="cb1-16"><a href="#cb1-16"></a>      password: root</span>
        <span id="cb1-17"><a href="#cb1-17"></a>      driverClassName: com.microsoft.sqlserver.jdbc.SQLServerDriver</span>
        <span id="cb1-18"><a href="#cb1-18"></a>      maxPoolSize: 50</span>
        <span id="cb1-19"><a href="#cb1-19"></a>      idleTimeout: 60000</span>
        <span id="cb1-20"><a href="#cb1-20"></a>      connectionTestQuery: SELECT 1</span>
        <span id="cb1-21"><a href="#cb1-21"></a>      validationTimeout: 30000</span>
        <span id="cb1-22"><a href="#cb1-22"></a>      isAutoCommit: false</span></code></pre></div>
        </div>
        </div>
        </div></td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb2-1"><a href="#cb2-1"></a>name: WSO2_STATUS_DASHBOARD_DB</span>
        <span id="cb2-2"><a href="#cb2-2"></a>  description: The datasource used for dashboard feature</span>
        <span id="cb2-3"><a href="#cb2-3"></a>  jndiConfig:</span>
        <span id="cb2-4"><a href="#cb2-4"></a> </span>
        <span id="cb2-5"><a href="#cb2-5"></a>               name: jdbc/wso2_status_dashboard</span>
        <span id="cb2-6"><a href="#cb2-6"></a>    useJndiReference: true</span>
        <span id="cb2-7"><a href="#cb2-7"></a>  definition:</span>
        <span id="cb2-8"><a href="#cb2-8"></a> </span>
        <span id="cb2-9"><a href="#cb2-9"></a>               type: RDBMS</span>
        <span id="cb2-10"><a href="#cb2-10"></a>    configuration:</span>
        <span id="cb2-11"><a href="#cb2-11"></a> </span>
        <span id="cb2-12"><a href="#cb2-12"></a>               jdbcUrl: </span>
        <span id="cb2-13"><a href="#cb2-13"></a>              &#39;jdbc:sqlserver://localhost;databaseName=monitoring&#39;</span>
        <span id="cb2-14"><a href="#cb2-14"></a> </span>
        <span id="cb2-15"><a href="#cb2-15"></a>               </span>
        <span id="cb2-16"><a href="#cb2-16"></a>              username: root</span>
        <span id="cb2-17"><a href="#cb2-17"></a>      password: root</span>
        <span id="cb2-18"><a href="#cb2-18"></a>      driverClassName: com.microsoft.sqlserver.jdbc.SQLServerDriver</span>
        <span id="cb2-19"><a href="#cb2-19"></a>      maxPoolSize: 50</span>
        <span id="cb2-20"><a href="#cb2-20"></a>      idleTimeout: 60000</span>
        <span id="cb2-21"><a href="#cb2-21"></a>      connectionTestQuery: SELECT 1</span>
        <span id="cb2-22"><a href="#cb2-22"></a>      validationTimeout: 30000</span>
        <span id="cb2-23"><a href="#cb2-23"></a>      isAutoCommit: false</span></code></pre></div>
        </div>
        </div>
        </div></td>
        </tr>
        <tr class="even">
        <td>Oracle</td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb3-1"><a href="#cb3-1"></a>name: WSO2_METRICS_DB</span>
        <span id="cb3-2"><a href="#cb3-2"></a>  description: The datasource used for dashboard feature</span>
        <span id="cb3-3"><a href="#cb3-3"></a>  jndiConfig:</span>
        <span id="cb3-4"><a href="#cb3-4"></a> </span>
        <span id="cb3-5"><a href="#cb3-5"></a>               name: jdbc/WSO2MetricsDB</span>
        <span id="cb3-6"><a href="#cb3-6"></a>  definition:</span>
        <span id="cb3-7"><a href="#cb3-7"></a> </span>
        <span id="cb3-8"><a href="#cb3-8"></a>               type: RDBMS</span>
        <span id="cb3-9"><a href="#cb3-9"></a>    configuration:</span>
        <span id="cb3-10"><a href="#cb3-10"></a> </span>
        <span id="cb3-11"><a href="#cb3-11"></a>               jdbcUrl: </span>
        <span id="cb3-12"><a href="#cb3-12"></a>              &#39;jdbc:oracle:thin:@localhost:1521/xe&#39;</span>
        <span id="cb3-13"><a href="#cb3-13"></a> </span>
        <span id="cb3-14"><a href="#cb3-14"></a>               </span>
        <span id="cb3-15"><a href="#cb3-15"></a>              username: root</span>
        <span id="cb3-16"><a href="#cb3-16"></a>      password: root</span>
        <span id="cb3-17"><a href="#cb3-17"></a>      driverClassName: oracle.jdbc.driver.OracleDriver</span>
        <span id="cb3-18"><a href="#cb3-18"></a>      maxPoolSize: 50</span>
        <span id="cb3-19"><a href="#cb3-19"></a>      idleTimeout: 60000</span>
        <span id="cb3-20"><a href="#cb3-20"></a>      connectionTestQuery: SELECT 1</span>
        <span id="cb3-21"><a href="#cb3-21"></a>      validationTimeout: 30000</span>
        <span id="cb3-22"><a href="#cb3-22"></a>      isAutoCommit: false</span></code></pre></div>
        </div>
        </div>
        </div></td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: xml; gutter: false; theme: Confluence"><pre class="sourceCode xml"><code class="sourceCode xml"><span id="cb4-1"><a href="#cb4-1"></a>name: WSO2_STATUS_DASHBOARD_DB</span>
        <span id="cb4-2"><a href="#cb4-2"></a>  description: The datasource used for dashboard feature</span>
        <span id="cb4-3"><a href="#cb4-3"></a>  jndiConfig:</span>
        <span id="cb4-4"><a href="#cb4-4"></a> </span>
        <span id="cb4-5"><a href="#cb4-5"></a>               name: jdbc/wso2_status_dashboard</span>
        <span id="cb4-6"><a href="#cb4-6"></a>    useJndiReference: true</span>
        <span id="cb4-7"><a href="#cb4-7"></a>  definition:</span>
        <span id="cb4-8"><a href="#cb4-8"></a> </span>
        <span id="cb4-9"><a href="#cb4-9"></a>               type: RDBMS</span>
        <span id="cb4-10"><a href="#cb4-10"></a>    configuration:</span>
        <span id="cb4-11"><a href="#cb4-11"></a> </span>
        <span id="cb4-12"><a href="#cb4-12"></a>               jdbcUrl: </span>
        <span id="cb4-13"><a href="#cb4-13"></a>              &#39;jdbc:oracle:thin:@localhost:1521/xe&#39;</span>
        <span id="cb4-14"><a href="#cb4-14"></a> </span>
        <span id="cb4-15"><a href="#cb4-15"></a>               </span>
        <span id="cb4-16"><a href="#cb4-16"></a>              username: root</span>
        <span id="cb4-17"><a href="#cb4-17"></a>      password: root</span>
        <span id="cb4-18"><a href="#cb4-18"></a>      driverClassName: oracle.jdbc.driver.OracleDriver</span>
        <span id="cb4-19"><a href="#cb4-19"></a>      maxPoolSize: 50</span>
        <span id="cb4-20"><a href="#cb4-20"></a>      idleTimeout: 60000</span>
        <span id="cb4-21"><a href="#cb4-21"></a>      connectionTestQuery: SELECT 1</span>
        <span id="cb4-22"><a href="#cb4-22"></a>      validationTimeout: 30000</span>
        <span id="cb4-23"><a href="#cb4-23"></a>      isAutoCommit: false</span></code></pre></div>
        </div>
        </div>
        </div></td>
        </tr>
        </tbody>
        </table>

## Configuring metrics

This section explains how to configure metrics for your status
dashboard.

### Configuring worker metrics

To enable metrics and to configure metric-related properties, do the
following configurations in the \<
`         SP_HOME>/conf/worker/deployment.yaml        ` file for the
required nodes.

1.  To enable Carbon metrics, set the `           enabled          `
    property to `           true          ` under
    `           wso2.metrics          ` as shown below.

    ``` xml
            wso2.metrics:
              enabled: true
    ```

2.  To enable JDBC reporting, set the
    `           Enable JDBC parameter          ` to
    `           true          ` in the
    `           wso2.metrics.jdbc:          ` =\>
    `           reporting:          ` subsection as shown below. If JDBC
    reporting is not enabled, only real-time metrics are displayed in
    the first page of the Status dashboard, and information relating to
    metrics history is not displayed in the other pages of the
    dashboard. To render the first entry of the graph, you need to wait
    for the time duration specified as the
    `           pollingPeriod          ` .

    ``` xml
            # Enable JDBC Reporter
             name: JDBC
             enabled: true
             pollingPeriod: 60
    ```

3.  Under `           wso2.metrics.jdbc          ` , configure the
    following properties to clean up the database entries.  

    ``` xml
            wso2.metrics.jdbc:
              # Data Source Configurations for JDBC Reporters
              dataSource:
                # Default Data Source Configuration
                - &JDBC01
                  # JNDI name of the data source to be used by the JDBC Reporter.
                  # This data source should be defined in a *-datasources.xml file in conf/datasources directory.
                  dataSourceName: java:comp/env/jdbc/WSO2MetricsDB
                  # Schedule regular deletion of metrics data older than a set number of days.
                  # It is recommended that you enable this job to ensure your metrics tables do not get extremely large.
                  # Deleting data older than seven days should be sufficient.
                  scheduledCleanup:
                    # Enable scheduled cleanup to delete Metrics data in the database.
                    enabled: false
        
                    # The scheduled job will cleanup all data older than the specified days
                    daysToKeep: 7
        
                    # This is the period for each cleanup operation in seconds.
                    scheduledCleanupPeriod: 86400
    ```

      

    <table>
    <thead>
    <tr class="header">
    <th>Parameter</th>
    <th>Default Value</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>               dataSource              </code></td>
    <td><code>               &amp;JDBC01              </code></td>
    <td><br />
    </td>
    </tr>
    <tr class="even">
    <td><code>               dataSource &gt; dataSourceName              </code></td>
    <td><code>               java:comp/env/jdbc/WSO2MetricsDB              </code></td>
    <td>The name of the datasource used to store metric data.</td>
    </tr>
    <tr class="odd">
    <td><code>               dataSource &gt; scheduledCleanup &gt; enabled              </code></td>
    <td><code>               false              </code></td>
    <td>If this is set to <code>               true              </code> , metrics data stored in the database is cleared at a specific time interval as scheduled.</td>
    </tr>
    <tr class="even">
    <td><code>               dataSource &gt; scheduledCleanup &gt; daysToKeep              </code></td>
    <td><code>               3              </code></td>
    <td>If scheduled clean-up of metric data is enabled, all metric data in the database that are older than the number of days specified in this parameter are deleted.</td>
    </tr>
    <tr class="odd">
    <td><code>               dataSource &gt; scheduledCleanup &gt; scheduledCleanupPeriod              </code></td>
    <td><code>               86400              </code></td>
    <td>This parameter specifies the time interval in seconds at which all metric data stored in the database must be cleared.</td>
    </tr>
    </tbody>
    </table>

4.  JVM metrics of which the log level is set to
    `           OFF          ` are not measured by default. If you need
    to monitor one or more of them, add the relevant metric name(s)
    under the `           wso2.metrics:          ` =\>
    `           levels          ` subsection as shown in the extract
    below. As shown below, you also need to mention log4j mode in which
    the metrics need to be monitored (i.e., `           OFF          ` ,
    `           INFO          ` , `           DEBUG          ` ,
    `           TRACE          ` , or `           ALL          ` ).  

    ``` xml
            wso2.metrics:
             # Enable Metrics
               enabled: true
                jmx:
                  # Register MBean when initializing Metrics
                registerMBean: true
                  # MBean Name
                name: org.wso2.carbon:type=Metrics
                # Metrics Levels are organized from most specific to least:
                # OFF (most specific, no metrics)
                # INFO
                # DEBUG
                # TRACE (least specific, a lot of data)
                # ALL (least specific, all data)
             levels:
                  # The root level configured for Metrics
                rootLevel: INFO
                  # Metric Levels
                levels:
                     jvm.buffers: 'OFF'
                     jvm.class-loading: INFO
                     jvm.gc: DEBUG
                     jvm.memory: INFO
    ```

    ![](images/icons/grey_arrow_down.png){.expand-control-image} Click
    here to view the default metric levels supported...

      

    -   **Class loading**

        Property

    -   **Garbage collector**

        Property

    -   **Memory**

        Property

    -   **Operating system load**

        Property

    -   **Threads**

        | Property                                                               | Default Level                                | Description                                                                                                                                                                                                     |
        |------------------------------------------------------------------------|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
        | `                   jvm.threads.count                  `               | `                   Debug                  ` | The gauge for showing the number of active and idle threads currently available in the JVM thread pool.                                                                                                         |
        | `                   jvm.threads.daemon.count                  `        | `                   Debug                  ` | The gauge for showing the number of active daemon threads currently available in the JVM thread pool.                                                                                                           |
        | `                   jvm.threads.blocked.count                  `       | `                   OFF                  `   | The gauge for showing the number of threads that are currently blocked in the JVM thread pool.                                                                                                                  |
        | `                   jvm.threads.deadlock.count                  `      | `                   OFF                  `   | The gauge for showing the number of threads that are currently deadlocked in the JVM thread pool.                                                                                                               |
        | `                   jvm.threads.new.count                  `           | `                   OFF                  `   | The gauge for showing the number of new threads generated in the JVM thread pool.                                                                                                                               |
        | `                   jvm.threads.runnable.count                  `      | `                   OFF                  `   | The gauge for showing the number of runnable threads currently available in the JVM thread pool.                                                                                                                |
        | `                   jvm.threads.terminated.count                  `    | `                   OFF                  `   | The gauge for showing the number of threads terminated from the JVM thread pool since user started running the WSO2 API Manager instance.                                                                       |
        | `                   jvm.threads.timed_waiting.count                  ` | `                   OFF                  `   | The gauge for showing the number of threads in the Timed\_Waiting state.                                                                                                                                        |
        | `                   jvm.threads.waiting.count                  `       | `                   OFF                  `   | The gauge for showing the number of threads in the Waiting state in the JVM thread pool. One or more other threads are required to perform certain actions before these threads can proceed with their actions. |

    -   **File descriptor details**

        Property

    -   **Swap space**

        Property

### Configuring Siddhi application metrics

To enable Siddhi application level metrics for a Siddhi application, you
need to add the `         @app:statistics        ` annotation bellow the
Siddhi application name in the Siddhi file as shown in the example
below.

``` sql
    @App:name('TestMetrics')
    @app:statistics(reporter = 'jdbc')
    define stream TestStream (message string);
```

The following are the metrics measured for a Siddhi application.

!!! info

The default level after enabling metrics is `         INFO        ` for
all the meytrics listed in the following table.


<table>
<thead>
<tr class="header">
<th>Metric</th>
<th>Components to which the metric is applied</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Latency</td>
<td><ul>
<li>Windows (per window.find and window.add)</li>
<li>Mappers (per sink mapper, source mapper)</li>
<li>Queries (per query)</li>
<li>Tables (per table insert, find, update, updateOrInsert, delete, contains)</li>
</ul></td>
</tr>
<tr class="even">
<td>Throughput</td>
<td><ul>
<li>Windows (per window.find and window.add)</li>
<li>Mappers (per sink mapper, source mapper)</li>
<li>Queries (per query)</li>
<li>Tables (per table insert, find, update, updateOrInsert, delete, contains )</li>
</ul></td>
</tr>
<tr class="odd">
<td>Memory</td>
<td>Queries (per query)</td>
</tr>
<tr class="even">
<td>Buffered Events Count</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>Number of events at disruptor</td>
<td>Streams (per stream)</td>
</tr>
<tr class="even">
<td>Number of events produced/received after restart</td>
<td><ul>
<li>Sources (per source)</li>
<li>Sinks (per sink)</li>
</ul></td>
</tr>
</tbody>
</table>

## Configuring cluster credentials

In order to access the nodes in a cluster and derive statistics, you
need to maintain and share a user name and a password for each node in a
SP cluster. This user name and password must be specified in the
`         <SP_HOME>/conf/dashboard/deployment.yaml        ` file. If you
want to secure sensitive information such as the user name and the
password, you can encrypt them via WSO2 Secure Vault.

1.  To specify the user name and the password to access a node, define
    them under the `           wso2.status.dashboard          ` section
    as shown in the following example.

    ``` xml
        wso2.status.dashboard:  
            workerAccessCredentials:
                username: 'admin'
                password: 'admin'
    ```

2.  To encrypt the user name and the password you defined, define
    aliases for them as described in [Protecting Sensitive Data via the
    Secure
    Vault](https://docs.wso2.com/display/SP440/Protecting+Sensitive+Data+via+the+Secure+Vault)
    .

        !!! info
    
        This functionality is currently supported only for single tenant
        environments.
    

## Configuring permissions

The following are the three levels of permissions that can be granted
for the users of WSO2 Stream Processor Status Dashboard.

<table>
<thead>
<tr class="header">
<th>Permission Level</th>
<th>Granted permissions</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             SysAdmin            </code></td>
<td><ul>
<li>Enabling/disabling metrics</li>
<li>Adding workers</li>
<li>Deleting workers</li>
<li>Viewing workers</li>
</ul></td>
</tr>
<tr class="even">
<td><code>             Developers            </code></td>
<td><ul>
<li>Adding workers</li>
<li>Deleting workers</li>
<li>Viewing workers</li>
</ul></td>
</tr>
<tr class="odd">
<td><code>             Viewers            </code></td>
<td>Viewing workers</td>
</tr>
</tbody>
</table>

The `         admin user        ` in the userstore is assigned the
`         SysAdmin        ` permission level by default.

To assign different permission levels to different roles, you can list
the required roles under the relevant permission level in the
`         wso2.status.dashboard        ` section of the
`         SP_HOME>/conf/dashboard/deployment.yaml        ` file as shown
in the extract below.

``` xml
    wso2.status.dashboard:
      sysAdminRoles:
        - role_1
      developerRoles:
        - role_2
      viewerRoles:
        - role_3
```

!!! info

The display name of the roles given in the configuration must be present
in the user store. To configure user store check, [User
Management](https://docs.wso2.com/display/SP440/User+Management) .

