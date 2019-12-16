# Configuring Kafka

!!! Warning
    **Please note that the contents on this page are under review!**

In order to use the kafka inbound endpoint, you need to download and install [Apache Kafka](http://kafka.apache.org/downloads.html). The recommended version is `kafka_2.9.2-0.8.1.1`.

To configure the kafka inbound endpoint, copy the following client libraries from the `KAFKA_HOME/libs` directory to the `MI_HOME/lib` directory.
      -   `kafka_2.9.2-0.8.1.1.jar`
      -   `scala-library-2.9.2.jar`
      -   `zkclient-0.3.jar`
      -   `zookeeper-3.3.4.jar`
      -   `metrics-core-2.2.0.jar`
      
!!! Note
    -  If you are using `kafka_2.x.x-0.8.2.0` or later, you also need to add the `kafka-clients-0.8.x.x.jar` file to the `MI_HOME/lib` directory.
    -  If you are using a newer version of ZooKeeper, follow the steps below:

        1. Create a directory named `conf` inside the `MI_HOME/repository` directory.
        2. Create a directory named identity inside the `MI_HOME/repository/conf` directory.
        3. Add the `jaas.conf` file to the `MI_HOME/repository/conf/identity` directory. This is required because Kerberos authentication is enforced on newer versions of ZooKeeper.

To start the Kafka server:

-   Navigate to `<KAFKA_HOME>` and run the
    following command to start the ZooKeeper server:

    ```bash
    bin/zookeeper-server-start.sh config/zookeeper.properties
    ```

-   Run the following command to start the Kafka server

    ```bash
    bin/kafka-server-start.sh config/server.properties
    ```