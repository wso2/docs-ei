# JMS Scenarios

This section provides information on how to improve the performance of
the following most common JMS use cases of WSO2 Enterprise
Integrator(WSO2 EI).

## WSO2 EI as a JMS consumer
In this scenario, WSO2 EI listens to a JMS queue, consumes messages, and
sends the messages to a HTTP back-end service.

You can improve the performance of this scenario by following the steps
below.

-   If the queue gets filled up at a high rate, and the queue is long,
    you can improve the performance by increasing the number of
    concurrent consumers. Add the following parameters to the JMS
    listener configuration of the esb.toml file to increasing the number of concurrent consumers:  

    ``` java
    [mediation]
    transport.jms.ConcurrentConsumers=50
    transport.jms.MaxConcurrentConsumers=50
    ```

-   Add the following parameter to the JMS listener configuration to enable
    JMS listener caching:  

    ```java
    [mediation]
     transport.jms.CacheLevel=consumer
    ```

    The possible values for the cache level are
    `           none          ` , `           auto          ` ,
    `           connection          ` , `           session          `
    and `           consumer          ` . Out of the possible values,
    `           consumer          ` is the highest level that provides
    maximum performance.

## WSO2 EI as a JMS Producer
In this scenario, a proxy service in WSO2 EI accepts messages from an
HTTP client via HTTP and sends the messages to a JMS queue.

You can improve the performance of this scenario by following the steps
given below.

-   Add the following parameter to the JMS sender configuration of the
    esb.toml file to enable JMS sender caching:  

    ``` java
    [mediation]
    transport.jms.CacheLevel=producer
    ```

    The possible values for the cache level are
    `           none          ` , `           auto          ` ,
    `           connection          ` , `           session          `
    and `           producer          ` . Out of the possible values,
    `           producer          ` is the highest level that provides
    maximum performance.

-   Add the following parameter to the configuration to remove
    `           ClientApiNonBlocking          ` when sending messages
    via JMS:

    ```java
    [mediation]
     ClientApiNonBlocking="remove"
    ```

      
    > By default, Axis2 spawns a new thread to handle each outgoing
        message. To change this behavior, you need to remove the
        `           ClientApiNonBlocking          ` property from the
        message. Removal of this property is vital when queuing transports
        like JMS are involved.
    

## WSO2 EI as Both a JMS Producer and Consumer
In this scenario, WSO2 EI listens to a JMS queue and consume messages,
and also sends messages to a JMS queue. To improve the performance of
this scenario, you can follow the steps described in the scenario where
the [EI acts as a JMS consumer](#JMSScenarios-consumer) as well the
steps described in the scenario where the [EI acts as a JMS
producer](#JMSScenarios-producer).