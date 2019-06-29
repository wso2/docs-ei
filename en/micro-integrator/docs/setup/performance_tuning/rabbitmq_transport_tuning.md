# Tuning the RabbitMQ Transport

RabbitMQ transport allows you to send or receive AMQP messages by
calling an AMQP broker (RabbitMQ) directly. In order to improve the
performance of the RabbitMQ transport you can do following.

### Set queue and exchange declaration parameters to `         false        `

Setting `         rabbitmq.queue.autodeclare        ` and
`         rabbitmq.exchange.autodeclare        ` parameters in the
publish url to `         false        ` can improve the
RabbitMQ transport performance.

When these parameters are set to `         false        ` RabbitMQ does
not try to create queues or exchanges if queues or exchanges are not
present. However, you should set these parameters if and only if queues
and exchanges are declared prior on the broker.

### Increase the connection pool size

You can increase the connection pool size to improve the performance of
the RabbitMQ sender and listener. The default connection pool size is
20. To change this, specify a required value for the
following parameter in the RabbitMQ transport sender and listener
configurations in the `         <EI_HOME>/conf/axis2/axis2.xml        `
file.

``` xml
    <parameter name="rabbitmq.connection.pool.size" locked="false">25</parameter>
```

``` xml
    <transportReceiver name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQListener">
       <parameter name="AMQPConnectionFactory" locked="false">
          <parameter name="rabbitmq.server.host.name" locked="false">localhost</parameter>
          <parameter name="rabbitmq.server.port" locked="false">5672</parameter>
          <parameter name="rabbitmq.server.user.name" locked="false"></parameter>
          <parameter name="rabbitmq.server.password" locked="false"></parameter>
          <parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
          <parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>
          <parameter name="rabbitmq.connection.pool.size" locked="false">25</parameter>
        </parameter>
    </transportReceiver>
```

``` xml
    <transportSender name="rabbitmq" class="org.apache.axis2.transport.rabbitmq.RabbitMQSender">
       <parameter name="RabbitMQConnectionFactory" locked="false">
           <parameter name="rabbitmq.server.host.name" locked="false">localhost</parameter>
           <parameter name="rabbitmq.server.port" locked="false">5672</parameter>
           <parameter name="rabbitmq.server.user.name" locked="false"></parameter>
           <parameter name="rabbitmq.server.password" locked="false"></parameter>
           <parameter name="rabbitmq.connection.retry.interval" locked="false">10000</parameter>
           <parameter name="rabbitmq.connection.retry.count" locked="false">5</parameter>
           <parameter name="rabbitmq.connection.pool.size" locked="false">10</parameter>
       </parameter>
    </transportSender>
```
### Increasing the member count

If you set the following parameter to false in the
`         <EI_HOME>/conf/axis2/axis2.xml        ` file, it sets the
configuration to handle the acknowledgement in the application level:
`         <parameter name="rabbitmq.queue.auto.ack">false</parameter>        `

Thus, it sends back the acknowledgement once it sends the request to the
back-end. (i.e., It does not wait for the response). However, in a
message store/processor implementation, it waits for the response to
send the acknowledgment back to provide guaranteed message delivery.
Therefore, there might be a delay in processing messages using message
processors than using a message producer/consumer implementation.

However, you can increase performance of message processors either by
[increasing the member
count](https://docs.wso2.com/display/EI650/Message+Processing+in+a+Worker-Manager+Cluster+Mode)
or by having multiple message processors. If you increase the member
count, it will create multiple child processors of the message
processor.

### Reuse the connection factory in the publisher

In the publisher url, set the connection factory name instead of the
connection parameters as specified below in the
`         rabbitmq.connection.factory        ` parameter . This reuses
the connection factories and thereby improves performance.

``` xml
    <address uri="rabbitmq://?rabbitmq.connection.factory=RabbitMQConnectionFactory&amp;rabbitmq.queue.name=queue1&amp;rabbitmq.queue.routing.key=queue1&amp;rabbitmq.replyto.name=replyqueue&amp;rabbitmq.exchange.name=ex1&amp;rabbitmq.queue.autodeclare=false&amp;rabbitmq.exchange.autodeclare=false&amp;rabbitmq.replyto.name=response_queue"/>
```
