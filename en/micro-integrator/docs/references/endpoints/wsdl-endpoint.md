# WSDL Endpoint

The **WSDL Endpoint** is an endpoint definition based on a specified
WSDL document.

The WSDL document can be specified in 2 ways:

-   As a URI.
-   As an inlined definition within the endpoint configuration.

------------------------------------------------------------------------

[XML Configuration](#WSDLEndpoint-XMLConfiguration) \|
[Parameters](#WSDLEndpoint-Parameters)

------------------------------------------------------------------------

### XML Configuration

The syntax of the endpoint is as follows.

``` html/xml
    <wsdl [uri="wsdl-uri"] service="qname" port/endpoint="qname">
        <wsdl:definition>...</wsdl:definition>?
        <wsdl20:description>...</wsdl20:description>?
        <enableRM [policy="key"]/>?
        <enableSec [policy="key"]/>?
        <enableAddressing/>?
    
        <timeout>
            <duration>timeout duration in milliseconds</duration>
            <action>discard|fault</action>
        </timeout>?
    
        <markForSuspension>
            [<errorCodes>xxx,yyy</errorCodes>]
            <retriesBeforeSuspension>m</retriesBeforeSuspension>
            <retryDelay>d</retryDelay>
        </markForSuspension>
    
        <suspendOnFailure>
            [<errorCodes>xxx,yyy</errorCodes>]
            <initialDuration>n</initialDuration>
            <progressionFactor>r</progressionFactor>
            <maximumDuration>l</maximumDuration>
        </suspendOnFailure>
    </wsdl>
```

The `         service        ` and `         port        ` name
containing the target EPR has to be specified with the
`         service        ` and `         port        ` (or
`         endpoint        ` ) attributes respectively.

`         enableRM        ` , `         enableSec        ` ,
`         enableAddressing        ` ,
`         suspendOnFailure        ` and `         timeout        `
elements are same as for an [Address Endpoint](_Address_Endpoint_) .

------------------------------------------------------------------------

### Parameters

Parameters available to configure a WSDL endpoint are as follows.

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 82%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Name</strong></td>
<td>This parameter is used to enter a unique name for the endpoint.</td>
</tr>
<tr class="even">
<td><strong>WSDL URI</strong></td>
<td>The URI of the WSDL. Click <strong>Test</strong> to test the URI.</td>
</tr>
<tr class="odd">
<td><strong>Service</strong></td>
<td>The service selected from the available services for the WSDL.</td>
</tr>
<tr class="even">
<td><strong>Port</strong></td>
<td>The port selected for the service specified in the <strong>Service</strong> parameter. In a WSDL, an endpoint is bound to each port inside each service.</td>
</tr>
<tr class="odd">
<td><strong>Show Advanced Options</strong></td>
<td>Click this link if you want to add advanced options for the endpoint.</td>
</tr>
<tr class="even">
<td><strong>Add Property</strong></td>
<td>Click this link if you want to add properties to the endpoint. See <a href="https://docs.wso2.com/display/EI650/Properties+Reference">Properties Reference</a> for details of the available properties.</td>
</tr>
</tbody>
</table>
