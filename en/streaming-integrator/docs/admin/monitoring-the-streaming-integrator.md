# Monitoring the Streaming Integrator

The Status Dashboard allows you to monitor the metrics of a stand-alone Streaming Integrator instance or a Streaming Integrator cluster. This involves
monitoring whether all processes of the Streaming Integrator setup are working in a healthy manner, monitoring the current status of a Streaming Integrator node, and viewing
metrics relating to the history of a node or the cluster. Both JVM level metrics or Siddhi application level metrics can be viewed from the Status Dashboard.

The following sections cover how to configure the Status Dashboard and analyze statistics relating to your Streaming Integrator deployment in it.

## Configuring the Status Dashboard

The following sections cover the configurations that need to be done in
order to view statistics relating to the performance of your Streaming Integrator
deployment in the Status Dashboard.

!!! tip "Before you begin""
    - Download the Analytics Dashboard from the [WSO2 Analytics Dashboard Github Repository](https://github.com/wso2/analytics-dashboard/releases).
    - Once you extract the zip file, the extracted location is referred to as the `<DASHBOARD_HOME>` in the rest of this section.
    

### Assigning unique carbon IDs to nodes

Carbon metrics uses the carbon ID as the source ID for metrics. Therefore, all the worker nodes are required to have a **unique carbon ID** defined in the
`wso2.carbon:` section of the `<SI_HOME>/conf/dashboard/deployment.yaml` file as shown in the extract below.

!!! info
    You need to ensure that the carbon ID of each node is unique because it is required for the Status dashboard to identify the worker nodes and display their statistics accordingly.


``` xml
    wso2.carbon:
        # value to uniquely identify a server
      id: wso2-si-1
        # server name
      name: WSO2 Streaming Integrator
        # server type
      type: wso2-si
        # ports used by this server
      ports:
          # port offset
        offset: 0
```



### Setting up the database

To monitor statistics in the Status Dashboard, you need a shared metrics database that stores the metrics of all the nodes.

Set up a database of the required type by following the steps below. In this section, a MySQL database is created as an example.

!!! info
    The Status Dashboard is only supported with H2, MySQL, MSSQL and Oracle database types. It is configured with the H2 database type by default. If you want to continue to use H2, skip this step.


1. Download and install the required database type. For this example, let's download and install [MySQL Server](http://dev.mysql.com/downloads/).

2. Download the required database driver. For this example, download the [MySQL JDBC driver](http://dev.mysql.com/downloads/connector/j/).

3. Unzip the database driver you downloaded, copy its JAR file (`mysql-connector-java-x.x.xx-bin.jar` in this example), and place it in the `<DASHBOARD_HOME>/lib` directory.

4. Enter the following command in a terminal/command window, where `username` is the username you want to use to access the databases.

    `mysql -u username -p`

5. When prompted, specify the password you are using to access the databases with the username you specified.

6. Create two databases named `WSO2_METRICS_DB` (to store metrics data) and `WSO2_STATUS_DASHBOARD_DB` (to store statistics) with tables. To create MySQL databases and tables for this example, run the following commands.

    ``` java
        mysql> create database WSO2_METRICS_DB;
        mysql> use WSO2_METRICS_DB;
        mysql> source <DASHBOARD_HOME>/wso2/portal/dbscripts/metrics/mysql.sql;
        mysql> grant all privileges on WSO2_METRICS_DB.* TO 'username'@'localhost';
    ```

    ``` java
            mysql> create database WSO2_STATUS_DASHBOARD_DB;
            mysql> use WSO2_STATUS_DASHBOARD_DB;
            mysql> source <DASHBOARD_HOME>/wso2/portal/dbscripts/metrics/mysql.sql;
            mysql> grant all privileges on WSO2_STATUS_DASHBOARD_DB.* TO 'username'@'localhost';
    ```
    !!! tip
        - The syntax of the MySQL commands depend on the MySQL version. The given systax is for MySQL 8x versions. Check the syntax for the version you are using when performing this step. 
        - Replace <DASHBOARD_HOME> with the path to your <DASHBOARD_HOME>.
        - Replace `'username'@'localhost'` with your username and host name (e.g., `'root'@'localhost'`)

7. Create two datasources named `WSO2_METRICS_DB` and `WSO2_STATUS_DASHBOARD_DB` by adding the following datasource configurations under the `wso2.datasources:` section of the `<DASHBOARD_HOME>/conf/portal/deployment.yaml` file.

    !!! info
        The names of the data sources must be the same as the names of the database tables you created for metrics and statistics. You need to change the values for the `username` and `password` parameters to the username and password that you are using to access the MySQL database.

        For detailed information about datasources, see [carbon-datasources](https://github.com/wso2/carbon-datasources/).


    - `WSO2_METRICS_DB`

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

    - `WSO2_STATUS_DASHBOARD_DB`

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



    The following are sample configurations for database tables when you use other database types supported.

    ???info "Click here to view the sample data source configurations."
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

### Configuring metrics

This section explains how to configure metrics for your status dashboard.

**Configuring worker metrics**

To enable metrics and to configure metric-related properties, do the
following configurations in the `<SI_HOME>/conf/server/deployment.yaml` file for the
required SI nodes.

1. To enable Carbon metrics, set the `enabled` property to `true` under `wso2.metrics` as shown below.

    ``` xml
            wso2.metrics:
              enabled: true
    ```

2. To enable JDBC reporting, set the `Enable JDBC parameter` to `true` in the `wso2.metrics.jdbc:` -> `reporting:` subsection as shown below. If JDBC reporting
 is not enabled, only real-time metrics are displayed in the first page of the Status dashboard, and information relating to metrics history is not displayed in
  the other pages of the dashboard. To render the first entry of the graph, you need to wait for the time duration specified as the `pollingPeriod`.

    ``` xml
            # Enable JDBC Reporter
             name: JDBC
             enabled: true
             pollingPeriod: 60
    ```

3. Under `wso2.metrics.jdbc` , configure the following properties to clean up the database entries.

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

4. JVM metrics of which the log level is set to `OFF` are not measured by default. If you need to monitor one or more of them, add the relevant metric name(s)
under the `wso2.metrics:` -> `levels` subsection as shown in the extract below. As shown below, you also need to mention log4j mode in which the metrics need to be monitored (i.e., `           OFF          ` ,
`INFO`, `DEBUG`, `TRACE`, or `ALL`).

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

    ???info "Click here to view the default metric levels supported..."
        - **Class loading**

            Property

        - **Garbage collector**

            Property

        - **Memory**

            Property

        - **Operating system load**

            Property

        - **Threads**

            | Property                          | Default Level | Description                                                                                                                                                                                                     |
            |-----------------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
            | `jvm.threads.count`               | `Debug`       | The gauge for showing the number of active and idle threads currently available in the JVM thread pool.                                                                                                         |
            | `jvm.threads.daemon.count`        | `Debug`       | The gauge for showing the number of active daemon threads currently available in the JVM thread pool.                                                                                                           |
            | `jvm.threads.blocked.count`       | `OFF`         | The gauge for showing the number of threads that are currently blocked in the JVM thread pool.                                                                                                                  |
            | `jvm.threads.deadlock.count`      | `OFF`         | The gauge for showing the number of threads that are currently deadlocked in the JVM thread pool.                                                                                                               |
            | `jvm.threads.new.count`           | `OFF`         | The gauge for showing the number of new threads generated in the JVM thread pool.                                                                                                                               |
            | `jvm.threads.runnable.count`      | `OFF`         | The gauge for showing the number of runnable threads currently available in the JVM thread pool.                                                                                                                |
            | `jvm.threads.terminated.count`    | `OFF`         | The gauge for showing the number of threads terminated from the JVM thread pool since user started running the WSO2 API Manager instance.                                                                       |
            | `jvm.threads.timed_waiting.count` | `OFF`         | The gauge for showing the number of threads in the Timed\_Waiting state.                                                                                                                                        |
            | `jvm.threads.waiting.count`       | `OFF`         | The gauge for showing the number of threads in the Waiting state in the JVM thread pool. One or more other threads are required to perform certain actions before these threads can proceed with their actions. |

        - **File descriptor details**

            Property

        - **Swap space**

            Property

**Configuring Siddhi application metrics**

To enable Siddhi application level metrics for a Siddhi application, you need to add the `@app:statistics` annotation below the Siddhi application name in the
Siddhi file as shown in the example below.

``` sql
    @App:name('TestMetrics')
    @app:statistics(reporter = 'jdbc')
    define stream TestStream (message string);
```

The following are the metrics measured for a Siddhi application.

!!! info
    The default level after enabling metrics is `INFO` for all the meytrics listed in the following table.

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

### Configuring cluster credentials

In order to access the nodes in a cluster and derive statistics, you need to maintain and share a user name and a password for each node in a SI cluster. This
user name and password must be specified in the `<DASHBOARD_HOME>/conf/portal/deployment.yaml` file. If you nwant to secure sensitive information such as the
user name and the password, you can encrypt them via WSO2 Secure Vault.

1. To specify the user name and the password to access a node, define them under a new section named `wso2.status.dashboard`as shown in the following example.

    ``` xml
        wso2.status.dashboard:
            workerAccessCredentials:
                username: 'admin'
                password: 'admin'
    ```

2. To encrypt the user name and the password you defined, define aliases for them as described in [Protecting Sensitive Data via the Secure Vault](protecting-sensitive-data-via-the-secure-vault.md).

    !!! info
        This functionality is currently supported only for single tenant environments.


### Configuring permissions

The following are the three levels of permissions that can be granted for the users of the Status Dashboard.

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

The `admin user` in the user store is assigned the `SysAdmin` permission level by default.

To assign different permission levels to different roles, you can list the required roles under the relevant permission level in the `wso2.status.dashboard`
section of the
`DASHBOARD_HOME>/conf/dashboard/deployment.yaml` file as shown in the extract below.

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
    The display name of the roles given in the configuration must be present in the user store. To configure user store check, [User Management](../admin/user-management.md).

## Accessing the Status Dashboard

To access the Status Dashboard, follow the procedure below:

3. In the terminal, navigate to the `<DASHBOARD_HOME>/bin` directory and issue the relevant command to start the dashboard profile based on your operating system.

    - For Windows: `portal.bat`
    - For Linux : `./portal.sh`

4. Access the Status Dashboard via the following URL format.

    `https://localhost:<DASHBOARD_PORT>/si-status-dashboard`

    e.g., <https://0.0.0.0:9643/sp-status-dashboard>

After login this opens  the Status Dashboard with the nodes that you have already added as shown in the example below.

![Status Dashboard Overview](../images/monitoring-the-streaming-integrator/status_dashboard_overview.png)

If no nodes are displayed, add the nodes for which you wnt to view statistics by following the steps in [Adding a node to the dashboard](#adding-a-node-to-the-dashboard).

## Node overview

Once you login to the status dashboard, the nodes that are already added
to the Status Dashboard are displayed as shown in the following example:

### Adding a node to the dashboard

If no nodes are displayed, you can add the nodes for which you want to
view the status by following the procedure below:

1. Click **ADD NEW NODE**.

    ![Add New Node](../images/monitoring-the-streaming-integrator/add-new-node-button.png)

    This opens the following dialog box.

    ![Add New Node dialog box](../images/monitoring-the-streaming-integrator/add-new-node.png)

2. Enter the following information in the dialog box and click **ADD  NODE** to add a gadget for the required node in the **Node Overview** page.

    1. In the **Host** parameter, enter the host ID of the node you want to add.

    2. In the **Port** parameter, enter the port number of the node you want to add.

3. If the node you added is currently unreachable, the following dialog box is displayed.

    ![Unreachable Node](../images/monitoring-the-streaming-integrator/unreachable-node.png)

    Click either **WORKER** or **MANAGER.** If you click **WORKER**, the node is displayed under **Never Reached**. If you click **Manager** , the node is displayed under **Distributed Deployments** as shown below.

    ![Unreachable nodes](../images/monitoring-the-streaming-integrator/distributed-deployment-with-unreachable-manager.png)

!!! info
    The following basic details are displayed for each node.

    -   **CPU Usage** : The CPU resources consumed by the Streaming Integrator node out of the
        available CPU resources in the machine in which it is deployed is
        expressed as a percentage.
    -   **Memory Usage** : The memory consumed by the node as a percentage
        of the total memory available in the system.
    -   **Load Average** :
    -   **Siddhi Apps** : The total number of Siddhi applications deployed
        in the node.


### Viewing status details

The following is a list of sections displayed in the **Node Overview**
page to provide information relating to the status of the nodes.

**Distributed Deployments**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/distributed-deployment-overview.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><div class="content-wrapper">
<p>The nodes that are connected in the distributed deployment are displayed under the relevant group ID in the status dashboard (e.g., <code>               sp              </code> in the above example). Both managers and workers are displayed under separate labels.</p>
<p><strong>Managers</strong> : The active manager node in the cluster is indicated by a green dot that is displayed with the host name and the port of the node. Similarly, a grey dot is displayed for passive manager nodes in the cluster.</p>
<p><strong>Workers</strong> : When you add an active manager node, it automatically retrieves the worker node details that are connected with that particular deployment. If the worker node is already registered in the Status Dashboard, you can view the metrics of that node as follows:<br />
<img src="../../images/monitoring-the-streaming-integrator/worker-metrics.png" /></p>
</div></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td><ul>
<li>To determine whether the request load is efficiently allocated between the nodes of a cluster.</li>
<li>To determine whether the cluster has sufficient resources to handle the load of requests.</li>
<li>To identify the nodes connected with the particular deployment.</li>
</ul></td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If there is a disparity in the CPU usage and the memory consumption of the nodes, redeploy the Siddhi applications between the nodes to balance out the workload.</li>
<li>If the CPU and memory are fully used and and the request load is increasing, allocate more resources (e.g., more memory, more nodes, etc.).</li>
</ul></td>
</tr>
</tbody>
</table>



**Clustered nodes**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/ha-active-passive-nodes.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>The nodes that are clustered together in a high-availability deployment are displayed under the relevant cluster ID in the Status Dashboard (e.g., under <code>              WSO2_A_1             </code> in the above example). The active node in the cluster (i.e., the active worker in a minimum HA cluster or the active manager in a fully distributed cluster) are indicated by a green dot that is displayed with the hostname and the port of the node. Similarly, a grey dot is displayed for passive nodes in the cluster.</p></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td><p>This allows you to determine the following:</p>
<ul>
<li>Whether the request load is efficiently allocated between the nodes of a cluster.</li>
<li>Whether the cluster has sufficient resources to handle the load of requests.</li>
</ul></td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If there is a disparity in the CPU usage and the memory consumption of the nodes, redeploy the Siddhi applications between the nodes to balance out the workload.</li>
<li>If the CPU and memory are fully used and and the request load is increasing, allocate more resources (e.g., more memory, more nodes, etc.).</li>
</ul></td>
</tr>
</tbody>
</table>

**Single nodes**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/single-nodes.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This section displays statistics for Streaming Integrator servers that operate as single node setups.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to compare the performance of single nodes agaisnt each other.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If the CPU usage of a node is too high, investigate the causes for it and take corrective action (e.g., undeploy unnecessary Siddhi applications).</li>
<li>If any underutilized single nodes are identified, you can either deploy more Siddhi applications thatare currrently deployed in other nodes with a high request load. Alternatively, you can redeploy the Siddhi applications of the underutilized node to other nodes, and then shut it down.</li>
</ul></td>
</tr>
</tbody>
</table>



**Nodes that cannot be reached**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/unreachable-nodes.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node is newly added to the Status dashboard and it is unavailable, it is displayed as shown in the above examples.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify nodes that cannot be reached at specific hosts and ports.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Check whether the host and port of the node you added is correct.</li>
<li>Check whether any authentication errors have occured for the node.</li>
</ul></td>
</tr>
</tbody>
</table>



**Nodes that are currently unavailable**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/currently-unreachable-nodes.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node that could be viewed previously is no longer available, its status is displayed in red as shown in the example above. The status displayed for such nodes is applicable for the last time at which the node had been reachable.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify previously available nodes that have become unreachable.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Check whether the node is inactive.</li>
<li>Check whether any authentication errors have occured for the node.</li>
</ul></td>
</tr>
</tbody>
</table>



**Nodes for which metrics is disabled**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/nodes-with-metrics-disabled.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node for which metrics is disabled is added to the Status dashboard, you can view the number of active and inactive Siddhi applications deployed in it. However, you cannot view the CPU usage, memory usage and the load average.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify nodes for which metrics is not enabled.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Enable metrics for the required nodes to view statistics about their status in the Status Dashboard. For instructions to enable metrics, see <a href="Monitoring-Stream-Processor_112391023.html#MonitoringStreamProcessor-Confi">Monitoring the Stream Processor - Configuring the Status Dashboard</a> .</td>
</tr>
</tbody>
</table>



**Nodes with JMX reporting disabled**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/jmx-reporter-disabled.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>When a node with JMX reporting disabled is added to the Status dashboard, you can view the number of active and inactive Siddhi applications deployed in it. However, you cannot view the CPU usage, memory usage and the load average.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to identify nodes for which JMX reporting is disabled</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Enable JMX reporting for the required nodes to view statistics about their status in the Status Dashboard. For instructions to enable JMX reporting, see <a href="Monitoring-Stream-Processor_112391023.html#MonitoringStreamProcessor-Confi">Monitoring the Stream Processor - Configuring the Status Dashboard</a> .</td>
</tr>
</tbody>
</table>



**Statistics trends**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/statistics-trends.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This dispalys the change that has taken taken place in the CPU usage, memory usage and the load average of nodes since the status was last viewed in the status dashboard. Positive changes are indicated in green (e.g., a decrease in the CPU usage in the above example), and egative changes are indicated in red (an increase in the memory usage and the load average in the above example).</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to view a summary of the performance trends of your Streaming Integrator clusters and single nodes.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Based on the performance trend observed, add more resources to your Streaming Integrator clusters/single nodes to handle more events, or shutdown one or more nodes if there is excess resources.</td>
</tr>
</tbody>
</table>


## Viewing node-specific pages

When you open the Status Dashboard, the [Node Overview](#node-overview) page is displayed by default. To view information specific to a selected worker node, click on the relevant
widget. This opens a separate page for the worker node as shown in the example below.

![Worker-specific Details](../../images/monitoring-the-streaming-integrator/worker-specific-details.png)

### Status indicators

The following gadgets can be viewed for the selected worker.

**Server General Details**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/server-general-details.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This gadget displays general information relating to the selected worker node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to understand the distribution of nodes in terms of the location, the time zone, operating system used etc., and to locate them.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>In a distributed set up, you can use this information to evaluate the clustered setup and make changes to optimize the benefits of deploying the Streaming Integrator as a cluster (e.g., making them physically available in different locations to minimize the risk of all the nodes failing at the same time etc.).</td>
</tr>
</tbody>
</table>


**CPU Usage**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/cpu-usage.png" /></p>
<p><br />
</p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the CPU usage of the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to observe the CPU usage of a selected node over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Identify sudden slumps in the CPU usage, and investigate the reasons (e.g., such as authentication errors that result in requests not reaching the Streaming Integrator server).</li>
<li>Identify continuous increases in the CPU usage and check whether the node is overloaded. If so, reallocate some of the Siddhi applications deployed in the node.</li>
</ul></td>
</tr>
</tbody>
</table>


**Memory Used**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="../../images/monitoring-the-streaming-integrator/memory-used.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the memory usage of the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to observe the memory usage of a selected node over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Identify sudden slumps in the memory usage, and investigate the reasons (e.g., a reduction in the requests recived due to system failure).</li>
<li>If there are continous increases in the memory usage, check whether there is an increase in the requests handled, and whether you have enough memory resources to handle the increased demand. If not, add more memory to the node or reallocate some of the Siddhi applications deployed in the node to other nodes.</li>
</ul></td>
</tr>
</tbody>
</table>


**System Load Average**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="../../images/monitoring-the-streaming-integrator/system-load-average.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the system load average for the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to observe the system load of the node over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>Observe the trends of the node's system load, and adjust the allocation of resources (e.g., memory) and work load (i.e., the number of Siddhi applications deployed) accordingly.</p></td>
</tr>
</tbody>
</table>

**Overall Throughput**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="../../images/monitoring-the-streaming-integrator/overall-throughput.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the overall throughput of the selected node.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of the selected node in terms of the throughput over time.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Compare the throughput of the node against that of other nodes with the same amount of CPU and memory resources. If there are significant variations, investigate the causes (e.g., the differences in the number of requests received by different Siddhi applications deployed in the nodes).</li>
<li>Observe changes in the throughput over time. If there are significant variances, investigate the causes (e.g., whether the node has been unavaialable to receive requests during a given time).</li>
</ul></td>
</tr>
</tbody>
</table>


**Siddhi Applications**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><img src="../../images/monitoring-the-streaming-integrator/siddhi-applications.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This table displays the complete list of Siddhi applications deployed in the selected node. The status is displayed in green for active Siddhi applications, and in red for inactive Siddhi applications. In addition, the following is displayed for each Siddhi application:</p>
<ul>
<li><strong>Age</strong> : The age of the Siddhi application in milliseconds.</li>
<li><strong>Latency</strong> : The time (in milliseconds) taken by the Siddhi application to process one request.</li>
<li><strong>Throughput:</strong> The number of requests processed by the Siddhi application since it has been active.</li>
<li><strong>Memory</strong> : The amount of memory consumed by the Siddhi application during its current active session, expressed in milliseconds.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of each Siddhi application deployed in the selected node.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>Identify the inactive Siddhi applications that are required to be active and take the appropriate corrective action.</li>
<li>Identify Siddhi applications that consume too much memory, and identify ways in which the memory usage can be optimized (e.g., use incremental processing).</li>
</ul></td>
</tr>
</tbody>
</table>


## Viewing worker history

This section explains how to view statistics relating to the performance of a selected node for a specific time interval.

1. Log in to the Status Dashboard.

2. In the **Node Overview** page, click on the required node to view information specific to that node.

3. In the page displayed with node-specific information, click one of the following gadgets to open the **Metrics** page.

    -   **CPU Usage**

    -   **Memory Used**

    -   **System Load Average**

    -   **Overall Throughput**

4. In the **Metrics** page, click the required time interval. Then the page displays statistics relating to the performance of the selected node applicable to that time period.

    ![Select Time Interval](../../images/monitoring-the-streaming-integrator/select-time-interval.png)

5. If you want to view more details, click **More Details**.

    ![More Details About Node Performance](../../images/monitoring-the-streaming-integrator/more-details-node-performance.png)

    As a result, the following additional information is displayed for the node for the selected time period.

    - **CPU Usage**

        ![CPU Usage](../../images/monitoring-the-streaming-integrator/worker-cpu-usage.png)

    - **JVM OS as CPU**

        ![JVM OS as CPU](../../images/monitoring-the-streaming-integrator/jvm-os-cpu.png)

    - **JVM Physical Memory**

        ![JVM Physical Memory](../../images/monitoring-the-streaming-integrator/jvm-physical-memory.png)

    - **JVM Threads**

        ![JVM Threads](../../images/monitoring-the-streaming-integrator/jvm-threads.png)

    - **JVM Swap Space**

        ![JVM Swap Space](../../images/monitoring-the-streaming-integrator/jvm-swap-space.png)


## Viewing statistics for Siddhi applications

When you open the WSO2 Status Dashboard, the [Node Overview](#node-overview) page is displayed by default.

1. To view information specific to a selected worker node, click on the relevant gadget. This opens the [page specific to the worker](#viewing-node-specific-pages).

2. To view information specific to a Siddhi application deployed in the Siddhi node, click on the relevant Siddhi application in the Siddhi Applications table. This opens a page with information specific to the selected Siddhi application as shown in the example below.

    ![Siddhi Application Statistics](../../images/monitoring-the-streaming-integrator/siddhi-application-statistics.png)

The following statistics can be viewed for an individual Siddhi Application.

**Latency**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/latency.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the latency of the selected Siddhi application. Latency is the time taken to complete processing a single event in the event flow.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of the selected Siddhi application.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>If the latency of the Siddhi application is too high, check the Siddhi queries and rewrite them to optimise performance.</td>
</tr>
</tbody>
</table>



**Overall Throughput**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/siddhi-application-overall-throughput.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This shows the overall throughput of a selected Siddhi application over time.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to assess the performance of the selected Siddhi application.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><ul>
<li>If the throughput of a Siddhi application varies greatly overtime, investigate reasons for any slumps in the throughput (e.g., errors in the deployment of the application).</li>
<li>If the throughput of the Siddhi application is lower than expected, investigate reasons, and take corrective action to improve the throughput (e.g., check the Siddhi queries in the application and rewrite them with best practices to achieve greater efficiency in the processing of events.</li>
</ul></td>
</tr>
</tbody>
</table>

**Memory Used**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/siddhi-application-memory-used.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the memory usage (In MB) of a selected Siddhi application over time.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to monitor the memory consumption of individual Siddhi applications.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>If there are major fluctuations in the memory consumption of a Siddhi application, investigate the reasons (e.g., Whether the Siddhi application has been inactive at any point of time).</p></td>
</tr>
</tbody>
</table>

**Code View**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/siddhi-application-code-view.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the queries defined in the Siddhi file of the application.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to check the queries of the Siddhi application if any further investigations are needed based on the observations of its latency, throughput and the memory consumption.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>Edit the Siddhi file if any changes that can improve the performance of the Siddhi application are identified.<br />
For detailed instructions to write a Siddhi application, see <a href="../develop/creating-a-Siddhi-Application.md">Creating a Siddhi Application</a>.<br />
For detailed information about the Siddhi logic, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a>.</p>
<p><br />
</p></td>
</tr>
</tbody>
</table>

**Design View**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/design-view.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This displays the graphical view for queries defined in the Siddhi file of the application.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to check the flow of the queries of the Siddhi application in the graphical way.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Edit the Siddhi file if any changes that can improve the performance of the Siddhi application are identified.</td>
</tr>
</tbody>
</table>



**Siddhi App Component Statistics**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/siddhi-application-component-statistics.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This table displays performance statistics related to dfferent components within a selected Siddhi application (e.g., queries). The columns displayed are as follows:</p>
<ul>
<li><strong>Type</strong> : The type of the Siddhi application component to which the information displayed in the row applies. The component type can be queries, streams, tables, windows and aggregations. For more information, see <a href="https://docs.wso2.com/display/SP440/Siddhi+Application+Overview#SiddhiApplicationOverview-Components">Siddhi Application Overview - Common components of a Siddhi application</a> .</li>
<li><strong>Name</strong> : The name of the Siddhi component within the application to which the information displayed in the row apply.</li>
<li><strong>Metric Type</strong> : The metric type for which the statistics are displayed. This can be either the latency (in milliseconds), throughput the number of events per second), or the amount of memory consumed (in bytes). The metric types based on which the performance of a Siddhi component is measured depends on the component type.</li>
<li><strong>Attribute</strong> : The attribute to which the given value applies.</li>
<li><strong>Value</strong> : The value for the metric type given in the row.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to carry out a detailed analysis of the performance of a selected Siddhi application and identify components that have a negative impact on the overall performance of the Siddhi application.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td>Identify the componets in a Siddhi application that have a negative impact on the performance, and rewrite them to improve performance. To understand Siddhi concepts in order to rewrite the components, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</td>
</tr>
</tbody>
</table>


## Viewing statistics for parent Siddhi applications

When you open the WSO2 Status Dashboard, the [Node Overview](#node-overview) page is displayed by default. To view information specific to an active manager, click on the required active manager node in the **Distributed Deployments** section. This opens a page with parent Siddhi applications deployed in that manager node as shown in the example below.

![Parent Siddhi Applications](../../images/monitoring-the-streaming-integrator/parent-siddhi-applications.png)

This page provides a summary of information relating to each parent Siddhi application as described in the table below. If a parent Siddhi application is active, it is indicated with a green dot that appears before the name of the Siddhi application. Similarly, an orange dot is displayed for inactive parent Siddhi applications.

<table>
<thead>
<tr class="header">
<th>Detail</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Groups</strong></td>
<td>This indicates the number of execution groups of the parent Siddhi application. In the above example, the <code>             Testing            </code> Siddhi application has only one execution group.</td>
</tr>
<tr class="even">
<td><strong>Child Apps</strong></td>
<td>This indicates the number of child applications of the parent Siddhi application. The number of active child applications is displayed in green, and the number of inactive child applications are displayed in red.</td>
</tr>
<tr class="odd">
<td><strong>Worker Nodes</strong></td>
<td><p>The number displayed in yellow indicates the total number of worker nodes in the resource cluster. In the above example, there are two worker nodes in the cluster.</p>
<p>The number displayed in green indicates the number of worker nodes in which the Siddhi application is deployed. In the above example, the <code>              Testing             </code> parent Siddhi application is deployed only in one worker node although there are two worker nodes in the resource cluster.</p></td>
</tr>
</tbody>
</table>

If you click on a parent Siddhi application, detailed information is
displayed as shown below.

![Parent Siddhi Application Details](../../images/monitoring-the-streaming-integrator/parent-siddhi-application-details.png)

The following are the widgets displayed.

**Code View**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/parent-application-code-view.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This displays the queries defined in the Parent Siddhi file of the application.</p>
<p>This allows you to check the queries of the Siddhi application if any further investigations are needed based on the kafka diagrams and performance.<br />
For detailed instructions to write a Siddhi application, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> .</p>
<p>For detailed information about the Siddhi logic, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</p></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This allows you to check the queries of the Siddhi application if any further investigations are needed based on the observations of the performance of the distributed cluster to which it belongs.</td>
</tr>
<tr class="even">
<td>Recommended Action</td>
<td><p>Edit the Siddhi file if any changes that can improve the performance of the Siddhi application are identified.<br />
For detailed instructions to write a Siddhi application, see <a href="https://docs.wso2.com/display/SP440/Converting+to+a+Distributed+Streaming+Application">Converting to a Distributed Streaming Application</a> .<br />
For detailed information about the Siddhi logic, see the <a href="https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/">Siddhi Query Guide</a> .</p></td>
</tr>
</tbody>
</table>

**Distributed Siddhi App Deployment**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/distributed-siddhi-application-deployment.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td>This is a graphical representation of how Kafka topics are connected to the child Siddhi applications of the selected parent Siddhi application. Kafka topics are represented by boxes with red margins, and the child applications are represented by boxes with blue margins.</td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>This is displayed for you to understand how the flow of information takes place.</td>
</tr>
</tbody>
</table>

**Child App Details**

<table>
<tbody>
<tr class="odd">
<td>View (Example)</td>
<td><div class="content-wrapper">
<p><img src="../../images/monitoring-the-streaming-integrator/child-application-details.png" /></p>
</div></td>
</tr>
<tr class="even">
<td>Description</td>
<td><p>This table displays the complete list of child Siddhi applications of the selected parent Siddhi application. The status is displayed in green for active Siddhi applications, and in red for inactive Siddhi applications. In addition, the following is displayed for each Siddhi application:</p>
<ul>
<li><strong>Group Name</strong> : The name of the execution group to which the child application belongs.</li>
<li><strong>Child App Status</strong> : This indicates whether the child application is currently active or not.</li>
<li><strong>Worker Node</strong> : The HTTPS host and The HTTPS port of the worker node in which the child siddhi application is deployed.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Purpose</td>
<td>To identify the currently active child applications.</td>
</tr>
</tbody>
</table>

## Application overview

When you open the WSO2 Status Dashboard, the [Node Overview](#node-overview) page is displayed by default. If you want to view all the Siddhi applications deployed in your Streaming Integrator setup, click on the **App View** tab (marked in the image below). The **App Overview** tab opens and all the Siddhi applications that are currently deployed are displayed as shown in the image below.

![Application Overview](../../images/monitoring-the-streaming-integrator/app-overview.png)

The status is displayed in green for active Siddhi applications, and in red for inactive Siddhi applications.

If no Siddhi applications are deployed in your Streaming Integrator setup, the following message is displayed.

![No Siddhi Applications Deployed](../../images/monitoring-the-streaming-integrator/no-siddhi-applications-deployed.png)


The Siddhi applications are listed under the deployment mode in which they are deployed (i.e., **Single Node Deployment**, **HA Deployment**, and **Scalable Deployment**).

The following information is displayed for each Siddhi application.

- **Siddhi Application**: The name of the Siddhi application.

- **Status**: This indicates whether the Siddhi application is currently active or inactive.

- **Deployed Time**: The time duration that has elapsed since the Siddhi application was deployed in the Streaming Integrator setup.

- **Deployed Node**: The host and the port of the Streaming Integrator node in which the Siddhi application is displayed.

The purpose of this tab is to check the status of all the Siddhi applications that are currently deployed in the Streaming Integrator setup.

If you click on a Siddhi Application under **Single Node Deployment** or **HA Deployment**, information specific to that Siddhi application is displayed as explained in [Viewing Statistics for Siddhi Applications](#viewing-statistics-for-siddhi-applications).

If you click on the parent Siddhi application under **Distributed Deployment**, information specific to that parent Siddhi application is
displayed as explained in [Viewing Statistics for Parent Siddhi Applications](#viewing-statistics-for-parent-siddhi-applications).

If you click on a deployed node, information specific to that node is displayed as explained in [Viewing Node-specific Pages](#viewing-node-specific-pages).