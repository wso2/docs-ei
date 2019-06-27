# Tuning Flow Control

Flow control is typically employed for controlling fast producers from
overloading slow consumers in producer-consumer scenarios. There may be
several reasons for a fast producer-slow consumer scenario. For example,
the consumer can be on a low resource footprint; or the message broker,
which lies in the middle of the producer and the consumer, may get
overloaded at a particular moment due to message accumulation within the
broker itself. This can cause message broker instances to run out of
resources, such as memory.

The Message Broker profile primarily supports buffer limit-based flow
control. This involves blocking message acceptance when the rate at
which messages are transmitted reaches a *high limit* , and unblocking
it when this rate reaches a *low limit* .

This is further illustrated in the following matrix.

|            | Global                                                                                                                                                                                                                                                                         | Per-publisher                                                                                                                                                                                                                                                                                                       |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| High Limit | When the total number of message content chunks reach the [global high limit](#TuningFlowControl-global) , flow control is enabled and message acceptance is blocked for all the publishers.                                                                                   | When the total number of message content chunks by an individual publisher reach the [per-publisher high limit](#TuningFlowControl-channel) , flow control is enabled for that publisher. As a result, message acceptance will be blocked for the publisher.                                                        |
| Low Limit  | If flow control is currently enabled, it will be disabled only when the total number of message content chunks reach the [global low limit](#TuningFlowControl-global) . Once this limit is reached, all the publishers are notified so that they can resume sending messages. | If flow control is currently enabled for an individual publisher, it will be disabled only when the total number of message content chunks reach the [per-publisher low limit](#TuningFlowControl-channel) . Once this limit is reached, the publisher will be notified so that he/she can resume sending messages. |

This section explains how to tune the performance of the Message Broker
profile in relation to flow control. The parameters described below can
be configured in the
`         <EI_HOME>/wso2/broker/conf/broker.xml        ` file.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Improvement Area</th>
<th>Description</th>
<th>Performance Recommendations</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Enabling/disabling flow control based on message content chunk limits</td>
<td><p>You can set the global limits and channel-specific (i.e., per publisher) limits for message content chunks in the <code style="line-height: 1.42857;">              broker.xml             </code> file as shown below.</p>
<ul>
<li><p>To define the global message content limits, set the <code>                &lt;lowLimit&gt;               </code> and <code>                &lt;highLimit&gt;               </code> parameters as shown below. These limits are applicable to all channels collectively.</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;flowControl&gt;</span>
<span id="cb1-2"><a href="#cb1-2"></a>        &lt;!-- This is the global buffer limits which enable/disable the flow control  globally --&gt;</span>
<span id="cb1-3"><a href="#cb1-3"></a>        &lt;global&gt;</span>
<span id="cb1-4"><a href="#cb1-4"></a>            &lt;lowLimit&gt;<span class="dv">800</span>&lt;/lowLimit&gt;</span>
<span id="cb1-5"><a href="#cb1-5"></a>            &lt;highLimit&gt;<span class="dv">8000</span>&lt;/highLimit&gt;</span>
<span id="cb1-6"><a href="#cb1-6"></a>        &lt;/global&gt;</span>
<span id="cb1-7"><a href="#cb1-7"></a>        .................</span>
<span id="cb1-8"><a href="#cb1-8"></a>&lt;/flowControl&gt;</span></code></pre></div>
</div>
</div></li>
<li><p>To define the channel specific buffer limits, set the <code style="line-height: 1.42857;">                &lt;lowLimit&gt;               </code> and <code style="line-height: 1.42857;">                &lt;highLimit&gt;               </code> parameters as shown below. These limits will be applicable to each channel.<br />
</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>&lt;flowControl&gt;</span>
<span id="cb2-2"><a href="#cb2-2"></a>        ................</span>
<span id="cb2-3"><a href="#cb2-3"></a>        &lt;!-- This is the channel specific buffer limits which enable/disable the flow control locally.--&gt;</span>
<span id="cb2-4"><a href="#cb2-4"></a>        &lt;bufferBased&gt;</span>
<span id="cb2-5"><a href="#cb2-5"></a>            &lt;lowLimit&gt;<span class="dv">100</span>&lt;/lowLimit&gt;</span>
<span id="cb2-6"><a href="#cb2-6"></a>            &lt;highLimit&gt;<span class="dv">1000</span>&lt;/highLimit&gt;</span>
<span id="cb2-7"><a href="#cb2-7"></a>        &lt;/bufferBased&gt;</span>
<span id="cb2-8"><a href="#cb2-8"></a>&lt;/flowControl&gt;</span></code></pre></div>
</div>
</div></li>
</ul></td>
<td><p>Having a large number as the higher limit would increase the number of messages stored in memory before they are stored in databases. This would result in a higher overall message publishing rate, but with reduced reliability.</p>
<p>If the difference between the higher limit and the lower limit is too small, it would cause frequent enabling and disabling of flow control. This would reduce the overall message publishing rate.</p>
<p>Default or recommended values are as follows.</p>
<p>Global limits:</p>
<ul>
<li>Low limit: 800</li>
<li>High limit: 8000</li>
</ul>
<p>Buffer based limits:</p>
<ul>
<li>High limit: 100</li>
<li>Low limit: 1000</li>
</ul></td>
</tr>
<tr class="even">
<td>Enabling/disabling flow control based on the memory</td>
<td><p>The <code>              globalMemoryRecoveryThresholdRatio             </code> parameter allows you to specify the memory consumption ratio at which flow control should be enabled (if it is currently disabled). This ratio is calculated using the following formula.</p>
<p>Used Memory/Allocated Memory</p>
<p>You can also specify the time interval at which the server should check whether the above ratio is reached via the <code>              memoryCheckInterval             </code> parameter.</p></td>
<td>If the publisher throughput is very high compared to the consumer throughput, this ratio should be reduced (to a maximum value of 1) to avoid out of memory scenarios. At the same time, the value for the <code>             memoryCheckInterval            </code> parameter should be reduced to perform more frequent checks on memory availability.</td>
</tr>
</tbody>
</table>
