# JMS Scenarios

This section provides information on how to improve the performance of
the following most common JMS use cases of WSO2 Enterprise
Integrator(WSO2 EI).

### WSO2 EI as a JMS consumer

![](attachments/119130143/119130144.png)

In this scenario, WSO2 EI listens to a JMS queue, consumes messages, and
sends the messages to a HTTP back-end service.

You can improve the performance of this scenario by following the steps
below.

-   If the queue gets filled up at a high rate, and the queue is long,
    you can improve the performance by increasing the number of
    concurrent consumers. Add the following parameters to the JMS
    listener configuration of the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file
    to increasing the number of concurrent consumers:  

    ``` xml
        <parameter name="transport.jms.ConcurrentConsumers" locked="false">50</parameter> 
        <parameter name="transport.jms.MaxConcurrentConsumers" locked="false">50</parameter>
    ```

-   Add the following parameter to the JMS listener configuration of the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file to enable
    JMS listener caching:  

    ``` xml
            <parameter name="transport.jms.CacheLevel">consumer</parameter>
    ```

    The possible values for the cache level are
    `           none          ` , `           auto          ` ,
    `           connection          ` , `           session          `
    and `           consumer          ` . Out of the possible values,
    `           consumer          ` is the highest level that provides
    maximum performance.

### WSO2 EI as a JMS Producer

![](attachments/119130143/119130146.png)

In this scenario, a proxy service in WSO2 EI accepts messages from an
HTTP client via HTTP and sends the messages to a JMS queue.

You can improve the performance of this scenario by following the steps
given below.

-   Add the following parameter to the JMS sender configuration of the
    `           <EI_HOME>/conf/axis2/axis2.xml          ` file to enable
    JMS sender caching:  

    ``` xml
            <parameter name="transport.jms.CacheLevel">producer</parameter>
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

    ``` xml
            <property name="ClientApiNonBlocking" action="remove" scope="axis2"/>
    ```

      
        !!! info
    
        Noe
    
        By default, Axis2 spawns a new thread to handle each outgoing
        message. To change this behavior, you need to remove the
        `           ClientApiNonBlocking          ` property from the
        message. Removal of this property is vital when queuing transports
        like JMS are involved.
    

### WSO2 EI as Both a JMS Producer and Consumer

![](attachments/119130143/119130145.png)

In this scenario, WSO2 EI listens to a JMS queue and consume messages,
and also sends messages to a JMS queue. To improve the performance of
this scenario, you can follow the steps described in the scenario where
the [EI acts as a JMS consumer](#JMSScenarios-consumer) as well the
steps described in the scenario where the [EI acts as a JMS
producer](#JMSScenarios-producer) .
