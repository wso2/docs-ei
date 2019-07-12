# Load-balance Group

The **Load-balanced Group** distributes the messages (load) arriving at
it among a set of listed [endpoints](_Working_with_Endpoints_) or static
members by evaluating the load balancing policy and other relevant
parameters.

------------------------------------------------------------------------

[XML Configuration](#Load-balanceGroup-XMLConfiguration) \|
[Parameters](#Load-balanceGroup-Parameters)

------------------------------------------------------------------------

### XML Configuration

The syntax of the load balance endpoint is as follows.

``` html/xml
    <session type="http|simpleClientSession"/>?
    <loadBalance [policy="roundRobin"] [algorithm="impl of org.apache.synapse.endpoints.algorithms.LoadbalanceAlgorithm"]
            [failover="true|false"]>
        <endpoint .../>+
        <member hostName="host" [httpPort="port"] [httpsPort="port2"]>+
    </loadBalance>
```

The Load-balance attributes and elements:

-   The `          policy         ` attribute of the load balance
    element specifies the load balance policy (algorithm) to be used for
    selecting the target endpoint or static member.

!!! info

Tip

Currently only the `         roundRobin        ` policy is supported.


-   The `          failover         ` attribute determines if the next
    endpoint or static member should be selected once the currently
    selected endpoint or static member has failed, and defaults to true.

<!-- -->

-   The `          loadBalance         ` element allows to list the set
    of endpoints or static members among which the load has to be
    distributed. These endpoints can belong to any endpoint type (see
    [Address Endpoint](_Address_Endpoint_) , [WSDL
    Endpoint](_WSDL_Endpoint_) , [Default Endpoint](_Default_Endpoint_)
    , [Configuring Failover Endpoints](_Configuring_Failover_Endpoints_)
    ). For example, Failover Endpoints can be listed inside the
    Load-balance endpoint to load balance between failover groups etc.

!!! info

Tip

The `         loadBalance        ` element cannot have both endpoint and
member child elements in the same configuration. In the case of the
member child element, the `         hostName        ` ,
`         httpPort        ` and/or `         httpsPort        `
attributes could be specified.


-   The optional `          session         ` element makes the endpoint
    a session affinity based load balancing endpoint. If it is
    specified, sessions are bound to endpoints in the first message and
    all successive messages for those sessions are directed to their
    associated endpoints.

!!! info

Tip

Currently there are two types of sessions supported in SAL endpoints.
Namely HTTP transport based session which identifies the sessions based
on HTTP cookies and the client session which identifies the session by
looking at a SOAP header sent by the client with the
`         QName "[http://ws.apache.org/ns/synapse]ClientID"        ` .


-   The `          failover         ` attribute mentioned above is not
    applicable for session affinity based endpoints and it is always
    considered as set to false. If it is required to have failover
    behavior in session affinity based load balance endpoints, list
    failover endpoints as the target endpoints.

    ------------------------------------------------------------------------

### Parameters

The parameters available to configure a load-balance endpoint are as
follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Endpoint Name</strong></td>
<td>This parameter is used to enter a unique name for the endpoint.</td>
</tr>
<tr class="even">
<td><strong>Algorithm</strong></td>
<td>The algorithm on which the load balancing is based. <strong>Round</strong> <strong>Robin</strong> is the default algorithm. You can also load a custom algorithm. Instructions for creating a custom algorithm are included in <a href="http://supunk.blogspot.com/2010/02/writing-load-balance-algorithm-for-wso2.html">this article</a> .</td>
</tr>
<tr class="odd">
<td><strong>Session Management</strong></td>
<td><p>A session management method from the load balancing group. The possible values are as follows.</p>
<ul>
<li><strong>None</strong> - If this is selected, session management is not used.</li>
<li><strong>Transport</strong> - If this is selected, session management is done on the transport level using HTTP cookies.</li>
<li><strong>SOAP</strong> - If this is selected, session management is done using SOAP sessions.</li>
<li><strong>Client ID</strong> - If this is selected, session management is done using an ID sent by the client.</li>
</ul></td>
</tr>
<tr class="even">
<td><strong>Session Timeout (Mills)</strong></td>
<td>The number of milliseconds after which the session would time out.</td>
</tr>
<tr class="odd">
<td><strong>Add Child</strong></td>
<td>Click this link to add a child endpoint. The required endpoint type can be selected from the list and a section displaying parameters relevant to the selected endpoint type will appear. You can add as many endpoints as required to the load-balance group. See the details of child endpoints in <a href="_Address_Endpoint_">Address Endpoint</a> , <a href="_WSDL_Endpoint_">WSDL Endpoint</a> , and <a href="_Configuring_Failover_Endpoints_">Configuring Failover Endpoints</a> .</td>
</tr>
<tr class="even">
<td><strong>Add Property</strong></td>
<td>Click this link to add properties to the load-balance endpoint. See <a href="https://docs.wso2.com/display/EI650/Properties+Reference">Properties Reference</a> for details of the available properties.</td>
</tr>
</tbody>
</table>
