# Scheduled Message Forwarding Processor

The scheduled message forwarding processor is a [message
processor](_Message_Processors_) that consumes messages in a message
store and sends them to an
[endpoint](https://docs.wso2.com/display/EI650/Working+with+Endpoints) .
If a message is successfully delivered to the endpoint , the processor
deletes the message from the message store. In case of a failure, it
will retry after a specified interval.

### Scheduled message forwarding processor parameters

Following are the parameters and the description for each of the
parameters you need to specify when creating a scheduled message
forwarding processor.

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
<td>The name of the scheduled message forwarding processor.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>Endpoint name</td>
<td>The endpoint to which the scheduled message forwarding processor should forward messages.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Message Store</td>
<td>The message store from which the scheduled message forwarding processor consumes messages.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td>Processor state ( <code>             is.active            </code> )</td>
<td>Activate ( <code>             true            </code> ) or Deactivate ( <code>             false            </code> )</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td>Forwarding interval ( <code>             interval            </code> )</td>
<td><p>Interval in milliseconds in which processor consumes messages.</p>
<p>If both Cron Expression and Forwarding Interval are specified in the configuration, Cron Expression will precede the Forwarding Interval.</p></td>
<td>No (The default value is 1000)</td>
</tr>
<tr class="even">
<td><p>Retry interval ( <code>              client.retry.interval             </code> )</p></td>
<td><p>Message retry interval in milliseconds.</p></td>
<td><p>No (The default is value is 1000)</p></td>
</tr>
<tr class="odd">
<td>Non retry http status codes ( <code>             non.retry.status.codes            </code> )</td>
<td><p>The parameter based on which the message processor decides if it needs to retry. If the HTTP status code of the response is specified as a n on retry http status code , it will not retry.</p></td>
<td>No</td>
</tr>
<tr class="even">
<td><p>Maximum redelivery attempts ( <code>              max.delivery.attempts             </code> )</p></td>
<td><p>Maximum redelivery attempts before deactivating the processor. This is used when the backend server is inactive and the tries to resend the message.</p></td>
<td><p>No (The default value is <code>              4)             </code></p></td>
</tr>
<tr class="odd">
<td>Drop message after maximum delivery attempts ( <code>             max.delivery.drop            </code> )</td>
<td><p>If this parameter is set to <code>              Enabled             </code> , the message will be dropped from the message store after the maximum number of delivery attempts are made, and the message processor will remain activated. This parameter would have no effect when no value is specified for the <strong>Maximum Delivery Attempts</strong> parameter.</p>
<p>The <strong>Maximum Delivery Attempts</strong> parameter can be used when the backend is inactive and the message is resent.<br />
<br />
If this parameter is disabled, the undeliverable message will not be dropped and the message processor will be deactivated.</p></td>
<td>No (The default value is <code>             Disabled)            </code></td>
</tr>
<tr class="even">
<td>Maximum store connection attempts ( <code>             max.store.connection.attempts            </code> )</td>
<td>The maximum number of times that the message processor should attempt to connect to a store in case of a broker crash or disconnect.<br />
If the message processor is unable to connect to a store after reaching the maximum number of attempts, the message processor will deactivate.</td>
<td>No (The default value is infinite, and is specified as -1)</td>
</tr>
<tr class="odd">
<td>Store connection attempt interval ( <code>             store.connection.retry.interval            </code> )</td>
<td>The interval between retry attempts with which the message processor should attempt to connect to a store in case of a broker crash or disconnect. You need to specify the interval in milliseconds.</td>
<td>No (The default value is 1000)</td>
</tr>
<tr class="even">
<td><p>Axis2 Client repository ( <code>              axis2.repo             </code> )</p></td>
<td><p>The location path of the Axis2 Client repository. This repository is used when it is needed to process messages prior to sending them to the endpoint.</p></td>
<td><p>No (The default value is <code>              /repository/deployment/client             </code> )</p></td>
</tr>
<tr class="odd">
<td><p>Axis2 Configuration ( <code>              axis2.config             </code> )</p></td>
<td><p>The location path of the Axis2 Configuration file to be used to process messages prior to sending them to the endpoint.</p></td>
<td><p>No (The default value is <code>              repository/conf/axis2/axis2_blocking_client.xml             </code> )</p></td>
</tr>
<tr class="even">
<td><p>Reply sequence name ( <code>              message.processor.reply.sequence             </code> )</p></td>
<td><p>The name of the sequence where the message reply should be sent.</p></td>
<td><p>No</p></td>
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
<td><p>Fail messages store ( <code>              message.processor.failMessagesStore             </code> )</p></td>
<td><div class="content-wrapper">
<p>The name of a backup message store to forward messages that fail to reach the backend. Specifying a backup message store allows the message processor to continue processing the next message instead of deactivating in case a message fails to be delivered to the backend. If a backup message store is not specified and a message fails to reach the backend, the message processor will attempt to redeliver the message based on the value of the <strong>Maximum redelivery attempts</strong> , before deactivating.</p>
!!! note
<p>Note</p>
<p>Specifying a backup message store while the <strong>Drop message after maximum delivery attempts</strong> ( <code>               max.delivery.drop              </code> ) parameter is <code>               Enabled              </code> results in messages that are not delivered being dropped from the message store and allows the message processor to continue processing the next message.</p>

</div></td>
<td>No</td>
</tr>
<tr class="even">
<td><p>Quartz configuration file path ( <code>              quartz.conf             </code> )</p></td>
<td><p>The Quartz configuration file path. This properties file contains the Quartz configuration<br />
parameters for fine tuning the Quartz engine. More details of the configuration can be<br />
found at <a href="http://quartz-scheduler.org/documentation/quartz-2.x/configuration/ConfigMain">http://quartz-scheduler.org/documentation/quartz-2.x/configuration/ConfigMain</a> .</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>Cron Expression ( <code>              cronExpression             </code> )</p></td>
<td><p>Interval in milliseconds in which processor consumes messages.</p>
<p>If both Cron Expression and Forwarding Interval are specified in the configuration, Cron Expression will precede the Forwarding Interval.</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td>Task Count ( <code>             member.count            </code> )
<p><br />
</p></td>
<td><p>The number of tasks that need to be created for this message processor.</p>
<p>Specifying this will not guarantee that the processor will run in each worker node. There can be instances where the processor will not run in some workers nodes.</p></td>
<td>No (The default value is 1)</td>
</tr>
</tbody>
</table>

In addition to specifying the required parameters, you can click **Show
Additional Parameters** and set any of the additional parameters if
necessary.

### Message context properties to be used with the scheduled message forwarding processor

| Property Name                               | Value                                                                                                                                                                                                  | Required                        |
|---------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| `              targetEndpoint             ` | Name of the Address Endpoint where the message should be delivered. This property is deprecated and is no longer required, but for backward compatibility, it does not cause errors if it is included. | Yes                             |
| OUT\_ONLY                                   | Set to `              true             ` if this is an out-only message                                                                                                                                | Required for out-only scenarios |
| FORCE\_ERROR\_ON\_SOAP\_FAULT               | Set to `              true             ` if it is required to retry in case of SOAP fault.                                                                                                             | NO                              |

### Samples

For samples illustrating how to use the message forwarding processor,
see:

-   [Sample 702: Introduction to Message Forwarding
    Processor](https://docs.wso2.com/display/EI650/Sample+702%3A+Introduction+to+Message+Forwarding+Processor)
-   [Sample 703: Adding Security to Message Forwarding
    Processor](https://docs.wso2.com/display/EI650/Sample+703%3A+Adding+Security+to+Message+Forwarding+Processor)
-   [Sample 704: RESTful Invocations with Message Forwarding
    Processor](https://docs.wso2.com/display/EI650/Sample+704%3A+RESTful+Invocations+with+Message+Forwarding+Processor)
-   [Sample 705: Load Balancing with Message Forwarding
    Processor](https://docs.wso2.com/display/EI650/Sample+705%3A+Load+Balancing+with+Message+Forwarding+Processor)
