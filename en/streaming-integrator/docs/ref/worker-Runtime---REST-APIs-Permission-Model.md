# Worker Runtime - REST APIs Permission Model

There are two sets of REST APIs available in worker runtime. Stream
Processor APIs and Event Simulator APIs have following permission model.
You need to have appropriate permission to invoke these APIS.

## Stream Processor  APIs

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>API Context</th>
<th>Required Permission</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>POST</td>
<td>/siddhi-apps</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>PUT</td>
<td>/siddhi-apps</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="odd">
<td>DELETE</td>
<td>/siddhi-apps/{appName}</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>GET</td>
<td>/siddhi-apps</td>
<td><p>PermissionString - siddhiApp.manage or siddhiApp.view</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="odd">
<td>GET</td>
<td>/siddhi-apps/{appName}</td>
<td><p>PermissionString - siddhiApp.manage or siddhiApp.view</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>GET</td>
<td>/siddhi-apps/{appName}/status</td>
<td><p>PermissionString - siddhiApp.manage or siddhiApp.view</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/siddhi-apps/{appName}/backup</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>POST</td>
<td><p>/siddhi-apps/{appName}/restore</p>
<p>/siddhi-apps/{appName}/restore?version=</p></td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="odd">
<td>GET</td>
<td>/statistics</td>
<td><p>PermissionString - siddhiApp.manage or siddhiApp.view</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>PUT</td>
<td>/statistics</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="odd">
<td>GET</td>
<td>/system-details</td>
<td><p>PermissionString - siddhiApp.manage or siddhiApp.view</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>GET</td>
<td>/siddhi-apps/statistics</td>
<td><p>PermissionString - siddhiApp.manage or siddhiApp.view</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="odd">
<td>PUT</td>
<td>/siddhi-apps/{appName}/statistics</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
<tr class="even">
<td>PUT</td>
<td>/siddhi-apps/statistics</td>
<td><p>PermissionString - siddhiApp.manage</p>
<p>AppName - SAPP</p></td>
</tr>
</tbody>
</table>

  

## Event Simulator APIs

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>API Context</th>
<th>Required Permission</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>POST</td>
<td>/simulation/single</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>POST</td>
<td>/simulation/feed</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>GET</td>
<td>/simulation/feed</td>
<td><p>PermissionString - simulator.manage or simulator.view</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>PUT</td>
<td>/simulation/feed/{simulationName}</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>GET</td>
<td>/simulation/feed/{simulationName}</td>
<td><p>PermissionString - simulator.manage or simulator.view</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>DELETE</td>
<td>/simulation/feed/{simulationName}</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/simulation/feed/{simulationName}?action=run</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>POST</td>
<td>/simulation/feed/{simulationName}?action=pause</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/simulation/feed/{simulationName}?action=resume</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>POST</td>
<td>/simulation/feed/{simulationName}?action=stop</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/simulation/feed/{simulationName}?action=resume</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>GET</td>
<td>/simulation/feed/{simulationName}/status</td>
<td><p>PermissionString - simulator.manage or simulator.view</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/simulation/files</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>GET</td>
<td>/simulation/files</td>
<td><p>PermissionString - simulator.manage or simulator.view</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>PUT</td>
<td>/simulation/files/{fileName}</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>DELETE</td>
<td>/simulation/files/{fileName}</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/simulation/ connectToDatabase</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="even">
<td>POST</td>
<td>/simulation/ connectToDatabase / retrieveTableNames</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
<tr class="odd">
<td>POST</td>
<td>/simulation/ connectToDatabase/{tableName}/ retrieveColumnNames</td>
<td><p>PermissionString - simulator.manage</p>
<p>AppName - SIM</p></td>
</tr>
</tbody>
</table>

  

```
    simulator
```
