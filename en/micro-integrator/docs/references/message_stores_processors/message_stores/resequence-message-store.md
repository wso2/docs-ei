# Resequence Message Store

The Resequence Message Store is used to store a stream of related but
out-of-sequence messages to put them back into the correct order. It
collects and re-orders the stored messages based on a defined sequence
number derived from some part of the message, so that they can be
published to the output channel in a specific order. This is
advantageous specially when the order of message delivery is important
to avoid some messages arriving earlier than others.

The resequencing store is an extension of the existing JDBC-based
message store. Hence, it inherits most of its properties from the [JDBC
Message Store](_JDBC_Message_Store_) .  

### UI Configuration

In the WSO2 ESB Management Console, click **Main** , click **Message
Stores** in the **Service Bus** menu, and then click **Resequence
Message Store** , to configure it as follows.

![add resequence message store](attachments/119131501/119131503.png)

Enter the configuration details in the below screen.

![enter details of the Resequence Message
Store](attachments/119131501/119131502.png)

When you add a Resequence Message Store, it is required to specify
values for the following:

-   **Name** : A unique name for the RabbitMQ message store.
-   **Database Table** : Table name of the database in which, messages
    will be stored.
-   **Connection Information** : If the database is connected via a
    Carbon datasource or via a connection pool.
-   **Driver** : The class name of the database driver.
-   **Url** : URL of the database.
-   **User** : The username to access the database.
-   **Password** : The password to access the database.
-   **Resequence Timeout (Seconds)** : The time the Message Processor
    waits for a message, which is missing to reorder them based on a
    specified order.
-   **Sequence ID Path** : The path from which, the Store identifies the
    sequence ID from the message content.

The following is a sample ESB configuration of the Resequence Message
Store.

``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <messageStore xmlns="http://ws.apache.org/ns/synapse"
                  class="org.apache.synapse.message.store.impl.resequencer.ResequenceMessageStore"
                  name="RStore">
       <parameter name="store.resequence.timeout">-1</parameter>
       <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
       <parameter name="store.failover.message.store.name">RStore</parameter>
       <parameter xmlns:m0="http://services.samples"
                  name="store.resequence.id.path"
                  expression="substring-after(//m0:placeOrder/m0:order/m0:symbol,'-')"/>
       <parameter name="store.jdbc.password">root</parameter>
       <parameter name="store.jdbc.driver">com.mysql.jdbc.Driver</parameter>
       <parameter name="store.jdbc.username">root</parameter>
       <parameter name="store.jdbc.connection.url">jdbc:mysql://localhost:3306/resequenceDB</parameter>
       <parameter name="store.jdbc.table">tbl_resequence</parameter>
    </messageStore>
```

The following are the properties related to the Resequence Message
Store.

<table>
<thead>
<tr class="header">
<th>Property Name</th>
<th>Description</th>
<th>Required</th>
<th>Possible Values</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             store.resequence.timeout            </code></td>
<td><p>The time the Processor waits for a message, which is missing to reorder them based on a specified order. If the current message sequence is 3, its predecessor would be 2 and the successor would be 4, and thereby, if 4 is not present, then there is a gap in the sequence. Therefore, the processor waits until the specified set time-out exceeds, and selects the next minimum sequence ID available as the predecessor. However, the time-out will be a rough estimate. The durations could vary depending on the load of the machine.</p>
<p><br />
</p>
<br />

<p><br />
</p></td>
<td>Yes</td>
<td><p>Specify a positive integer for the count and the timeout in seconds.</p>
<p>If you specify the count as -1, then the processor will wait indefinitely until the correct sequence ID is present to fill the gap.</p></td>
</tr>
<tr class="even">
<td><pre><code>store.producer.guaranteed.delivery.enable</code></pre></td>
<td>Whether you want to enable guaranteed delivery or not. For more information, see <a href="_Guaranteed_Delivery_with_Failover_Message_Store_and_Scheduled_Failover_Message_Forwarding_Processor_">Guaranteed Delivery with Failover Message Store and Scheduled Failover Message Forwarding Processor</a> .</td>
<td>No</td>
<td>True/False</td>
</tr>
<tr class="odd">
<td><pre><code>store.failover.message.store.name</code></pre></td>
<td>The name of the Message Store used if the original message store fails. For more information, see <a href="_Guaranteed_Delivery_with_Failover_Message_Store_and_Scheduled_Failover_Message_Forwarding_Processor_">Guaranteed Delivery with Failover Message Store and Scheduled Failover Message Forwarding Processor</a> .</td>
<td>No</td>
<td>An appropriate String value</td>
</tr>
<tr class="even">
<td><code>             store.resequence.id.path            </code></td>
<td><p>The path from which, the Store identifies the sequence ID from the message content. The path could be either XPath or JSON. You can specify it in the expression field.</p>
<br />

<p><br />
</p></td>
<td>Yes</td>
<td>Set a positive integer as the sequence ID. The store expects the sequence ID to start from 1.</td>
</tr>
<tr class="odd">
<td><code>             store.jdbc.password            </code></td>
<td>The password to access the database.</td>
<td>No</td>
<td>An appropriate String value</td>
</tr>
<tr class="even">
<td><code>             store.jdbc.driver            </code></td>
<td>The class name of the database driver.</td>
<td>Yes</td>
<td>An appropriate String value</td>
</tr>
<tr class="odd">
<td><code>             store.jdbc.username            </code></td>
<td>The username to access the database.</td>
<td>Yes</td>
<td>An appropriate String value</td>
</tr>
<tr class="even">
<td><code>             store.jdbc.connection.url            </code></td>
<td>The database URL.</td>
<td>Yes</td>
<td>An appropriate String value of a URL.</td>
</tr>
<tr class="odd">
<td><code>             store.jdbc.table            </code></td>
<td>Table name of the database in which, messages will be stored.</td>
<td>Yes</td>
<td>An appropriate String value</td>
</tr>
</tbody>
</table>

!!! info

For more information on its usage, go to [Resequencer Pattern in the EIP
Guide](https://docs.wso2.com/display/IntegrationPatterns/Resequencer) .

