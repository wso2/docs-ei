# NATS Connector Configuration
[NATS](https://nats.io/) is a distributed messaging platform based on the publish-subscribe messaging model
where publishers publish messages on a paticular subject and consumers listening on that 
subject can consume these messages. For more information, see the [NATS documentation.](https://docs.nats.io/)

The NATS connector allows you to access the NATS API through WSO2 EI and acts as a message 
publisher that facilitates message publishing. 
The NATS connector sends messages to NATS brokers.

# Setting up the environment
__Download and install the connector__

1. Download the connector from the [WSO2 Store](https://store.wso2.com/store/assets/esbconnector/details/3fcaf309-1a69-4edf-870a-882bb76fdaa1) 
by clicking the Download Connector button.

2. You can then follow this [documentation](https://docs.wso2.com/display/EI650/Working+with+Connectors+via+the+Management+Console) to add the connector to your WSO2 EI instance 
and to enable it (via the management console).

3. For more information on using connectors and their operations in your WSO2 EI configurations, see [Using a Connector](https://docs.wso2.com/display/EI650/Using+a+Connector).

4. If you want to work with connectors via WSO2 EI Tooling, see [Working with Connectors via Tooling](https://docs.wso2.com/display/EI650/Working+with+Connectors+via+Tooling).

5. Install NATS server from the [NATS website download page](https://nats.io/download/). Download and install the latest NATS jar file from [this](https://mvnrepository.com/artifact/io.nats/jnats) website 
and paste it in the <EI_HOME>/lib folder.

# NATS parameters

## Core NATS configuration parameters
| Parameter Name | Description | Default Value | Possible Values |
| :---:  | --- | :---: | :---: |
| servers | The format is nats://host1:port1,nats://host2:port2 | nats://localhost:4222 | nats://localhost:4223, nats://localhost:4224 |
| username | Username for authentication. | - | user |
| password | Password for authentication. | - | password | 
| tlsProtocol | The TLS protocol used to generate the SSLContext. | - | TLSv1.2 | 
| tlsKeyStoreType | The format of the key store file. | JKS | JKS |
| tlsKeyStoreLocation | The location of the key store file. | - | /home/user/keystore.jks | 
| tlsKeyStorePassword | The password for the key store file. | - | pass |
| tlsTrustStoreType | The format of the trust store file. | JKS | JKS |
| tlsTrustStoreLocation | The location of the trust store file. | - | /home/user/truststore.jks |
| tlsTrustStorePassword | The password for the trust store file. | - | pass |
| tlsKeyManagerAlgorithm | The TLS algorithm for the key manager factory. | SunX509 | SunX509 |
| tlsTrustManagerAlgorithm | The TLS algorithm for the trust manager factory. | SunX509 | SunX509 |
| bufferSize | Initial size in bytes for connections. | - | 65536 |
| connectionName | Name for the connection. | - | natsconn |
| connectionTimeout | The connection timeout for connection attempts for each server. | - |  2 |
| inboxPrefix | Inbox prefix to manage authorization of subjects. | - | _INBOX. |
| dataPortType | The class to use for the connections data port. | - | - |
| maxControlLine | The maximum length of a control line sent by this connection. | - | 1024 | 
| maxPingsOut | Maximum number of pings the client can have in flight. | - | 2 |
| maxReconnects | Maximum number of reconnect attempts. Use 0 to turn off auto-reconnect and -1 to turn on infinite reconnects. | - | 60 |
| pingInterval | The interval between attempts to ping the server. | - | 2 | 
| reconnectBufferSize | Maximum number of bytes to buffer in the client when trying to reconnect. | - | 8388608 |
| reconnectWait | Time to wait between reconnect attempts to the same server. | - | 2 | 
| requestCleanUpInterval | The interval between cleaning passes on outstanding request futures that are cancelled or timeout in the application code. | - | 5 |
| verbose | Turn on verbose mode with the server. | - | true/false | 
| pedantic | Turn on pedantic mode with the server. | - | true/false |
| supportUTF8Subjects | Enable UTF8 subjects | - | true/false | 
| turnOnAdvancedStats | Enable advanced stats. | - | true/false | 
| traceConnection | Enable connection trace messages. | - | true/false | 
| useOldRequestStyle | Turn on the old request style that uses a new inbox and subscriber for each request. | - | true/false | 
| noRandomize | Turn off server pool randomization. The server goes in the order they were configured or provided by a server in a cluster update. | - | true/false | 
| noEcho | This flag will prevent the server from echoing messages back to the connection if it has subscriptions on the subject being published to. | - | true/false |
| maxPoolSize | Maximum size of the connection pool. | 5 | 10 |

## NATS Streaming configuration parameters
| Parameter Name | Description | Default Value | Possible Values |
| :---:  | --- | :---: | :---: |
| url | The format is nats://host1:port1. | nats://localhost:4222 | nats://localhost:4223 | 
| clientId | Unique ID of the publisher. | - | client123 | 
| clusterId | ID of the cluster to connect to. | test-cluster | cluster123 |
| useCoreNatsConnection | CORE NATS - Configure a Core NATS connection to streaming connection. | - | true/false | 
| natsUrl | CORE NATS - Core NATS URL. | nats://localhost:4222 | nats://localhost:4224 | 
| username | CORE NATS - Username for authentication. | - | user | 
| password | CORE NATS - Password for authentication. | - | password | 
| tlsProtocol | CORE NATS - The TLS protocol used to generate the SSLContext. | - | TLSv1.2 |
| tlsKeyStoreType | CORE NATS - The format of the key store file. | JKS | JKS | 
| tlsKeyStoreLocation | CORE NATS - The location of the key store file. | - | /home/user/keystore.jks | 
| tlsKeyStorePassword | CORE NATS - The password for the key store file. | - | pass | 
| tlsTrustStoreType | CORE NATS - The format of the trust store file. | JKS | JKS | 
| tlsTrustStoreLocation | CORE NATS - The location of the trust store file. | - | /home/user/truststore.jks | 
| tlsTrustStorePassword | CORE NATS - The password for the trust store file. | - | pass | 
| tlsKeyManagerAlgorithm | CORE NATS - The TLS algorithm for the key manager factory. | SunX509 | SunX509 | 
| tlsTrustManagerAlgorithm | CORE NATS - The TLS algorithm for the trust manager factory. | SunX509 | SunX509 | 
| bufferSize | CORE NATS - Initial size in bytes for connections. | - | 65536 | 
| connectionName | CORE NATS - Name for the connection. | - | natsconn |
| connectionTimeout | CORE NATS - The connection timeout for connection attempts for each server. | - | 2 |
| inboxPrefix | CORE NATS - Inbox prefix to manage authorization of subjects. | - | _INBOX. |
| dataPortType | CORE NATS - The class to use for the connections data port. | - | - | 
| maxControlLine | CORE NATS - The maximum length of a control line sent by this connection. | - | 1024 |
| natsMaxPingsOut | CORE NATS - Maximum number of pings the client can have in flight. | - | 2 | 
| maxReconnects | CORE NATS - Maximum number of reconnect attempts. Use 0 to turn off auto-reconnect and -1 to turn on infinite reconnects. | - | 60 |
| natsPingInterval | CORE NATS - The interval between attempts to ping the server. | - | 2 |
| reconnectBufferSize | CORE NATS - Maximum number of bytes to buffer in the client when trying to reconnect. | - | 8388608 |
| reconnectWait | CORE NATS - Time to wait between reconnect attempts to the same server. | - | 2 | 
| requestCleanUpInterval | CORE NATS - The interval between cleaning passes on outstanding request futures that are cancelled or timeout in the application code. | - | 5 |
| verbose | CORE NATS - Turn on verbose mode with the server. | - | true/false |
| pedantic | CORE NATS - Turn on pedantic mode with the server. | - | true/false |
| supportUTF8Subjects | CORE NATS - Enable UTF8 subjects | - | true/false |
| turnOnAdvancedStats | CORE NATS - Enable advanced stats. | - | true/false |
| natsTraceConnection | CORE NATS - Enable connection trace messages. | - | true/false |
| useOldRequestStyle | CORE NATS - Turn on the old request style that uses a new inbox and subscriber for each request. | - | true/false |
| noRandomize | CORE NATS - Turn off server pool randomization. The server goes in the order they were configured or provided by a server in a cluster update. | - | true/false |
| noEcho | CORE NATS - This flag will prevent the server from echoing messages back to the connection if it has subscriptions on the subject being published to. | - | true/false |
| maxPubAcksInFlight | The maximum pub acks the server/client should allow. | - | 16384 |
| pubAckWait | The time to wait for a pub ack. | - | 30 |
| connectWait | The time to wait for establishing a connection. | - | 5 | 
| discoverPrefix | The prefix used in discovery for the cluster. | - | _STAN.discover |
| maxPingsOut | The max pings that can be out to the server before it is considered lost. | - | 2 |
| pingInterval | The time between pings to the server, if it is supported. | - | 2 |
| traceConnection | Enable connection trace messages. | - | true/false | 
| maxPoolSize | Maximum size of the connection pool. | 5 | 10 |



