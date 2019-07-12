# WSO2 MB Message Store

WSO2 Enterprise Integrator (WSO2 EI) is shipped with a separate message
broker profile (WSO2 MB). You can easily set up this WSO2 MB profile as
the [message store](_Message_Stores_) for the ESB. Explained below are
the parameters you need to configure for a WSO2 MB Message Store. Also,
find details of other configurations that are relevant to the WSO2 MB
Message Store.

See the following topics for details:

-   [WSO2 MB Message Store
    parameters](#WSO2MBMessageStore-WSO2MBMessageStoreparameters)
-   [Other configurations](#WSO2MBMessageStore-Otherconfigurations)
-   [Related topics](#WSO2MBMessageStore-Relatedtopics)

### WSO2 MB Message Store parameters

!!! tip

Use [this link](_Creating_a_Message_Store_) for instructions on how to
create a message store artifact for the ESB. Be sure to set the message
store type as **WSO2 MB Message Store** , and configure the parameters
as explained below.


Listed below are the **required** parameters for configuring the WSO2 MB
Message Store.

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Message Store Name</td>
<td>Give a unique name for the JMS message store.</td>
</tr>
<tr class="even">
<td>Message Store Type</td>
<td>Select <strong>WSO2 MB Message Store</strong> from the list of options.</td>
</tr>
<tr class="odd">
<td>Initial Context Factory</td>
<td>This specifies the JNDI initial context factory class ( <code>             java.naming.factory.initial            </code> ). This class implements the implement the <code>             java.naming.spi.InitialContextFactory            </code> interface. The value is set to <code>             org.wso2.andes.jndi.PropertiesFileInitialContextFactory            </code> , by default.</td>
</tr>
<tr class="even">
<td>Queue Connection Factory</td>
<td><div class="content-wrapper">
<p>This is the connection factory URlfor connecting to WSO2 MB. By default, the value is set to <code>               amqp://admin:admin@clientID/carbon?brokerlist='tcp://localhost:5673'              </code> .</p>
!!! note
<p>Important!</p>
<p>Be sure to change the port to 5675.</p>

</div></td>
</tr>
<tr class="odd">
<td>JNDI Queue Name</td>
<td>The name of the queue in the broker that will store messages.</td>
</tr>
<tr class="even">
<td>JMS API Specification Version</td>
<td>The JMS API version to be used. Possible values are 1.1 or 1.0. The value is set to 1.1, by default.</td>
</tr>
</tbody>
</table>

If you need to ensure guaranteed delivery of your messages, specify
values for the following parameters:

| Parameter Name                      | Description                                                                                                                                         |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Enable Producer Guaranteed Delivery | This flag specifies whether guaranteed delivery is enabled on the producer side. The value is set to `             False            ` , by default. |
| Failover Message Store              | The message store to which the store mediator should send messages when the original message store fails.                                           |

### Other configurations

When you use the Message Broker profile as the message broker and
configure a [Message Processor](_Message_Processors_) with
`         max.delivery.attempts        ` , if the Message Broker profile
does not get an acknowledgment from the Integration profile, it re-sends
the message that results in duplicate messages being delivered. To avoid
this, add the following line in the
`         <EI_HOME>/bin/integrator.sh        ` file:

``` java
    -DAndesAckWaitTimeOut=3600000 \
```

### Related topics

-   See the tutorial on [Storing and Forwarding
    Messages](https://docs.wso2.com/display/EI650/Storing+and+Forwarding+Messages)
    .
-   Read more about this use case: [Store and Forward Using JMS Message
    Stores](https://docs.wso2.com/display/EI650/Store+and+Forward+Using+JMS+Message+Stores)
    .
