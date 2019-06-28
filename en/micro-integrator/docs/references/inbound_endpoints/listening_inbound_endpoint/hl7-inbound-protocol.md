# HL7 Inbound Protocol

The ESB profile of WSO2 Enterprise Integrator (WSO2 EI) HL7 inbound
protocol is a multi-tenant capable alternative to the HL7 transport. The
HL7 inbound endpoint implementation is fully asynchronous and is based
on the **Minimal Lower Layer Protocol(MLLP)** implemented on top of
event driven I/O.

Following is a sample HL7 inbound endpoint configuration:

**Sample HL7 Inbound Endpoint**

``` xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                    name="InboundHL7"
                    sequence="main"
                    onError="fault"
                    protocol="hl7"
                    suspend="false">
      <parameters>
         <parameter name="inbound.hl7.Port">20000</parameter>
         <parameter name="inbound.hl7.AutoAck">true</parameter>
         <parameter name="inbound.hl7.ValidateMessage">true</parameter>
         <parameter name="inbound.hl7.TimeOut">10000</parameter>
         <parameter name="inbound.hl7.CharSet">UTF-8</parameter>
         <parameter name="inbound.hl7.BuildInvalidMessages">false</parameter>
         <parameter name="inbound.hl7.PassThroughInvalidMessages">false</parameter>  
      </parameters>
    </inboundEndpoint>
```

In the above inbound endpoint configuration, you will see that the
endpoint is defined using the HL7 protocol and that all requests coming
into this endpoint are directed to the `         main        ` sequence.

### HL7 inbound endpoint parameters

The following table provides information on the HL7 inbound
endpoint parameters you can set:

<table>
<thead>
<tr class="header">
<th><strong>Parameter</strong></th>
<th><strong>Description</strong></th>
<th><strong>Default Value</strong></th>
<th><strong>Possible Values</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>inbound.hl7.Port</td>
<td>The port on which you need to run the MLLP listener.</td>
<td>N/A</td>
<td>You need to specify this.</td>
</tr>
<tr class="even">
<td>inbound.hl7.AutoAck</td>
<td>Whether or not an auto acknowledgement should be sent on message receipt. If set to false, you can define the type of HL7 acknowledgement to be sent . For more information, see <a href="#HL7InboundProtocol-MediationProperties">HL7 inbound endpoint mediation level properties</a> .</td>
<td>true</td>
<td>true | false</td>
</tr>
<tr class="odd">
<td>inbound.hl7.ValidateMessage</td>
<td>This enables HL7 message validation.</td>
<td>true</td>
<td>true | false</td>
</tr>
<tr class="even">
<td>inbound.hl7.TimeOut</td>
<td>The timeout interval in milliseconds to trigger a NACK message.</td>
<td>10000</td>
<td>[0..9]*</td>
</tr>
<tr class="odd">
<td>inbound.hl7.CharSet</td>
<td>The character set used for encoding and decoding messages. Some multi-byte character encodings (e.g. UTF-16, UTF-32) may result in byte values equal to the MLLP framing characters or byte values lower than 0x1F, resulting in errors.</td>
<td>UTF-8</td>
<td>ISO-8859-1<br />
UTF-8<br />
US-ASCII</td>
</tr>
<tr class="even">
<td>inbound.hl7.BuildInvalidMessages</td>
<td>If the <code>             inbound.hl7.ValidateMessage            </code> parameter is set to <code>             false            </code> and the incoming message is invalid, this parameter specifies whether the raw message received through the MLLP transport should be passed onto the mediation layer.</td>
<td>false</td>
<td>true | false</td>
</tr>
<tr class="odd">
<td>inbound.hl7.PassthroughInvalidMessages</td>
<td>If the <code>             inbound.hl7.BuildInvalidMessages            </code> parameter is set to <code>             true            </code> , this parameter notifies the Axis2 HL7 transport sender whether to use the raw message.</td>
<td>false</td>
<td>true | false</td>
</tr>
<tr class="even">
<td>inbound.hl7.MessagePreProcessor</td>
<td>An implementation of the <code>             org.wso2.carbon.inbound.endpoint.protocol.hl7.HL7MessagePreprocessor            </code> interface can be defined here. It provides an extension point to intercept incoming messages before any type of message parsing occurs.</td>
<td>N/A</td>
<td>Fully qualified class name.</td>
</tr>
</tbody>
</table>

### HL7 inbound endpoint mediation level properties

Following are the mediation level properties that you can set in the HL7
inbound endpoint:

!!! info

Note

The scope of these properties is the `         default        ` scope.


| **Property**                                                                                            | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `             <property name="HL7_RESULT_MODE" value="ACK|NACK" scope="default"/>            `          | This is use to define the type of HL7 acknowledgement to be sent. If the `             inbound.hl7.AutoAck            ` parameter is set to `             true            ` this property has no effect.                                                                                                                                                                                                                                                                                                             |
| `             <property name="HL7_NACK_MESSAGE" value="<ERROR MESSAGE>" scope="default" />            ` | This is used to define a custom error message to be sent if you have set the property `             HL7_RESULT_MODE            ` as `             NACK            ` .                                                                                                                                                                                                                                                                                                                                                |
| `             <property name="HL7_APPLICATION_ACK" value="true" scope="default"/>            `          | If the `             inbound.hl7.AutoAck            ` parameter is set to `             false            ` and no immediate auto generated ACK is sent back to the client, this property defines whether we should automatically generate the ACK for the request once the mediation flow is complete. If both the `             inbound.hl7.AutoAck            ` parameter and this property are set to `             false            ` , you need to generate an ACK message in the correct format as a response. |

### Tuning the HL7 inbound endpoint

The HL7 inbound endpoint can be configured using the
`         <EI_HOME>/conf/hl7.properties        ` file.

The supported list of tuning parameters for this file are as follows:

| Property              | Description                                                                                                                                                            | Value                                                                           |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| hl7\_id\_generator    | By default the HAPI HL7 parsing library uses a file based ID generator to generate unique control IDs. To use a UUID based ID generator you can change this to ‘uuid’. | file \| uuid (default = file)                                                   |
| worker\_threads\_core | Defines the HL7 inbound worker thread pool size.                                                                                                                       | \[0..9\]\* (default = 100)                                                      |
| io\_thread\_count     | Defines the number of IO threads the IO Reactor uses. It is recommended to set this to the number of cores on the machine.                                             | \[0..9\]\* (default = 2)                                                        |
| so\_timeout           | Defines TCP socket timeout.                                                                                                                                            | \[0..9\]\* (default = 0)                                                        |
| connect\_timeout      | Defines the TCP connect timeout.                                                                                                                                       | \[0..9\]\* (default = 0)                                                        |
| so\_keep\_alive       | Defines TCP socket keep alive.                                                                                                                                         | true \| false (default = true)                                                  |
| so\_rcvbuf            | Defines the TCP socket receive buffer size.                                                                                                                            | \[0..9\]\* (default = 0 uses OS default. Maximum value depends on OS settings). |
| so\_sndbuf            | Defines the TCP socket send buffer size.                                                                                                                               | \[0..9\]\* (default = 0 uses OS default. Maximum value depends on OS settings). |

### Samples

For a sample that demonstrates how the HL7 inbound protocol can be used
to receive a simple HL7 message, see [Sample 905: Inbound HL7 with
Automatic
Acknowledgement](https://docs.wso2.com/display/EI6xx/Sample+905%253A+Inbound+HL7+with+Automatic+Acknowledgement)
.

  
