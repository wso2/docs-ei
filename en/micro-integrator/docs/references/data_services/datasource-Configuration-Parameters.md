# Datasource Configuration Parameters

When a database operation is processed, the server spawns a database
connection from an associated datasource. After using this connection,
the server returns it to the pool of connections. The physical
connection is not dropped with the database server unless it becomes
stale or the datasource connection is closed. This is called
**datasource connection pooling** and is a recommended way to gain more
performance/throughput in the system. RDBMS datasources in the server
use Tomcat JDBC connection pool (
`         org.apache.tomcat.jdbc.pool        ` ). It is common to all
components that access databases for data persistence, such as the
registry, user management (if configured against a JDBC userstore), etc.

You can configure the datasource connection pool parameters, such as how
long a connection is persisted in the pool, using the **datasource
configuration parameters** section that appears in the management
console when creating a datasource.

![](attachments/29919828/29897825.png){width="600"}

Each parameter is described below:

!!! note

The default values of the following parameters might not be optimal for
the specific hardware/server configurations in your environment. We
recommend that you carry out load tests in your environment to tune them
accordingly. See [Tuning Datasource Configuration
Parameters](_Tuning_Datasource_Configuration_Parameters_) for
information on how you can select the best values for these parameters.


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Transaction Isolation</td>
<td>The default TransactionIsolation state of connections created by this pool.
<ul>
<li>TRANSACTION_UNKNOWN</li>
<li>TRANSACTION_NONE</li>
<li>TRANSACTION_READ_COMMITTED</li>
<li>TRANSACTION_READ_UNCOMMITTED</li>
<li>TRANSACTION_REPEATABLE_READ</li>
<li>TRANSACTION_SERIALIZABLE</li>
</ul></td>
</tr>
<tr class="even">
<td>Initial Size</td>
<td><p>(int)</p>
<p>The initial number of connections created when the pool is started. Default value is 0.</p></td>
</tr>
<tr class="odd">
<td>Max. Active</td>
<td><p>(int)</p>
<p>The maximum number of active connections that can be allocated from this pool at the same time. The default value is 100.</p>
<p>If you set this value too low, the response times for some requests might slow down as they have to wait for connections to get free. A value too high might cause too much memory and resource utilization and the system to slow down or be unresponsive.</p></td>
</tr>
<tr class="even">
<td>Max. Idle</td>
<td><p>(int)</p>
<p>The maximum number of connections that can remain idle in the pool, without extra ones being released. Default value is 8. Put a negative value for unlimited. Idle connections are checked periodically (if enabled) and connections that have been idle for longer than minEvictableIdleTimeMillis will be released. (also see <a href="#DatasourceConfigurationParameters-TestWhileIdle">testWhileIdle</a> )</p></td>
</tr>
<tr class="odd">
<td>Min. Idle</td>
<td><p>(int)</p>
<p>The minimum number of connections that can remain idle in the pool, without extra ones being created. The connection pool can shrink below this number if validation queries fail. Default value is 0. (also see <a href="#DatasourceConfigurationParameters-TestWhileIdle">testWhileIdle</a> )</p></td>
</tr>
<tr class="even">
<td>Max. Wait</td>
<td><p>(int)</p>
<p>Maximum number of milliseconds that the pool waits (when there are no available connections) for a connection to be returned before throwing an exception. Default value is 30000 (30 seconds).</p></td>
</tr>
<tr class="odd">
<td>Validation Query</td>
<td><p>(String)</p>
<p>The SQL query used to validate connections from this pool before returning them to the caller. If specified, this query does not have to return any data, it just can't throw an SQLException. The default value is null. Example values are SELECT 1(mysql), select 1 from dual(oracle), SELECT 1(MS Sql Server).</p></td>
</tr>
<tr class="even">
<td>validationQueryTimeout</td>
<td><p>(int)</p>
<p>The timeout in seconds before a connection validation queries fail. This works by calling <code>              java.sql.Statement.setQueryTimeout(seconds)             </code> on the statement that executes the <code>              validationQuery             </code> . The pool itself doesn't timeout the query. It is still up to the JDBC driver to enforce query timeouts. A value less than or equal to zero will disable this feature. The default value is <code>              -1             </code> .</p></td>
</tr>
<tr class="odd">
<td>Test On Return</td>
<td><p>(boolean)</p>
<p>Used to indicate if objects will be validated before being returned to the pool. NOTE - for a true value to have any effect, the validationQuery parameter must be set to a non-null string. The default value is false.</p></td>
</tr>
<tr class="even">
<td>Test On Borrow</td>
<td><p>(boolean)</p>
<p>Used to indicate if objects will be validated before being borrowed from the pool. If the object fails to validate, it will be dropped from the pool, and we will attempt to borrow another one. NOTE - for a true value to have any effect, the validationQuery parameter must be set to a non-null string. In order to have a more efficient validation, see <a href="#DatasourceConfigurationParameters-ValidationInterval">validationInterval</a> . Default value is false.</p></td>
</tr>
<tr class="odd">
<td>Test While Idle</td>
<td><p>(boolean)</p>
<p>The indication of whether objects will be validated by the idle object evictor (if any). If an object fails to validate, it will be dropped from the pool. NOTE - for a true value to have any effect, the <a href="#DatasourceConfigurationParameters-ValidationQuery">validationQuery</a> parameter must be set to a non-null string. The default value is false and this property has to be set in order for the pool cleaner/test thread to run (also see <a href="#DatasourceConfigurationParameters-timeBetweenEvictionRunsMillis">timeBetweenEvictionRunsMillis</a> ).</p></td>
</tr>
<tr class="even">
<td>Time Between Eviction Runs Mills</td>
<td><p>(int)</p>
<p>The number of milliseconds to sleep between runs of the idle connection validation/cleaner thread. This value should not be set under 1 second. It dictates how often we check for idle, abandoned connections, and how often we validate idle connections. The default value is 5000 (5 seconds).</p></td>
</tr>
<tr class="odd">
<td>Number of Tests Per Eviction Run</td>
<td><p>(int)</p>
<p>The number of objects to examine during each run of the idle object evictor thread.</p></td>
</tr>
<tr class="even">
<td>Minimum Evictable Idle Time</td>
<td><p>(int)</p>
<p>The minimum amount of time an object may sit idle in the pool before it is eligible for eviction. The default value is 60000 (60 seconds).</p></td>
</tr>
<tr class="odd">
<td>Remove Abandoned</td>
<td><p>(boolean)</p>
<p>Flag to remove abandoned connections if they exceed the <a href="#DatasourceConfigurationParameters-removeAbandonedTimout">removeAbandonedTimout</a> . If set to true a connection is considered abandoned and eligible for removal if it has been in use longer than the removeAbandonedTimeout. Setting this to true can recover db connections from applications that fail to close a connection. See also logAbandoned. The default value is false.</p></td>
</tr>
<tr class="even">
<td>Remove Abandoned Timeout</td>
<td>(int) Timeout in seconds before an abandoned(in use) connection can be removed. The default value is 60 (60 seconds). The value should be set to the longest running query your applications might have.</td>
</tr>
<tr class="odd">
<td>Log Abandoned</td>
<td>(boolean) Flag to log stack traces for application code which abandoned a Connection. Logging of abandoned Connections adds overhead for every Connection borrow because a stack trace has to be generated. The default value is false.</td>
</tr>
<tr class="even">
<td>Auto Commit</td>
<td>(boolean) The default auto-commit state of connections created by this pool. If not set, default is JDBC driver default (If not set then the setAutoCommit method will not be called.)</td>
</tr>
<tr class="odd">
<td>Default Read Only</td>
<td>(boolean) The default read-only state of connections created by this pool. If not set then the setReadOnly method will not be called. (Some drivers don't support read-only mode, ex: Informix)</td>
</tr>
<tr class="even">
<td>Default Catalog</td>
<td>(String) The default catalog of connections created by this pool.</td>
</tr>
<tr class="odd">
<td>Validator Class Name</td>
<td>(String) The name of a class that implements the org.apache.tomcat.jdbc.pool.Validator interface and provides a no-arg constructor (may be implicit). If specified, the class will be used to create a Validator instance, which is then used instead of any validation query to validate connections. The default value is null. An example value is com.mycompany.project.SimpleValidator.</td>
</tr>
<tr class="even">
<td>Connection Properties</td>
<td>(String) The connection properties that will be sent to our JDBC driver when establishing new connections. Format of the string must be [propertyName=property;]* NOTE - The "user" and "password" properties will be passed explicitly, so they do not need to be included here. The default value is null.</td>
</tr>
<tr class="odd">
<td>Init SQL</td>
<td>The ability to run a SQL statement exactly once, when the connection is created.</td>
</tr>
<tr class="even">
<td>JDBC Interceptors</td>
<td>Flexible and pluggable interceptors to create any customizations around the pool, the query execution and the result set handling.</td>
</tr>
<tr class="odd">
<td>Validation Interval</td>
<td>(long) avoid excess validation, only run validation at most at this frequency - time in milliseconds. If a connection is due for validation, but has been validated previously within this interval, it will not be validated again. The default value is 30000 (30 seconds).</td>
</tr>
<tr class="even">
<td>JMX Enabled</td>
<td>(boolean) Register the pool with JMX or not. The default value is true.</td>
</tr>
<tr class="odd">
<td>Fair Queue</td>
<td>(boolean) Set to true if you wish that calls to getConnection should be treated fairly in a true FIFO fashion. This uses the org.apache.tomcat.jdbc.pool.FairBlockingQueue implementation for the list of the idle connections. The default value is true. This flag is required when you want to use asynchronous connection retrieval. Setting this flag ensures that threads receive connections in the order they arrive. During performance tests, there is a very large difference in how locks and lock waiting is implemented. When fairQueue=true there is a decision making process based on what operating system the system is running. If the system is running on Linux (property os.name=Linux. To disable this Linux specific behavior and still use the fair queue, simply add the property org.apache.tomcat.jdbc.pool.FairBlockingQueue.ignoreOS=true to your system properties before the connection pool classes are loaded.</td>
</tr>
<tr class="even">
<td>Abandon When Percentage Full</td>
<td>(int) Connections that have been abandoned (timed out) wont get closed and reported up unless the number of connections in use are above the percentage defined by abandonWhenPercentageFull. The value should be between 0-100. The default value is 0, which implies that connections are eligible for closure as soon as removeAbandonedTimeout has been reached.</td>
</tr>
<tr class="odd">
<td>Max Age</td>
<td>(long) Time in milliseconds to keep this connection. When a connection is returned to the pool, the pool will check to see if the now - time-when-connected &gt; maxAge has been reached, and if so, it closes the connection rather than returning it to the pool. The default value is 0, which implies that connections will be left open and no age check will be done upon returning the connection to the pool.</td>
</tr>
<tr class="even">
<td>Use Equals</td>
<td>(boolean) Set to true if you wish the ProxyConnection class to use String.equals and set to false when you wish to use == when comparing method names. This property does not apply to added interceptors as those are configured individually. The default value is true.</td>
</tr>
<tr class="odd">
<td>Suspect Timeout</td>
<td>(int) Timeout value in seconds. Default value is 0. Similar to to the removeAbandonedTimeout value but instead of treating the connection as abandoned, and potentially closing the connection, this simply logs the warning if logAbandoned is set to true. If this value is equal or less than 0, no suspect checking will be performed. Suspect checking only takes place if the timeout value is larger than 0 and the connection was not abandoned or if abandon check is disabled. If a connection is suspect a WARN message gets logged and a JMX notification gets sent once.</td>
</tr>
<tr class="even">
<td>Alternate User Name Allowed</td>
<td>(boolean) By default, the jdbc-pool will ignore the DataSource.getConnection(username,password) call, and simply return a previously pooled connection under the globally configured properties username and password, for performance reasons.<br />
<br />
The pool can however be configured to allow use of different credentials each time a connection is requested. To enable the functionality described in the DataSource.getConnection(username,password) call, simply set the property alternateUsernameAllowed to true. Should you request a connection with the credentials user1/password1 and the connection was previously connected using different user2/password2, the connection will be closed, and reopened with the requested credentials. This way, the pool size is still managed on a global level, and not on a per schema level. The default value is false.</td>
</tr>
</tbody>
</table>

Also see <http://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html> and
DBCP configuration guide in
<http://commons.apache.org/proper/commons-dbcp/configuration.html> .
