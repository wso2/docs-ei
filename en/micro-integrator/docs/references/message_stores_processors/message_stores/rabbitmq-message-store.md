# RabbitMQ Message Store

RabbitMQ message store persists messages in a RabbitMQ queue inside a
RabbitMQ broker. The RabbitMQ message store can be configured by
specifying the class as
`         org.apache.synapse.message.store.impl.rabbitmq.RabbitmqStore        `
and then setting all other required parameters to connect to a
RabbitMQ broker.

-   [Required parameters](#RabbitMQMessageStore-Requiredparameters)
-   [SSL enabled RabbitMQ message store
    parameters](#RabbitMQMessageStore-SSLEnabledParaSSLenabledRabbitMQmessagestoreparameters)
-   [Additional RabbitMQ message store
    parameters](#RabbitMQMessageStore-RabbitMQParaAdditionalRabbitMQmessagestoreparameters)
-   [Example RabbitMQ message store
    configurations](#RabbitMQMessageStore-ExampleRabbitMQmessagestoreconfigurations)

### Required parameters

When you add a RabbitMQ message store, it is required to specify values
for the following:

-   **Name** - A unique name for the RabbitMQ message store.
-   **RabbitMQ Server Host Name** (
    `          store.rabbitmq.host.name         ` ) - The address of the
    RabbitMQ broker.
-   **RabbitMQ Server Host Port** (
    `          store.rabbitmq.host.port         ` ) - The port number of
    the RabbitMQ message broker.
-   **SSL Enabled** - Whether or not SSL is enabled on the message
    store.

When **SSL Enabled** is set to true, you can set the parameters relating
to the SSL configuration. For descriptions of each of these parameters
you can set, see [SSL enabled RabbitMQ message store
parameters](#RabbitMQMessageStore-SSLEnablePara) .

### SSL enabled RabbitMQ message store parameters

| Parameter Name                                                                                      | Value                                                                                                                                 |
|-----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| SSL Key Store Location ( `             rabbitmq.connection.ssl.keystore.location            ` )     | The location of the keystore file.                                                                                                    |
| SSL Key Store Type ( `             rabbitmq.connection.ssl.keystore.type            ` )             | The type of the keystore used (e.g., JKS, PKCS12).                                                                                    |
| SSL Key Store Password ( `             rabbitmq.connection.ssl.keystore.password            ` )     | The password to access the keystore.                                                                                                  |
| SSL Trust Store Location ( `             rabbitmq.connection.ssl.truststore.location            ` ) | The location of the Java keystore file containing the collection of CA certificates trusted by this application process (truststore). |
| SSL Trust Store Type ( `             rabbitmq.connection.ssl.truststore.type            `           | The type of the truststore used.                                                                                                      |
| SSL Trust Store Password ( `             rabbitmq.connection.ssl.truststore.password            ` ) | The password to unlock the trust store file specified in `             rabbitmq.connection.ssl.truststore.location            `       |
| SSL Version ( `             rabbitmq.connection.ssl.version            ` )                          | SSL protocol version (e.g., SSL, TLSV1, TLSV1.2)                                                                                      |

Following is a sample configuration for SSL enabled RabbitMQ message
store:

``` xml
    <messageStore xmlns="http://ws.apache.org/ns/synapse"
                  class="org.apache.synapse.message.store.impl.rabbitmq.RabbitMQStore"
                  name="store1">
       <parameter name="store.producer.guaranteed.delivery.enable">false</parameter>
       <parameter name="store.failover.message.store.name">store1</parameter>
       <parameter name="store.rabbitmq.host.name">localhost</parameter>
       <parameter name="store.rabbitmq.queue.name">WithoutClientCertQueue</parameter>
       <parameter name="store.rabbitmq.host.port">5671</parameter>
       <parameter name="rabbitmq.connection.ssl.enabled">true</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.location">path/to/truststore</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.type">JKS</parameter>
       <parameter name="rabbitmq.connection.ssl.truststore.password">truststorepassword</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.location">path/to/keystore</parameter>     
       <parameter name="rabbitmq.connection.ssl.keystore.type">PKCS12</parameter>
       <parameter name="rabbitmq.connection.ssl.keystore.password">keystorepassword</parameter>   
       <parameter name="rabbitmq.connection.ssl.version">SSL</parameter>   
    </messageStore>
```

Configuring parameters that provide information related to keystores
and truststores can be optional based on your broker configuration.

For example, if `         fail_if_no_peer_cert        ` is set to
`         false        ` in the RabbitMQ broker configuration, then you
only need to specify
`         <parameter name="rabbitmq.connection.ssl.enabled" locked="false">true</parameter>        `
. Additionally, you can also set
`         <parameter name="rabbitmq.connection.ssl.version" locked="false">true</parameter>        `
parameter to specify the SSL version. If
`         fail_if_no_peer_cert        ` is set to
`         true        ` , you need to provide keystore and truststore
information.

Following is a sample message store configuration where
`         fail_if_no_peer_cert        ` is set to
`         false        ` :

``` java
    {ssl_options, [{cacertfile,"/path/to/testca/cacert.pem"},
                   {certfile,"/path/to/server/cert.pem"},
                   {keyfile,"/path/to/server/key.pem"},
                   {verify,verify_peer},
                   {fail_if_no_peer_cert,false}]}  
```

  

### Additional RabbitMQ message store parameters

| Parameter Name                                                                     | Value                                                                                                                                                     | Required                                                                                                                                                                   |
|------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RabbitMQ Queue Name ( `             store.rabbitmq.queue.name            ` )       | The message store queue name.                                                                                                                             | Though this is not a required parameter, we recommend specifying a value for this.                                                                                         |
| RabbitMQ Exchange Name ( `             store.rabbitmq.exchange.name            ` ) | The name of the RabbitMQ exchange to which the queue is bound (If the ESB profile has to declare this exchange it will be declared as a direct exchange). | No                                                                                                                                                                         |
| Routing key ( `             store.rabbitmq.route.key            ` )                | The exchange and queue binding value. Messages will be routed using this.                                                                                 | No (This is considered only when both the exchange name and type are provided. Else messages will be routed using the default exchange and queue name as the routing key). |
| User Name ( `             store.rabbitmq.username            ` )                   | The user name to connect to the broker.                                                                                                                   | No                                                                                                                                                                         |
| Password ( `             store.rabbitmq.password            ` )                    | The password to connect to the broker.                                                                                                                    | No                                                                                                                                                                         |
| Virtual Host ( `             store.rabbitmq.virtual.host            ` )            | The virtual host name of the broker.                                                                                                                      | No                                                                                                                                                                         |

If you need to ensure guaranteed delivery when you store incoming
messages to a RabbitMQ message store, and then deliver them to a
particular backend, click **Show Guaranteed Delivery Parameters** and
specify values for the following parameters:

-   **Enable Producer Guaranteed Delivery** (
    `          store.producer.guaranteed.delivery.enable         ` ) -
    Whether it is required to enable guaranteed delivery on the producer
    side.
-   **Failover Message Store** (
    `                     store.failover.message.store.name                   `
    ) - The message store to which the store mediator should send
    messages when the original message store fails.

### Example RabbitMQ message store configurations

Following is a basic sample RabbitMQ message store configuration:

``` xml
    <messageStore class="org.apache.synapse.message.store.impl.rabbitMQ.RabbitMQStore" name="RabbitMS" xmlns="http://ws.apache.org/ns/synapse">
      <parameter name="store.rabbitmq.host.name">localhost</parameter>
      <parameter name="store.rabbitmq.queue.name">EIStore</parameter>
      <parameter name="store.rabbitmq.host.port">5672</parameter>
      <parameter name="store.rabbitmq.username">guest</parameter>
      <parameter name="store.rabbitmq.password">guest</parameter>
      <parameter name="store.rabbitmq.exchange.name">exchange</parameter>
    </messageStore>   
```
