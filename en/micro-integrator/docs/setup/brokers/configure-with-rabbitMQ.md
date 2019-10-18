# Connecting to RabbitMQ

This section describes how to configure WSO2 Micro Integrator to connect with RabbitMQ.

## Enabling the RabbitMQ transport 

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.listener]]
name = "AMQPConnectionFactory"
parameter.hostname = "localhost"
parameter.port = 5672
parameter.username = "guest"
parameter.password = "guest"
```

Download the "amqp-client-5.7.0.jar" and copy it into `MI_HOME/lib` directory.

## Enabling SSL

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.listener]]
parameter.ssl_enable = true
parameter.ssl_version = "SSL"
parameter.keystore_file_name ="$ref{keystore.tls.file_name}"
parameter.keystore_type = "$ref{keystore.tls.type}"
parameter.keystore_password = "$ref{keystore.tls.password}"
parameter.truststore_file_name ="$ref{truststore.file_name}"
parameter.truststore_type = "$ref{truststore.type}"
parameter.truststore_password = "$ref{truststore.password}"
```
See the complete list of server-level configurations for the [RabbitMQ Listener](../../../references/config-catalog/#rabbitmq-listener) and [RabbitMQ Sender](../../../references/config-catalog/#rabbitmq-sender).

## Configuring connection recovery

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.listener]]
parameter.retry_interval = "10"
parameter.retry_count = 5  
```

In case of a network failure or broker shutdown, WSO2 Micro Integrator will try to recreate the connection. The following parameters need to be configured in the transport listener to enable connection recovery in RabbitMQ.

If the parameters specified above are set, the Micro Integrator will retry 5 times with 10000 ms time intervals to reconnect when the connection is lost. If reconnecting also fails, the Micro Integrator will terminate the connection. If you do not specify values for the above parameters, the Micro Integrator uses 30000ms as the default retry interval and 3 as the default retry count.

Optionally, you can configure the following parameter in your proxy service when you define your mediation sequence:

<parameter name="rabbitmq.server.retry.interval" locked="false">10000</parameter> 

The parameter specified above sets the retry interval with which the RabbitMQ client tries to reconnect. Generally having this value less than the value specified as `rabbitmq.connection.retry.interval` will help synchronize the reconnection of the Micro Integrator and the RabbitMQ client.

## Configuring high throughput of message delivery

For increased performance and higher throughput in message delivery, configure the transport sender as shown below.

Add the following parameters to the deployment.toml file (stored in the `MI_HOME/conf` directory).

```toml
[[transport.rabbitmq.sender]]
name = "CachedRabbitMQConnectionFactory"
parameter.hostname = "localhost"
parameter.port = 5672
parameter.username = "guest"
parameter.password = "guest"
```
When configuring the proxy service, be sure to add the following connection factory parameter in the address URI: `rabbitmq.connection.factory=CachedRabbitMQConnectionFactory`.