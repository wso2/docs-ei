# Proxy Service Properties

See the topics given below for the list of properties that can be configured when [creating a proxy service](../../develop/creating-artifacts/creating-a-proxy-service.md).

## General Properties

Listed below are the main properties that are required when [creating a proxy service](../../develop/creating-artifacts/creating-a-proxy-service.md) of any type.

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
        <li>If you have a <a href="../../../develop/creating-artifacts/creating-endpoints">predefined <b>Endpoint</b></a> artifact in WSO2 Integration Studio, provide the name of the artifact.</li>
        <li>If you have a predefined <b>Endpoint</b> artifact that is saved in the <a href="../../../concepts/registry-concepts">registry</a>, provide the link to the artifact.</li>
      </ul>
      See <a href="../../../../references/synapse-properties/endpoint-properties">Endpoint Properties</a> for the complete list of properties you can define for the Endpoint artifact.
    </td>
  </tr>
</table>

## Logging Properties

The following properties are required when [creating a logging proxy service](../../develop/creating-artifacts/creating-a-proxy-service.md):

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Target Endpoint</td>
    <td>
      See the descriptions of <a href="#general-properties">general properties</a>
    </td>
  </tr>
  <tr>
    <td>Transports</td>
    <td>
      See the descriptions of <a href="#general-properties">general properties</a>
    </td>
  </tr>
  <tr>
    <td>Request Log Level</td>
    <td>
      This is the log level used for logging the request message.
      <ul>
        <li>
          <strong>Simple</strong> logs <code>               To              </code> , <code>               From              </code> , <code>               WSAction              </code> , <code>               SOAPAction              </code> , <code>               ReplyTo              </code> , <code>               MessageID              </code> , and any properties.
        </li>
        <li>
          <strong>Full</strong> logs all attributes of the message plus the SOAP envelope information.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Response Log Level</td>
    <td>
      This is the log level used for logging the response message.
      <ul>
        <li>
          <strong>Simple</strong> logs <code>               To              </code> , <code>               From              </code> , <code>               WSAction              </code> , <code>               SOAPAction              </code> , <code>               ReplyTo              </code> , <code>               MessageID              </code> , and any properties.
        </li>
        <li>
          <strong>Full</strong> logs all attributes of the message plus the SOAP envelope information.
        </li>
      </ul>
    </td>
  </tr>
</table>

## WSDL Properties

The following properties are required when [creating a WSDL-based proxy service](../../develop/creating-artifacts/creating-a-proxy-service.md):

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Transports</td>
    <td>
      See the descriptions of <a href="#general-properties">general properties</a>
    </td>
  </tr>
  <tr>
    <td>WSDL URI</td>
    <td>
      The URL and the URN of the WSDL. The URL defines the host address of the network resource (can be omitted if resources are not network homed), and the URN defines the resource name in local namespaces. For example, if the URL is <code>ftp://ftp.dlink.ru</code> and the URN is <code>/pub/ADSL/</code>, you would enter <code>ftp://ftp.dlink.ru/pub/ADSL/</code> for the URI.
    </td>
  </tr>
  <tr>
    <td>WSDL Service</td>
    <td>
      The WSDL service name.
    </td>
  </tr>
  <tr>
    <td>WSDL Port</td>
    <td>
      The port of the WSDL.
    </td>
  </tr>
  <tr>
    <td>Publish Same Service Contract</td>
    <td>
      Select this option if you want to publish this WSDL.
    </td>
  </tr>
</table>

## Transformer Proxy Properties

The following properties are required when [creating a transformer proxy service](../../develop/creating-artifacts/creating-a-proxy-service.md):

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Transports</td>
    <td>
      See the descriptions of <a href="#general-properties">general properties</a>
    </td>
  </tr>
  <tr>
    <td>Target Endpoint</td>
    <td>
      See the descriptions of <a href="#general-properties">general properties</a>
    </td>
  </tr>
  <tr>
    <td>Request XSLT</td>
    <td>Specify the location of the XSLT you want to use for transformining incoming request messages.</td>
  </tr>
  <tr>
    <td>Transform Responses</td>
    <td>Select this option if you want the Micro Integrator to transform response messages that are sent back to the client.</td>
  </tr>
  <tr>
    <td>Response XSLT</td>
    <td>
      Specify the location of the XSLT you want to use for transformining response messages.
    </td>
  </tr>
</table>

## Service Parameters

- See the list of [Transport Parameters](transport-parameters.md) you can configure at service level for a proxy service.
- You can also configure the following service-level property to expose an [Inbound Endpoint](../../concepts/message-entry-points/#inbound-endpoints) through a proxy service:
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