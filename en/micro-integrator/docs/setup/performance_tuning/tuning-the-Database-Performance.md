# Tuning the Database Performance

This section explains how to optimize the database performance of the
Message Broker profile by configuring various parameters in the
`         <EI_HOME>/wso2/broker/conf/datasources/master-datasources.xml        `
file.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Improvement Area</th>
<th>Description</th>
<th>Performance Recommendations</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>The maximum number of active connections</td>
<td>This is specified by entering a value for the <code>             maxActive            </code> parameter.</td>
<td>This should be set in proportion to the number of messages published and consumed. If there are too many active connections in the connection pool, some of them would be idle, incurring an unnecessary system overhead. The default/recommended value is 100.</td>
</tr>
<tr class="even">
<td>The maximum waiting time.</td>
<td>The <code>             maxWait            </code> parameter specifies the maximum number of milliseconds the Message Broker waits before an existing active connection is returned in order to make a call to the database when no active connections are currently available. An exception is thrown once this time duration has elapsed.</td>
<td>A lower number of milliseconds can be set if you want the database to be updated faster. If you increase the waiting time, it will reduce the performance of the Message Broker in terms of the <a href="_Tuning_the_Message_Publisher_and_Consumer_Performance_">publish/consumption rate</a> . The default/recommended value is 30000.</td>
</tr>
<tr class="odd">
<td>Testing objects borrowed from the pool</td>
<td>Select <code>             true            </code> or <code>             false            </code> for the <code>             testOnBorrow            </code> parameter to indicate whether a validation test should be performed on objects in the pool before borrowing them.</td>
<td>This parameter should be set to <code>             true            </code> if it is important to ensure the reliability of connections borrowed. However, performing validation tests would incur a system overhead. The default/recommended value is <code>             false            </code> .</td>
</tr>
<tr class="even">
<td>Validation interval</td>
<td>The <code>             validationInterval            </code> parameter specifies the minimum number of milliseconds that should elapse after a validation test performed on an object in the connection pool before another test is performed. This parameter is relevant only if the <code>             testOnBorrow            </code> parameter is set to <code>             true            </code> .</td>
<td>The purpose of the parameter is to control system overhead that can be caused by excessive validation tests. the default/recommended value is 30000 milliseconds. You can increase this value to minimise the system overhead or reduce it if it is more important to ensure the reliability of the connections.<br />
<br />
</td>
</tr>
</tbody>
</table>
