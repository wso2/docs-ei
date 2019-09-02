# Proxy Service Properties

## General Properties

The following properties are required for a proxy service:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Proxy Servive Name</td>
    <td>A unique name for the proxy service.</td>
  </tr>
  <tr>
    <td>Transports</td>
    <td>
      The transport protocols that are used to receive messages. Once you have selected the required transports, you can later add <a href="#service-parameters">service parameters</a> relevant to each transport type.
    </td>
  </tr>
  <tr>
    <td>Target Endpoint</td>
    <td>
      The proxy service uses an <b>Endpoint</b> artifact inline to define the location to which messages should be routed. You can choose one of the following options to specify the endpoint.
      <ul>
        <li>Enter the URL of the endpoint.</li>
        <li>If you have a predefined <b>Endpoint</b> artifact in WSO2 Integration Studio, provide the name of the artifact.</li>
        <li>If you hae a predefined <b>Endpoint</b> artifact that is saved in the registry, provide the link to the artifact.</li>
      </ul>
      See <a href="endpoint-properties">Endpoint Properties</a> for the complete list of properties you can define for the Endpoint artifact.
    </td>
  </tr>
</table>

## Logging Properties

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Request Log Level</td>
    <td></td>
  </tr>
  <tr>
    <td>Response Log Level</td>
    <td></td>
  </tr>
</table>

## WSDL-Based Properties

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>WSDL URI</td>
    <td></td>
  </tr>
  <tr>
    <td>WSDL Service</td>
    <td></td>
  </tr>
  <tr>
    <td>WSDL Port</td>
    <td></td>
  </tr>
  <tr>
    <td>Publish Same Service Contract</td>
    <td></td>
  </tr>
</table>

## Transformer Proxy Properties

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Request XSLT</td>
    <td></td>
  </tr>
  <tr>
    <td>Transform Responses</td>
    <td></td>
  </tr>
  <tr>
    <td>Response XSLT</td>
    <td></td>
  </tr>
</table>

## Service Parameters

Inbound endpoint parameters for proxy services:

<table>
   <tr>
      <th>Service Parameter</th>
      <th>Description</th>
   </tr>
   <tr>
      <td>inbound.only</td>
      <td>
            Whether the proxy service needs to be exposed only via inbound endpoints. If set to <code>true</code> all requests that the proxy service receives via normal transport will be rejected. The proxy service will process only the requests that are received via inbound endpoints.</br></br> The default setting is <code>false</code>.
      </td>
   </tr>
</table>