# Store Mediator

The **Store mediator** enqueues messages passing through its mediation
sequence in a given [message
store](https://docs.wso2.com/display/EI650/Message+Stores) . It can
serve as a [dead letter
channel](https://docs.wso2.com/display/IntegrationPatterns/Dead+Letter+Channel)
if it is included in a fault sequence and if its message store is
connected to a [message
processor](https://docs.wso2.com/display/EI650/Message+Processors) that
forwards all the messages in the store to an endpoint.

------------------------------------------------------------------------

### Syntax

!!! info

The Store mediator is a [content
aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


``` xml
    <axis2ns1:store xmlns:axis2ns1="http://ws.apache.org/ns/synapse" messageStore="JMSMS" sequence="storeSequence"></axis2ns1:store>
```

You can dynamically route messages to a Message Store via the Store
mediator by resolving the name of the Message Store from the message
context. To enable this, give a path expression (followed by its
namespace definitions) for the value of the store name attribute as
shown below.

``` xml
    <axis2ns1:store xmlns:axis2ns1="http://ws.apache.org/ns/synapse" messagestore="{//m:msgstr/m:arg/m:value}"
    xmlns:m="http://services.samples/xsd" sequence="storeSequence"></axis2ns1:store>
```

### Configuration

The parameters available to configure the Store mediator is as follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Message Store</strong></td>
<td><div class="content-wrapper">
<p>The Message Store, in which messages will be stored. You can give the name of the Message Store either as a <strong>value</strong> or as an <strong><strong>expression</strong> .</strong></p>
!!! tip
<p>You should add the message store to the ESB profile before you can select it here.</p>

<ul>
<li>To give the Message Store name as a value, select <strong>Value</strong> for <strong>Specify As</strong> , and selct the name of the Message Store from the drop down of <strong>Value</strong> .</li>
<li><p>To give the Message Store name as an expression, select <strong>Expression</strong> for <strong>Specify As</strong> , and e nter the XPath to derive the Message Store from the message context.</p>
<p>In the namespace editor, add the namespaces that are used in the XPath.</p></li>
</ul>
</div></td>
</tr>
<tr class="even">
<td><strong>On Store Sequence</strong></td>
<td>The <a href="_Mediation_Sequences_">sequence</a> that will be called before the message gets stored. This <a href="_Mediation_Sequences_">sequence</a> should be pre-defined in the registry before it can be entered here. Click either <strong>Configuration Registry</strong> or <strong>Governance</strong> <strong>Registry</strong> to select the required sequence from the resource tree. For more information on configuring the Registry, go to <a href="https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry">Working with the Registry</a> in the Common Admin Guide.</td>
</tr>
</tbody>
</table>

### Examples

Following are examples demonstrating the usage of the Store mediator.

#### Example 1 - Defining the Message Store as a value

A proxy service can be configured with the Store mediator as follows to
save messages in a message store named `         JMSMS        ` .

``` xml
    <proxy name="SimpleProxy" transports="http https" startonload="true" trace="disable" xmlns="http://ws.apache.org/ns/synapse">
       <target>
          <inSequence>
             <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2" type="STRING"></property>
             <property name="OUT_ONLY" value="true" scope="default" type="STRING"></property>
             <store messageStore="JMSMS"></store>
          </inSequence>
       </target>
    </proxy>
```

#### Example 2 - Defining the Message Store as an XPath expression

A proxy service can be configured with the Store mediator as follows to
save messages in a Message Store, which is dynamically set via the
message context specified using an XPath expression.

!!! tip

You can use the [SimpleStock Quote
service](https://docs.wso2.com/display/EI620/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
as the backend service and [SOAP UI](https://www.soapui.org/) as the
Client to try out the below example.


``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <registry provider="org.wso2.carbon.mediation.registry.WSO2Registry">
            <parameter name="cachableDuration">15000</parameter>
        </registry>
        <taskManager provider="org.wso2.carbon.mediation.ntask.NTaskTaskManager"/>
        <proxy name="StoreMediatorProxy" startOnLoad="true" transports="http https">
            <description/>
            <target>
                <inSequence>
                    <store
                        messageStore="{//ser:getQuote/ser:request/ser:symbol}" xmlns:ser="http://services.samples"/>
                </inSequence>
            </target>
        </proxy>
        <endpoint name="StoreMediatorEndpoint">
            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
        </endpoint>
        <sequence name="fault">
            <!-- Log the message at the full log level with the ERROR_MESSAGE and the ERROR_CODE-->
            <log level="full">
                <property name="MESSAGE" value="Executing default 'fault' sequence"/>
                <property expression="get-property('ERROR_CODE')" name="ERROR_CODE"/>
                <property expression="get-property('ERROR_MESSAGE')" name="ERROR_MESSAGE"/>
            </log>
            <!-- Drops the messages by default if there is a fault -->
            <drop/>
        </sequence>
        <sequence name="main">
            <in>
                <!-- Log all messages passing through -->
                <log level="full"/>
                <!-- ensure that the default configuration only sends if it is one of samples -->
                <!-- Otherwise Synapse would be an open proxy by default (BAD!)               -->
                <filter regex="http://localhost:9000.*" source="get-property('To')">
                    <!-- Send the messages where they have been sent (i.e. implicit "To" EPR) -->
                    <send/>
                </filter>
            </in>
            <out>
                <send/>
            </out>
            <description>The main sequence for the message mediation</description>
        </sequence>
        <messageStore name="StoreMediatorStore"/>
        <messageProcessor
            class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor"
            messageStore="StoreMediatorStore" name="StoreMediatorProcessor" targetEndpoint="StoreMediatorEndpoint">
            <parameter name="client.retry.interval">1000</parameter>
            <parameter name="throttle">false</parameter>
            <parameter name="max.delivery.attempts">4</parameter>
            <parameter name="member.count">1</parameter>
            <parameter name="max.delivery.drop">Disabled</parameter>
            <parameter name="interval">1000</parameter>
            <parameter name="is.active">true</parameter>
            <parameter name="target.endpoint">StoreMediatorEndpoint</parameter>
        </messageProcessor>
        <!-- You can add any flat sequences, endpoints, etc.. to this synapse.xml file if you do
        *not* want to keep the artifacts in several files -->
    </definitions>
```
