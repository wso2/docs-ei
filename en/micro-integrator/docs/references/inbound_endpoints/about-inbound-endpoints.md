# Working with Inbound Endpoints

An inbound endpoint is a message entry point that can inject messages
directly from the transport layer to the mediation layer, without going
through the Axis2 engine. The following diagram illustrates the inbound
endpoint architecture.

![](attachments/119130469/119130470.png)

Out of the existing transports only the HTTP transport supports
multi-tenancy, this is one limitation that is overcome with the
introduction of the inbound architecture. Another limitation when it
comes to conventional Axis2 based transports is that the transports do
not support dynamic configurations. With the ESB profile inbound
endpoints, it is possible to create inbound messaging channels
dynamically, and there is also built-in cluster coordination as well as
multi-tenancy support for all transports.

For detailed information on each type of inbound endpoint available with
the ESB profile, see [WSO2 EI Inbound
Endpoints](_WSO2_EI_Inbound_Endpoints_) .

## Working with Inbound Endpoints

When it comes to creating and managing inbound endpoints, you can use
tooling to create a new inbound endpoint and to import an existing
inbound endpoint, or you can manage inbound endpoints via the WSO2 EI
Management Console. For detailed information on how to work with inbound
endpoints, see [Creating an Inbound
Endpoint](_Creating_an_Inbound_Endpoint_) .

## WSO2 EI Inbound Endpoints

Based on the protocol, the behaviour of an inbound endpoint can either
be listening, polling or event-based.

For detailed information on listening, polling and event-based inbound
endpoints see the following topics:

-   [Listening Inbound Endpoints](_Listening_Inbound_Endpoints_)
-   [Polling Inbound Endpoints](_Polling_Inbound_Endpoints_)
-   [Event-Based Inbound Endpoints](_Event-Based_Inbound_Endpoints_)

For information on how to create a custom inbound endpoint based on your
requirement, see [Custom Inbound Endpoint](_Custom_Inbound_Endpoint_) .

Following is a sample inbound endpoint configuration:

``` xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                    name="HttpListenerEP"
                    sequence="TestIn"
                    onError="fault"
                    protocol="http"
                    suspend="false">
      <parameters>
         <parameter name="inbound.http.port">8085</parameter>
      </parameters>
    </inboundEndpoint>
```

In an inbound endpoint configuration, the common inbound endpoint
parameters are specified as attributes of the
`         <inboundEndpoint>        ` element whereas the protocol
specific parameters are specified as `         <parameter>        `
elements.

### Common inbound endpoint parameters

The following parameters are common to all inbound endpoints:

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>sequential</td>
<td>The behavior when executing the given sequence.<br />
When set as <code>             true            </code> , mediation will happen within the same thread. When set as <code>             false            </code> , the mediation engine will use the inbound thread pool. (The default thread pool values can be found in the <code>             &lt;EI_HOME&gt;/conf/synapse.properties            </code> file).</td>
<td>Yes</td>
<td>true or false</td>
<td>true</td>
</tr>
<tr class="even">
<td>suspend</td>
<td>When set to <code>             true            </code> , this makes the inbound endpoint inactive.</td>
<td>Yes</td>
<td>true or false</td>
<td>false</td>
</tr>
</tbody>
</table>

### Specifying inbound endpoint parameters as registry values

Other than specifying parameter values inline, you can also
specify parameter values as registry entries. The advantage of
specifying a parameter value as a registry entry is that the same
inbound endpoint configuration can be used in different environments
simply by changing the registry entry value.

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="file" sequence="request" onError="fault" protocol="file" suspend="false">
       <parameters>
          ...............
          <parameter name="transport.vfs.FileURI" key="conf:/repository/ei/ei-configurations/test"/>
          ...............
       </parameters>
    </inboundEndpoint>
```

If you need to provide the registry entry value via the Management
Console, specify it as
`         $registry:conf:/repository/ei/ei-configurations/test        `
.

### Using secure vault aliases in your inbound endpoint configuration

To use secure vault support in your inbound configuration, you can add
`         {wso2:vault-lookup('xx')}        ` as inbound parameters,
where `         xx        ` is the alias.

You can encrypt and store the password using the alias
`         my.password        ` , and retrieve this password  by adding
the following parameter in your inbound endpoint configuration:

`         <parameter name="transport.jms.Password">{wso2:vault-lookup('my.password')}</parameter>        `

## Listening Inbound Endpoints

A listening inbound endpoint listens on a given port for requests that
are coming in. When a request is available it is injected to a given
sequence. Listening inbound endpoints support two way operations and are
synchronous.

See the following topics for detailed information on each listening
inbound endpoint available with the ESB profile of WSO2 Enterprise
Integrator:

-   [HTTP Inbound Protocol](_HTTP_Inbound_Protocol_)
-   [HTTPS Inbound Protocol](_HTTPS_Inbound_Protocol_)
-   [HL7 Inbound Protocol](_HL7_Inbound_Protocol_)
-   [CXF WS-RM Inbound Protocol](_CXF_WS-RM_Inbound_Protocol_)
-   [WebSocket Inbound Protocol](_WebSocket_Inbound_Protocol_)
-   [Secure WebSocket Inbound
    Protocol](_Secure_WebSocket_Inbound_Protocol_)

For information on how to create a custom inbound endpoint by extending
the listening inbound endpoint behaviour, see [Creating a custom
listening inbound
endpoint](Custom-Inbound-Endpoint_119130497.html#CustomInboundEndpoint-CustomListening)
.

## Polling Inbound Endpoints

A polling inbound endpoint polls periodically for data and when data is
available the data is injected to a given sequence. For example,  the
JMS inbound endpoint checks the JMS queue periodically for messages and
when a message is available that message is injected to a specified
sequence. Polling inbound endpoints support one way operations and are
asynchronous.

### Common polling inbound endpoint parameters

The following parameters are common to all polling inbound endpoints:

| Parameter Name | Description                                                                                                                                                                                                                                                                                                                                                       | Required | Possible Values    | Default Value |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|--------------------|---------------|
| interval       | The polling interval for the inbound endpoint to execute each cycle. This value is set in milliseconds.                                                                                                                                                                                                                                                           | No       | A positive integer | \-            |
| coordination   | This parameter only applicable in a cluster environment. In a cluster environment an inbound endpoint will only be executed in worker nodes. If set to `             true            ` in a cluster setup, this will run the inbound only in a single worker node. Once the running worker is down the inbound starts on another available worker in the cluster. | No       | true or false      | true          |

See the following topics for detailed information on each polling
inbound endpoint available with the ESB profile of WSO2 EI:

-   [File Inbound Protocol](_File_Inbound_Protocol_)
-   [JMS Inbound Protocol](_JMS_Inbound_Protocol_)
-   [Kafka Inbound Protocol](_Kafka_Inbound_Protocol_)

For information on how to create a custom inbound endpoint by extending
the polling inbound endpoint behaviour, see [Creating a custom polling
inbound
endpoint](Custom-Inbound-Endpoint_119130497.html#CustomInboundEndpoint-CustomPolling)
.

## Event-Based Inbound Endpoints

An event-based inbound endpoint polls only once to establish a
connection with the remote server and then consumes events.

### Common event-based endpoint parameters

The following parameter is common to all event-based inbound endpoints:

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
<th><p>Possible Values</p></th>
<th><p>Default Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>coordination</td>
<td>This parameter is only applicable in a cluster environment.<br />
In a cluster environment an inbound endpoint will only be executed in worker nodes. If this parameter is set to <code>             true            </code> in a cluster environment, the inbound will only be executed in a single worker node. Once the running worker node is down the inbound will start on another available worker node in the cluster.</td>
<td>No</td>
<td>true or false</td>
<td>true</td>
</tr>
</tbody>
</table>

See the following topics for detailed information on each
event-based inbound endpoint available with the ESB profile of WSO2 EI:

-   [MQTT Inbound Protocol](_MQTT_Inbound_Protocol_)
-   [RabbitMQ Inbound Protocol](_RabbitMQ_Inbound_Protocol_)

For information on how to create a custom inbound endpoint by extending
the event-based inbound endpoint behaviour, see [Creating a custom
event-based inbound
endpoint](Custom-Inbound-Endpoint_119130497.html#CustomInboundEndpoint-CustomEventBased)
.
