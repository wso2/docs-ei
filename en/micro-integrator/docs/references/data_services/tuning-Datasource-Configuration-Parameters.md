# Tuning Datasource Configuration Parameters

When you [configure datasource
connections](_Datasource_Configuration_Parameters_) , ensure that the
parameters relevant to connection pooling are tuned according to your
production environment. Consider the following when tuning:

-   The application's concurrency requirement.
-   The average time taken to run a database query.
-   The maximum number of connections the database server can support.

The goal of tuning the pool properties is to maintain a pool that is
large enough to handle the peak load to the datasource without
unnecessarily utilising resources. The following are the highly used,
important properties:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property Name</th>
<th>How to Tune</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Min. Idle</td>
<td>Configure this to match the expected minimum concurrency. The default value is 0.</td>
</tr>
<tr class="even">
<td>Max. Idle</td>
<td>The value should be less than the Max. Active value. For high performance, tune Max. Idle to match the number of average, concurrent requests to the pool. If this value is set to a large value, the pool will contain unnecessary idle connections.</td>
</tr>
<tr class="odd">
<td>Max. Active</td>
<td><p>The maximum latency (approximately) = (P / M) * T ,</p>
<p><em>where,</em></p>
<ul>
<li>M = Max. Active value</li>
<li>P = Peak concurrency value</li>
<li>T = Time (average) taken to process a query.</li>
</ul>
<p>Therefore, by increasing the Max. Active value (up to the expected highest number of concurrency), the time that requests wait in the queue for a connection to be released will decrease. But before increasing the Max. Active value, consult the database administrator, as it will create up to Max.Active connections at burst times from a single node, and it may not be possible for the DBMS to handle the accumulated count of these active connections.</p></td>
</tr>
<tr class="even">
<td>Max. Wait</td>
<td><p>Adjust this to a value slightly higher than the maximum latency for a request.</p>
<p>That is, Max. Wait = (P / M) * T + buffer time.</p></td>
</tr>
<tr class="odd">
<td>Validation Query</td>
<td>Set "Validation Query" to a simple test query like SELECT 1.</td>
</tr>
<tr class="even">
<td>Test On Borrow</td>
<td>When the connection to the database is broken, the connection pool does not know that the connection has been lost. As a result, the connection pool will continue to distribute connections to the application until the application actually tries to use the connection. To resolve this problem, set "Test On Borrow" to "true" and make sure that the "Validation Query" property is set.</td>
</tr>
<tr class="odd">
<td>validationInterval</td>
<td><p>This parameter allows to control how frequently a given validation query is executed. By default it is set to 30 seconds. Deciding the value for the "validationInterval" depends on the target application behavior. If a larger value is set, the frequency of executing the Validation Query is low, which results in better performance (but for values larger than several seconds, this becomes negligible, as the validation query usually executes very fast). With a smaller value, a stale connection will be identified quickly when it is presented. This maybe required for systems where the connections should be instantly repaired in a case like a database server restart. Therefore, selecting a value for the this property is a trade-off and ultimately depends on what is acceptable for the application.</p></td>
</tr>
</tbody>
</table>

The following properties are used for detecting and removing the
connections that get leaked from the connection pool:

| Property Name            | How to configure                                                                                                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Remove Abandoned         | If this property is set to 'true', a connection is considered abandoned and eligible for removal if it has been in use for longer than the `              removeAbandonedTimeout             ` value explained below.                                              |
| Remove Abandoned Timeout | The time in seconds that should pass before a connection that is in use can be removed. This is the time period after which the connection will be declared abandoned. This value should be set to the longest running query that the applications might have.     |
| Log Abandoned            | Set this property to "true" if you wish to log when the connection was abandoned. If this option is set to "true", a stack trace is recorded during the `             dataSource.getConnection            ` call and is printed when a connection is not returned. |
