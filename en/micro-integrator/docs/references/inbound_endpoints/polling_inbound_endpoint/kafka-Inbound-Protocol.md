# Kafka Inbound Protocol

Kafka is a distributed, partitioned, replicated commit log service. It
provides the functionality of a messaging system. Kafka maintains feeds
of messages in topics. Producers write data to topics and consumers read
from topics. For more information on Apache Kafka, go to [Apache Kafka
documentation](http://kafka.apache.org/documentation.html) .

The kafka inbound endpoint acts as a message consumer. It creates a
connection to ZooKeeper and requests messages for a topic, topics or
topic filters.

In order to use the kafka inbound endpoint, you need to download and
install [Apache Kafka](http://kafka.apache.org/downloads.html) . The
recommended version is `         kafka_2.9.2-0.8.1.1        ` .

To configure the kafka inbound endpoint, copy the following client
libraries from the `         <KAFKA_HOME>/libs        ` directory to the
`         <EI_HOME>/lib        ` directory.

-   `          kafka_2.9.2-0.8.1.1.jar         `
-   `           scala-library-2.9.2.jar          `

-   `           zkclient-0.3.jar          `

-   `           zookeeper-3.3.4.jar          `

-   `           metrics-core-2.2.0.jar          `

!!! info

Note

-   If you are using `          kafka_2.x.x-0.8.2.0         ` or later,
    you also need to add the
    `          kafka-clients-0.8.x.x.jar         ` file to the
    `          <EI_HOME>/lib         ` directory.
-   If you are using a newer version of ZooKeeper, follow the steps
    below:

    1.  Create a directory named `             conf            ` inside
        the `             <EI_HOME>/repository            ` directory.
    2.  Create a directory named identity inside the
        `             <EI_HOME>/repository/conf            ` directory.
    3.  Add the [jaas.conf](attachments/119130492/119130493.conf) file
        to the
        `             <EI_HOME>/repository/conf/identity            `
        directory. This is required because Kerberos authentication is
        enforced on newer versions of ZooKeeper.


Configuration parameters for a kafka inbound endpoint are XML fragments
that represent various properties.

Following is a sample high level kafka configuration that can be used to
consume messages using the specified topic or topics:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     suspend="false">
       <parameters>
          <parameter name="interval">100</parameter> 
          <parameter name="coordination">true</parameter>
          <parameter name="sequential">true</parameter>
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="consumer.type">highlevel</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="topics">test,sampletest</parameter>
          <parameter name="group.id">test-group</parameter>
       </parameters>
    </inboundEndpoint>
```

### Kafka inbound endpoint parameters for a high level configuration

<table>
<thead>
<tr class="header">
<th><p>Parameter</p>
<p><br />
</p></th>
<th><p>Description</p>
<p><br />
</p></th>
<th><p>Required</p>
<p><br />
</p></th>
<th>Possible Values</th>
<th>Default Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>zookeeper.connect</code></pre></td>
<td>The host and port of a ZooKeeper server ( <code>             hostname:port            </code> ).</td>
<td>Yes</td>
<td>localhost:2181</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><pre><code>consumer.type</code></pre></td>
<td>The consumer configuration type. This can either be <code>             simple            </code> or <code>             highlevel            </code> .</td>
<td>Yes</td>
<td>highlevel, simple</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><pre><code>interval</code></pre></td>
<td>The polling interval for the inbound endpoint to poll the messages.</td>
<td>Yes</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><pre><code>coordination</code></pre></td>
<td>If set to <code>             true            </code> in a cluster setup, this will run the inbound only in a single worker node.</td>
<td>Yes</td>
<td>true, false</td>
<td>true</td>
</tr>
<tr class="odd">
<td><pre><code>sequential</code></pre></td>
<td>The behaviour when executing the given sequence.</td>
<td>Yes</td>
<td>true, false</td>
<td>true</td>
</tr>
<tr class="even">
<td><pre><code>topics</code></pre></td>
<td>The category to feed the messages. A high level kafka configuration can have more than one topic. You can specify multiple topic names as comma separated values.</td>
<td>Yes</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><pre><code>content.type</code></pre></td>
<td>The content of the message.</td>
<td>Yes</td>
<td>appllication/xml, application/json</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><pre><code>group.id</code></pre></td>
<td><p>If all the consumer instances have the same consumer group, this works as a traditional queue balancing the load over the consumers.</p>
<p>If all the consumer instances have different consumer groups, this works as publish-subscribe and all messages are broadcast to all consumers.</p></td>
<td>Yes</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><pre><code>thread.count</code></pre></td>
<td>The number of threads.</td>
<td>No</td>
<td><br />
</td>
<td>1</td>
</tr>
<tr class="even">
<td><code>             consumer.id            </code></td>
<td>The id of the consumer.</td>
<td>No</td>
<td><br />
</td>
<td>null</td>
</tr>
<tr class="odd">
<td><code>             socket.timeout.ms            </code></td>
<td>The socket timeout for network requests.</td>
<td>No</td>
<td><br />
</td>
<td>30 * 1000</td>
</tr>
<tr class="even">
<td><code>             socket.receive.buffer.bytes            </code></td>
<td>The socket receive buffer for network requests.</td>
<td>No</td>
<td><br />
</td>
<td>64 * 1024</td>
</tr>
<tr class="odd">
<td><code>             fetch.message.max.bytes            </code></td>
<td>The number of byes of messages to attempt to fetch for each topic-partition in each fetch request.</td>
<td>No</td>
<td><br />
</td>
<td>1024 * 1024</td>
</tr>
<tr class="even">
<td><code>             num.consumer.fetchers            </code></td>
<td>The number fetcher threads used to fetch data.</td>
<td>No</td>
<td><br />
</td>
<td>1</td>
</tr>
<tr class="odd">
<td><code>             auto.commit.enable            </code></td>
<td>The committed offset to be used as the position from which the new consumer will begin when the process fails.</td>
<td>No</td>
<td>true, false</td>
<td>true</td>
</tr>
<tr class="even">
<td><code>             auto.commit.interval.ms            </code></td>
<td>The frequency in ms that the consumer offsets are committed to zookeeper.</td>
<td>No</td>
<td><br />
</td>
<td>60 * 1000</td>
</tr>
<tr class="odd">
<td><code>             queued.max.message.chunks            </code></td>
<td>The maximum number of message chunks buffered for consumption. Each chunk can go up to the value specified in <code>             fetch.message.max.bytes            </code> .</td>
<td>No</td>
<td><br />
</td>
<td>2</td>
</tr>
<tr class="even">
<td><code>             rebalance.max.retries            </code></td>
<td>The maximum number of retry attempts.</td>
<td>No</td>
<td><br />
</td>
<td>4</td>
</tr>
<tr class="odd">
<td><code>             fetch.min.bytes            </code></td>
<td>The minimum amount of data the server should return for a fetch request.</td>
<td>No</td>
<td><br />
</td>
<td>1</td>
</tr>
<tr class="even">
<td><code>             fetch.wait.max.ms            </code></td>
<td>The maximum amount of time the server will block before responding to the fetch request when sufficient data is not available to immediately serve <code>             fetch.min.bytes            </code> .</td>
<td>No</td>
<td><br />
</td>
<td>100</td>
</tr>
<tr class="odd">
<td><code>             rebalance.backoff.ms            </code></td>
<td>The backoff time between retries during rebalance.</td>
<td>No</td>
<td><br />
</td>
<td>2000</td>
</tr>
<tr class="even">
<td><code>             refresh.leader.backoff.ms            </code></td>
<td>The backoff time to wait before trying to determine the leader of a partition that has just lost its leader.</td>
<td>No</td>
<td><br />
</td>
<td>200</td>
</tr>
<tr class="odd">
<td><code>             auto.offset.reset            </code></td>
<td><p>Set this to one of the following values based on what you need to do when there is no initial offset in ZooKeeper or if an offset is out of range.</p>
<p>smallest - Automatically reset the offset to the smallest offset.<br />
largest - Automatically reset the offset to the largest offset.<br />
anything else - Throw an exception to the consumer.</p></td>
<td>No</td>
<td>smallest, largest, anything else</td>
<td>largest</td>
</tr>
<tr class="even">
<td><code>             consumer.timeout.ms            </code></td>
<td>The timeout interval after which a timeout exception is to be thrown to the consumer if no message is available for consumption. It is a good practice to set this value lower than the interval of the Kafka inbound endpoint.</td>
<td>No</td>
<td><br />
</td>
<td>3000</td>
</tr>
<tr class="odd">
<td><code>             exclude.internal.topics            </code></td>
<td>Set to <code>             true            </code> if messages from internal topics such as offsets should be exposed to the consumer.</td>
<td>No</td>
<td>true, false</td>
<td>true</td>
</tr>
<tr class="even">
<td><code>             partition.assignment.strategy            </code></td>
<td>The partitions assignment strategy to be used when assigning partitions to consumer streams.</td>
<td>No</td>
<td>range, roundrobin</td>
<td>range</td>
</tr>
<tr class="odd">
<td><code>             client.id            </code></td>
<td>The user specified string sent in each request to help trace calls.</td>
<td>No</td>
<td><br />
</td>
<td>value of group  id</td>
</tr>
<tr class="even">
<td><p><code>              zookeeper.session.timeout.ms             </code></p></td>
<td>The ZooKeeper session timeout value in milliseconds.</td>
<td>No</td>
<td><br />
</td>
<td>6000</td>
</tr>
<tr class="odd">
<td><code>             zookeeper.connection.timeout.ms            </code></td>
<td>The maximum time in milliseconds that the client should wait while establishing a connection to ZooKeeper .</td>
<td>No</td>
<td><br />
</td>
<td>6000</td>
</tr>
<tr class="even">
<td><p><code>              zookeeper.sync.time.ms             </code></p></td>
<td>The time difference in milliseconds that a ZooKeeper follower can be behind a ZooKeeper leader.</td>
<td>No</td>
<td><br />
</td>
<td>2000</td>
</tr>
<tr class="odd">
<td><code>             offsets.storage            </code></td>
<td>The offsets storage location.</td>
<td>No</td>
<td>zookeeper, kafka</td>
<td>zookeeper</td>
</tr>
<tr class="even">
<td><code>             offsets.channel.backoff.ms            </code></td>
<td>The backoff period in milliseconds when reconnecting the offsets channel or retrying failed offset fetch/commit requests.</td>
<td>No</td>
<td><br />
</td>
<td>1000</td>
</tr>
<tr class="odd">
<td><code>             offsets.channel.socket.timeout.ms            </code></td>
<td>The socket timeout in milliseconds when reading responses for offset fetch/commit requests.</td>
<td>No</td>
<td><br />
</td>
<td>10000</td>
</tr>
<tr class="even">
<td><code>             offsets.commit.max.retries            </code></td>
<td>The maximum retry attempts allowed. If a consumer metadata request fails for any reason, retry takes place but does not have an impact on this limit.</td>
<td>No</td>
<td><br />
</td>
<td>5</td>
</tr>
<tr class="odd">
<td><code>             dual.commit.enabled            </code></td>
<td>If <code>             offsets.storage            </code> is set to <code>             kafka            </code> , the commit offsets can be dual to ZooKeeper. Set this to true if you need to perform migration from zookeeper-based offset storage to kafka-based offset storage.</td>
<td>No</td>
<td>true, false</td>
<td>true</td>
</tr>
</tbody>
</table>

Following is a sample high level kafka configuration that can be used to
consume messages using the topics filter,  which can either be a white
list or a black list:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     suspend="false">
       <parameters>
          <parameter name="interval">100</parameter> 
          <parameter name="coordination">true</parameter>
          <parameter name="sequential">true</parameter>
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="consumer.type">highlevel</parameter>
          <parameter name="content.type">application/xml</parameter>
          <parameter name="topic.filter">test</parameter>
          <parameter name="filter.from.whitelist">true</parameter>
          <parameter name="group.id">test-group</parameter>      
       </parameters>
    </inboundEndpoint>
```

In the above configuration, you will see that the following parameters
are set to consume topic filters:

!!! info

Note

In high level kafka configurations, the follwing parameters are used
instead of the `         topics        ` paramater.


    <parameter name="topic.filter">test</parameter>
    <parameter name="filter.from.whitelist">true</parameter>

  

Following are descriptions of the parameters set to consume topic
filters in a kafka configuration:

<table>
<thead>
<tr class="header">
<th><p>Parameter</p>
<p><br />
</p></th>
<th><p>Description</p>
<p><br />
</p></th>
<th><p>Required</p>
<p><br />
</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>topic.filter</code></pre></td>
<td>The name of the topic filter.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>filter.from.whitelist</code></pre></td>
<td>If this is set to <code>             true            </code> , messages are consumed from the whitelist(include).<br />
If this is set to <code>             false            </code> , messages are consumed from the blacklist(exclude).</td>
<td>Yes</td>
</tr>
</tbody>
</table>

Following is a sample low level kafka configuration that can be used to
consume messages from a specific server in a specific partition, so that
the messages are limited:

``` html/xml
    <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                     name="KakfaListenerEP"
                     sequence="requestHandlerSeq"
                     onError="inFaulte"
                     protocol="kafka"
                     interval="1000"
                     suspend="false">
       <parameters>     
          <parameter name="zookeeper.connect">localhost:2181</parameter>
          <parameter name="group.id">test-group</parameter>  
          <parameter name="content.type">application/xml</parameter>
          <parameter name="consumer.type">simple</parameter>
          <parameter name="simple.max.messages.to.read">5</parameter>
          <parameter name="simple.topic">test</parameter>
          <parameter name="simple.brokers">localhost</parameter>
          <parameter name="simple.port">9092</parameter>
          <parameter name="simple.partition">1</parameter>
          <parameter name="interval">1000</parameter>
       </parameters>
    </inboundEndpoint>
```

### Kafka inbound endpoint parameters for a low level configuration

<table>
<thead>
<tr class="header">
<th><p>Parameter</p>
<p><br />
</p></th>
<th><p>Description</p>
<p><br />
</p></th>
<th><p>Required</p>
<p><br />
</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>simple.topic</code></pre></td>
<td>The category to feed the messages.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>simple.brokers</code></pre></td>
<td>The specific Kafka broker name.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>simple.port</code></pre></td>
<td>The specific Kafka server port number.</td>
<td>Yes</td>
</tr>
<tr class="even">
<td><pre><code>simple.partition</code></pre></td>
<td>The partition of the topic.</td>
<td>Yes</td>
</tr>
<tr class="odd">
<td><pre><code>simple.max.messages.to.read</code></pre></td>
<td><p>The maximum number of messages to retrieve.</p></td>
<td>Yes</td>
</tr>
</tbody>
</table>

### Samples

For a sample that demonstrates how one way message bridging from Kafka
to HTTP can be done using the kafka inbound endpoint, see [Sample 904:
Kafka Inbound Endpoint
Sample](https://docs.wso2.com/display/EI620/Sample+904%3A+Inbound+Endpoint+Kafka+Protocol+Sample)
.
