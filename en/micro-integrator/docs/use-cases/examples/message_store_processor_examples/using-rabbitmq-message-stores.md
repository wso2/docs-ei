# Using the RabbitMQ Message Store
## Synapse configuration
Configuring parameters that provide information related to keystores and truststores can be optional based on your broker configuration.

For example, if `         fail_if_no_peer_cert        ` is set to `         false        ` in the RabbitMQ broker configuration, then you
only need to specify
`         <parameter name="rabbitmq.connection.ssl.enabled" locked="false">true</parameter>        `
. Additionally, you can also set
`         <parameter name="rabbitmq.connection.ssl.version" locked="false">true</parameter>        `
parameter to specify the SSL version. If
`         fail_if_no_peer_cert        ` is set to
`         true        ` , you need to provide keystore and truststore
information.

Following is a sample broker configuration where
`         fail_if_no_peer_cert        ` is set to
`         false        ` :

```xml
{ssl_options, [{cacertfile,"/path/to/testca/cacert.pem"},
               {certfile,"/path/to/server/cert.pem"},
               {keyfile,"/path/to/server/key.pem"},
               {verify,verify_peer},
               {fail_if_no_peer_cert,false}]}   
```