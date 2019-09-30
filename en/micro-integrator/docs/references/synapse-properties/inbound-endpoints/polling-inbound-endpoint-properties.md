# Polling Inbound Endpoint Properties

See the topics given below for the list of properties that can be configured for a Polling Inbound Endpoint artifact.

## JMS Inbound 

Listed below are the properties used for [creating a JMS inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Required Properties

The following properties are required when [creating a JMS inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>
      <p>java.naming.factory.initial</p>
    </td>
    <td>
      <p>The JNDI initial context factory class. The class must implement the <code>java.naming.spi.InitialContextFactory</code> interface.</p>
    </td>
  </tr>
  <tr>
    <td>
       <p>java.naming.provider.url</p>
     </td>
     <td>
       <p>The URL of the JNDI provider.</p>
     </td>
  </tr>
  <tr>
    <td>
      <p>transport.jms.ConnectionFactoryJNDIName</p>
    </td>
    <td>
      <p>The JNDI name of the connection factory.</p>
    </td>
  </tr>
  <tr>
    <td>interval</td>
    <td>
      The polling interval for the inbound endpoint to execute each cycle. This value is set in milliseconds.
    </td>
  </tr>
  <tr>
    <td>coordination</td>
    <td>
      This optional property is only applicable in a cluster environment. In a clustered environment, an inbound endpoint will only be executed in worker nodes. If set to <code>true</code> in a cluster setup, this will run the inbound only in a single worker node. Once the running worker is down, the inbound starts on another available worker in the cluster. By default, coordniation is enabled.
    </td>
  </tr>
  <tr>
    <td>
       sequential
     </td>
     <td>Whether the messages need to be polled and injected sequentially or not.</td>
  </tr>
</table>

### Optional Properties

The following optional properties can be configured when [creating a JMS inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
   <thead>
      <tr>
         <th>
            <p>Property Name</p>
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
            transport.jms.ConnectionFactoryType
         </td>
         <td>
            The type of the connection factory.</br></br>  Set to <b>queue</b> by default.
         </td>
      </tr>
      <tr>
         <td>
            <p>transport.jms.Destination/p>
         </td>
         <td>
            <p>The JNDI name of the destination.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.SessionAcknowledgement
         </td>
         <td>
            <p>The JMS session acknowledgment mode. You can use one of the following: <code>AUTO_ACKNOWLEDGE</code> , <code>CLIENT_ACKNOWLEDGE</code> , <code>DUPS_OK_ACKNOWLEDGE</code> , <code>SESSION_TRANSACTED</code>.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.CacheLevel
         </td>
         <td>
            <p>The JMS resource cache level. Possible values are as follows: <code>0</code>(none), <code>1</code>(connection), <code>2</code>(session), <code>3</code>(consumer).</br></br> The default value is <code>0-none</code>.</br></br>
            <b>Note</b>:To subscribe to topics, set the value of <code>transport.jms.CacheLevel</code> to <code>3</code>.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.UserName
         </td>
         <td>
            <p>The JMS connection username.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.Password
         </td>
         <td>
            <p>The JMS connection password.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.JMSSpecVersion
         </td>
         <td>
            <p>The JMS API version. The possible values are as follows: <code>1.0.2b</code>, <code>1.1</code>, <code>2.0</code>.</br></br> The default value is <code>1.1.</code>.</p>
         </td>
      </tr>
      <tr>
         <td>transport.jms.SubscriptionDurable</td>
         <td>Whether the connection factory is subscription durable or not.</td>
      </tr>
      <tr>
         <td>transport.jms.DurableSubscriberClientID</td>
         <td>The <code>ClientId</code> parameter when using durable subscriptions. This property is required if the value specified as <code>transport.jms.SubscriptionDurable</code> is <code>true</code>.</td>
      </tr>
      <tr>
         <td>
            transport.jms.DurableSubscriberName
         </td>
         <td>
            <p>The name of the durable subscriber. This property is required if the value specified as <code>transport.jms.SubscriptionDurable</code> is <code>true</code>.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.MessageSelector
         </td>
         <td>
            Message selector implementation.
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.ReceiveTimeout
         </td>
         <td>The time to wait for a JMS message during polling.<br />
            Set this parameter value to a negative integer to wait indefinitely. Set it to zero to prevent waiting.</br></br> The default value is 1.
         </td>
      </tr>
      <tr>
         <td>transport.jms.ContentType</td>
         <td>How the inbound listener should determine the content type of received messages. Priority is always given to the JMS message type. Possible values are any simple string value. In which case the transport listener assumes that the received messages always have the specified content type, or a set of rules. For more information, see <a href="http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration">http://axis.apache.org/axis2/java/transports/jms.html#Service_configuration</a>.</td>
      </tr>
      <tr>
         <td>transport.jms.ContentTypeProperty</td>
         <td>Gets the content type from the message property.</td>
      </tr>
      <tr>
         <td>transport.jms.ReplyDestination</td>
         <td>The destination where the response generated by the back-end service is stored.</td>
      </tr>
      <tr>
         <td>
            <p>transport.jms.PubSubNoLocal</p>
         </td>
         <td>
            <p>Whether messages should be published via the same connection that they were received.</p>
         </td>
      </tr>
      <tr>
         <td>
            transport.jms.SharedSubscription
         </td>
         <td>
            <p>If set to <code>true</code>, messages will be forwarded to only one of the consumers and consumers will share the messages that are published to the topic.</p>
         </td>
      </tr>
      <tr>
         <td>
            <p>pinnedServers</p>
         </td>
         <td>
            <p>List of synapse server names separated by commas or spaces where this inbound endpoint should be deployed. If there is no pinned server list, the inbound endpoint configuration will be deployed in all server instances.</p>
         </td>
      </tr>
      <tr>
         <td>transport.jms.ConcurrentConsumers</td>
         <td>Number of concurrent threads to be started to consume messages when polling.</br></br> The default value is 1. You can change this to any positive integer. However, for topics the value must always be 1.</td>
      </tr>
      <tr>
         <td>
            transport.jms.retry.duration
         </td>
         <td>
            <p>The retry interval (in miliseconds) to reconnect to the JMS server.</p>
         </td>
      </tr>
      <tr>
         <td>transport.jms.RetriesBeforeSuspension</td>
         <td>The number of consecutive mediation failures after which polling should be suspended. Specify any positive numerical value based on your requirement. Polling will be suspended when the mediation failure count reaches the specified value.</td>
      </tr>
      <tr>
         <td>transport.jms.PollingSuspensionPeriod</td>
         <td>The period of time that polling is to be suspended when the <code>             transport.jms.RetriesBeforeSuspension</code> parameter is set.</br></br> Default value is 60000.</td>
      </tr>
   </tbody>
</table>

## Kafka Inbound 

Listed below are the properties used for [creating an Kafka inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

### Required Properties

The following properties are required when [creating a Kafka inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
      <td>
        zookeeper.connect
      </td>
      <td>The host and port of a ZooKeeper server (<code>hostname:port</code>).</td>
      </tr>
      <tr>
         <td>
            consumer.type
         </td>
         <td>The consumer configuration type. This can either be <code>simple</code> or <code>highlevel</code>.</td>
      </tr>
      <tr>
         <td>
          interval
         </td>
         <td>The polling interval for the inbound endpoint to poll the messages.</td>
      </tr>
      <tr>
         <td>
          coordination
         </td>
         <td>If set to <code>true</code> in a clustered setup, this will run the inbound only in a single worker node.</br></br> The property is set to <code>true</code> by default.</td>
      </tr>
      <tr>
         <td>
           sequential
         </td>
         <td>The behaviour when executing the given sequence.</br></br> The property is set to <code>true</code> by default. Set this property to <code>false</code> to use the Kafka inbound in a non-sequential mode as it allows better performance than the sequential mode.</td>
      </tr>
      <tr>
         <td>
            topics
         </td>
         <td>The category to feed the messages. A high-level kafka configuration can have more than one topic. You can specify multiple topic names as comma separated values.</td>
      </tr>
      <tr>
         <td>
           content.type
         </td>
         <td>The content of the message. The possible values are as follows: <code>appllication/xml</code> or <code>application/json</code>.</td>
      </tr>
      <tr>
         <td>
          group.id
         </td>
         <td>
            <p>If all the consumer instances have the same consumer group, this works as a traditional queue balancing the load over the consumers.</p>
         </td>
      </tr>
      <tr>
         <td>
            topic.filter
         </td>
         <td>The name of the topic filter.</td>
      </tr>
      <tr>
         <td>
            filter.from.whitelist
         </td>
         <td>If this is set to <code>true</code>, messages are consumed from the whitelist(include).<br />
            If this is set to <code>false</code>, messages are consumed from the blacklist(exclude).
         </td>
      </tr>
      <tr>
    <td>coordination</td>
    <td>
      This optional property is only applicable in a cluster environment. In a clustered environment, an inbound endpoint will only be executed in worker nodes. If set to <code>true</code> in a cluster setup, this will run the inbound only in a single worker node. Once the running worker is down, the inbound starts on another available worker in the cluster. By default, coordniation is enabled.
    </td>
  </tr>
  <tr>
    <td>
       sequential
     </td>
     <td>Whether the messages need to be polled and injected sequentially or not.</td>
  </tr>
</table>

### Optional Properties

The following optional properties can be configured when [creating a Kafka inbound endpiont](../../../develop/creating-artifacts/creating-an-inbound-endpoint.md).

<table>
   <thead>
      <tr>
         <th>
           Property Name
         </th>
         <th>
            <p>Description</p>
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
          thread.count
         </td>
         <td>The number of threads. The default value is 1.</td>
      </tr>
      <tr>
         <td>consumer.id</td>
         <td>The id of the consumer. The default value is <code>null</code>.</td>
      </tr>
      <tr>
         <td>socket.timeout.ms</td>
         <td>The socket timeout for network requests. The default value is <code>30 * 1000</code>.</td>
      </tr>
      <tr>
         <td>socket.receive.buffer.bytes</td>
         <td>The socket receive buffer for network requests. The default value is <code>64 * 1024</code>.</td>
      </tr>
      <tr>
         <td>fetch.message.max.bytes</td>
         <td>The number of bytes of messages that the system should attempt to fetch for each topic-partition in each fetch request. The default values is <code>1024 * 1024</code>.</td>
      </tr>
      <tr>
         <td>num.consumer.fetchers</td>
         <td>The number fetcher threads used to fetch data. The default value is 1.</td>
      </tr>
      <tr>
         <td>auto.commit.enable</td>
         <td>The committed offset to be used as the position from which the new consumer will begin when the process fails.</br></br> The default value is <code>true</code>.</td>
      </tr>
      <tr>
         <td>auto.commit.interval.ms</td>
         <td>The frequency (in miliseconds) at which the consumer offsets are committed to zookeeper.</br></br> The default value is <code>60 * 1000</code>.</td>
      </tr>
      <tr>
         <td>queued.max.message.chunks</td>
         <td>The maximum number of message chunks buffered for consumption. Each chunk can go up to the value specified in <code>fetch.message.max.bytes</code>.</br></br> The default value is 2.</td>
      </tr>
      <tr>
         <td>rebalance.max.retries</td>
         <td>The maximum number of retry attempts. The default value is 4.</td>
      </tr>
      <tr>
         <td>fetch.min.bytes</td>
         <td>The minimum amount of data the server should return for a fetch request. The default value is 1.</td>
      </tr>
      <tr>
         <td>fetch.wait.max.ms</td>
         <td>The maximum amount of time the server will stay blocked before responding to the fetch request when sufficient data is not available to immediately serve <code>fetch.min.bytes</code>.</br></br> The default value is <code>100</code></td>
      </tr>
      <tr>
         <td>rebalance.backoff.ms</td>
         <td>The backoff time between retries during rebalance. The default value is 2000.</td>
      </tr>
      <tr>
         <td>refresh.leader.backoff.ms</td>
         <td>The backoff time to wait before trying to determine the leader of a partition that has just lost its leader.</br></br> The default value is 200.</td>
      </tr>
      <tr>
         <td>auto.offset.reset</td>
         <td>
            <p>Set this to one of the following values based on what you need to do when there is no initial offset in ZooKeeper or if an offset is out of range.</p>
            <ul>
              <li><b>smallest</b>: Automatically reset the offset to the smallest offset.</li>
              <li><b>largest</b>: Automatically reset the offset to the largest offset.</li>
              <li><b>anything else</b>: Throw an exception to the consumer.</li>
            </ul>
            The default values is <b>largest</b>.
         </td>
      </tr>
      <tr>
         <td>consumer.timeout.ms</td>
         <td>The timeout interval after which a timeout exception is to be thrown to the consumer if no message is available for consumption. It is a good practice to set this value lower than the interval of the Kafka inbound endpoint. The default value is 2000.</td>
      </tr>
      <tr>
         <td>exclude.internal.topics</td>
         <td>Set to <code>true</code> if messages from internal topics such as offsets should be exposed to the consumer. The default value is <code>true</code>.</td>
      </tr>
      <tr>
         <td>partition.assignment.strategy</td>
         <td>The partitions assignment strategy to be used when assigning partitions to consumer streams. Possible values are <code>range</code> and <code>roundrobin</code>.</br></br> Default setting is <code>range</code>.</td>
      </tr>
      <tr>
         <td>client.id</td>
         <td>The user specified string sent in each request to help trace calls.</td>
      </tr>
      <tr>
         <td>
            <p>zookeeper.session.timeout.ms</p>
         </td>
         <td>The ZooKeeper session timeout value in milliseconds. The default value is 6000.</td>
      </tr>
      <tr>
         <td>zookeeper.connection.timeout.ms</td>
         <td>The maximum time in milliseconds that the client should wait while establishing a connection to ZooKeeper.</br></br> The default value is 6000.</td>
      </tr>
      <tr>
         <td>
            <p>zookeeper.sync.time.ms</p>
         </td>
         <td>The time difference in milliseconds that a ZooKeeper follower can be behind a ZooKeeper leader. The default value is 2000.</td>
      </tr>
      <tr>
         <td>offsets.storage</td>
         <td>The offsets storage location. Possible values are <code>zookeeper<code> and <code>kafka</code>. Default setting is <code>zookeeper</code>.</td>
      </tr>
      <tr>
         <td>offsets.channel.backoff.ms</td>
         <td>The backoff period in milliseconds when reconnecting the offsets channel or retrying failed offset fetch/commit requests.</br></br> Default value is 1000.</td>
      </tr>
      <tr>
         <td>offsets.channel.socket.timeout.ms</td>
         <td>The socket timeout in milliseconds when reading responses for offset fetch/commit requests.</br></br> The default value is 10000.</td>
      </tr>
      <tr>
         <td>offsets.commit.max.retries</td>
         <td>The maximum retry attempts allowed. If a consumer metadata request fails for any reason, retry takes place but does not have an impact on this limit.</br></br> Default value is 5.</td>
      </tr>
      <tr>
         <td>dual.commit.enabled</td>
         <td>If <code>offsets.storage</code> is set to <code>kafka</code>, the commit offsets can be dual to ZooKeeper. Set this to <code>true</code> if you need to perform migration from zookeeper-based offset storage to kafka-based offset storage. The default value is <code>true</code>.</td>
      </tr>
      <tr>
         <td>
            simple.topic
         </td>
         <td>The category to feed the messages.</td>
      </tr>
      <tr>
         <td>
            simple.brokers
         </td>
         <td>The specific Kafka broker name.</td>
      </tr>
      <tr>
         <td>
            simple.port
         </td>
         <td>The specific Kafka server port number.</td>
      </tr>
      <tr>
         <td>
            simple.partition
         </td>
         <td>The partition of the topic.</td>
      </tr>
      <tr>
         <td>
            simple.max.messages.to.read
         </td>
         <td>
            <p>The maximum number of messages to retrieve.</p>
         </td>
      </tr>
   </tbody>
</table>