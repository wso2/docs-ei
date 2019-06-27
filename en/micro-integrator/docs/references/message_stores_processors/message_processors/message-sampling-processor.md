# Message Sampling Processor

The message sampling processor consumes messages in a [message
store](_Message_Stores_) and sends them to a configured
[sequence](https://docs.wso2.com/display/EI650/Mediation+Sequences) .
This process happens in a preconfigured interval. This message processor
does not ensure reliable messaging.

### Message sampling processor parameters

Following are the additional parameters you can set when adding a
message sampling processor:

<table>
<tbody>
<tr class="odd">
<td>Processor State ( <code>             is.active            </code> )</td>
<td>Activate ( <code>             true            </code> ) or Deactivate ( <code>             false            </code> )</td>
<td>No. The default is Activate ( <code>             true            </code> ).</td>
</tr>
<tr class="even">
<td>Sampling Interval ( <code>             interval            </code> )</td>
<td><p>Interval in milliseconds in which processor consumes messages</p></td>
<td>No. The default value is 1000.</td>
</tr>
<tr class="odd">
<td><p>Sampling Concurrency ( <code>              concurrency             </code> )</p></td>
<td><p>The number of messages the processor consumes at a time</p></td>
<td><p>No. The default is value is 1.</p></td>
</tr>
<tr class="even">
<td><p>Quartz Configuration File Path ( <code>              quartz.conf             </code> )</p></td>
<td><p>Quartz configuration file path. This properties file contains the Quartz configuration<br />
parameters for fine tuning the Quartz engine. More details of the configuration can be<br />
found at <a href="http://quartz-scheduler.org/documentation/quartz-2.x/configuration/ConfigMain">http://quartz-scheduler.org/documentation/quartz-2.x/configuration/ConfigMain</a> .</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>Cron Expression ( <code>              cronExpression             </code> )</p></td>
<td><p>Cron expression to be used to configure the retry pattern</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

### Samples

For samples illustrating how to use the message sampling processor, see:

-   [Sample 701: Introduction to Message Sampling
    Processor](https://docs.wso2.com/display/EI650/Sample+701%3A+Introduction+to+Message+Sampling+Processor)
