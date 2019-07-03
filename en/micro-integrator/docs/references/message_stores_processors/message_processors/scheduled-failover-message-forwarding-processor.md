# Scheduled Failover Message Forwarding Processor

The scheduled failover message forwarding processor is a [message
processor](_Message_Processors_) that ensures reliable message delivery.
This message processor is useful when it comes to scenarios where a
message store failure takes place and it is necessary to ensure
guaranteed message delivery.

The only difference of the scheduled failover message forwarding
processor from the scheduled message forwarding processor is that the
scheduled message forwarding processor forwards messages to a defined
endpoint, whereas the scheduled failover message forwarding processor
forwards messages to a target message store.

For an example scenario where the scheduled failover message forwarding
processor is used, see [Guaranteed Delivery with Failover Message Store
and Scheduled Failover Message Forwarding
Processor](_Guaranteed_Delivery_with_Failover_Message_Store_and_Scheduled_Failover_Message_Forwarding_Processor_)
.

### Scheduled failover message forwarding processor parameters

Following are the parameters and the description for each of the
parameters you need to specify when creating a scheduled failover
message forwarding processor.

<table>
<thead>
<tr class="header">
<th><p>Parameter Name</p></th>
<th><p>Description</p></th>
<th><p>Required</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Name</td>
<td>The name of the scheduled failover message forwarding processor.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>Source Message Store</td>
<td>The message store from which the scheduled failover message forwarding processor consumes messages .</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Target Message Store</td>
<td>The message store to which the scheduled failover message forwarding processor forwards messages.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>Processor state ( <code>             is.active            </code> )</td>
<td>Activate ( <code>             true            </code> ) or Deactivate ( <code>             false            </code> )</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Forwarding interval ( <code>             interval            </code> )</td>
<td><p>Interval in milliseconds in which processor consumes messages.</p></td>
<td>No (The default value is 1000)</td>
</tr>
<tr class="even">
<td><p>Retry interval ( <code>              client.retry.interval             </code> )</p></td>
<td><p>Message retry interval in milliseconds.</p></td>
<td><p>No (The default is value is 1000)</p></td>
</tr>
<tr class="odd">
<td><p>Maximum delivery attempts ( <code>              max.delivery.attempts             </code> )</p></td>
<td><p>Maximum redelivery attempts before deactivating the processor. This is used when the backend server is inactive and the ESB profile tries to resend the message. If you set the value of this property to -1, it deactivates the message processor without retrying, after the first attempt fails.</p></td>
<td><p>No (The default value is <code>              4)             </code></p></td>
</tr>
<tr class="even">
<td>Drop message after maximum delivery attempts ( <code>             max.delivery.drop            </code> )</td>
<td><p>If this parameter is set to <code>              Enabled             </code> , the message will be dropped from the message store after the maximum number of delivery attempts are made, and the message processor will remain activated. This parameter would have no effect when no value is specified for the <strong>Maximum Delivery Attempts</strong> parameter.</p>
<p>The <strong>Maximum Delivery Attempts</strong> parameter can be used when the backend is inactive and the message is resent.<br />
<br />
If this parameter is disabled, the undeliverable message will not be dropped and the message processor will be deactivated.</p></td>
<td>No (The default value is <code>             Disabled)            </code></td>
</tr>
<tr class="odd">
<td><p>Fault sequence name ( <code>              message.processor.fault.sequence             </code> )</p></td>
<td><p>The name of the sequence where the fault message should be sent to in case of a SOAP fault.</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Deactivate sequence name ( <code>              message.processor.deactivate.sequence             </code> )</p></td>
<td><p>The deactivate sequence that will be executed when the processor is deactivated automatically. Automatic deactivation occurs when the maximum delivery attempts is exceeded and the Drop message after maximum delivery attempts parameter is disabled.</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>Quartz configuration file path ( <code>              quartz.conf             </code> )</p></td>
<td><p>The Quartz configuration file path. This properties file contains the Quartz configuration<br />
parameters for fine tuning the Quartz engine. More details of the configuration can be<br />
found at <a href="http://quartz-scheduler.org/documentation/quartz-2.x/configuration/ConfigMain">http://quartz-scheduler.org/documentation/quartz-2.x/configuration/ConfigMain</a> .</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Cron Expression ( <code>              cronExpression             </code> )</p></td>
<td><p>The cron expression to be used to configure the retry pattern.</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td>Task Count (Cluster Mode)
<p><br />
</p></td>
<td>The required number of worker nodes when you need to run the processor in more than 1 worker node. Specifying this will not guarantee that the processor will run on each worker node. There can be instances where the processor will not run in some workers nodes.</td>
<td>No (The default value is 1)</td>
</tr>
</tbody>
</table>

In addition to specifying the required parameters, you can click **Show
Additional Parameters** and set any of the additional parameters if
necessary.
