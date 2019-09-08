# Guaranteed Delivery with Failover

WSO2 Micro Integrator ensures guaranteed delivery with the failover message store and scheduled failover message forwarding processor. The topics in the following section describe how you can setup guaranteed message delivery with failover configurations.

## What you'll build

The following diagram illustrates a scenario where a failover message
store and a scheduled failover message forwarding processor is used
to ensure guaranteed delivery:

![](attachments/119131519/119131522.png)

In this scenario, the original message store fails due to either network
failure, message store crash or system shutdown for maintenance, and the
failover message store is used as the solution for the original message
store failure. So now the store mediator sends messages to the failover
message store. Then, when the original message store is available again,
the messages that were sent to the failover message store need to be
forwarded to the original message store. The **scheduled failover message forwarding processor**
is used for this purpose. The scheduled failover message
forwarding processor is almost the same as the scheduled message
forwarding processor, the only difference is that the scheduled message
forwarding processor forwards messages to a defined endpoint, whereas
the scheduled failover message forwarding processor forwards messages to
the original message store that the message was supposed to be
temporarily stored.

## Let's get started!

This tutorial includes the following sections:

### Step 1: Set up the workspace

To set up the tools:

-   Go to the [product page](https://wso2.com/integration/) of **WSO2 Micro Integrator**, download the product installer and run it to set up the product.
-   Select the relevant [WSO2 Integration Studio](https://wso2.com/integration/tooling/) based on your operating system and extract the
    ZIP file.  The path to this folder is referred to as `MI_TOOLING_HOME` throughout this tutorial.
-   Download the CLI Tool for monitoring artifact deployments.

### Step 2: Develop the integration artifacts

#### Create the failover message store

In this example an in-memory message store is used to create the
failover message store. If you have a cluster setup, it will not be
possible to use an in-memory message store since it is not possible to
share in-memory stores among nodes in a cluster. This step does not
involve any special configuration.

``` 
<messageStore name="failover"/>  
```
![](attachments/119131519/119131525.png)

#### Create the original message store

In this example a JMS message store is used to create the original
message store.  When creating the original message store, you need to
enable guaranteed delivery on the producer side. To do this, set the
following parameters in the message store configuration:

`<parameter name="store.failover.message.store.name">failover</parameter>`  
`<parameter name="store.producer.guaranteed.delivery.enable">true</parameter>`

  
Following is the message store configuration used in this example:

```
<messageStore  
    class="org.apache.synapse.message.store.impl.jms.JmsStore" name="Orginal">  
    <parameter name="store.failover.message.store.name">failover</parameter>  
    <parameter name="store.producer.guaranteed.delivery.enable">true</parameter>  
    <parameter name="java.naming.factory.initial">org.apache.activemq.jndi.ActiveMQInitialContextFactory</parameter>  
    <parameter name="java.naming.provider.url">tcp://localhost:61616</parameter>  
    <parameter name="store.jms.JMSSpecVersion">1.1</parameter>  
</messageStore>
```

![](attachments/119131519/119131524.png)

#### Define the endpoint for the scheduled message forwarding processor

In this example, the SimpleStockquate service is used as the back-end
service. Follow the instructions below to define the address endpoint.

1.  Log in to the Management Console of the ESB profile.
2.  Click **Main** -\> **Service Bus** -\> **Endpoints** -\> **Add
    Endpoint** -\> **Address Endpoint** .
3.  Enter the following details as shown below.

    ``` xml
    <endpoint name="SimpleStockQuoteService">  
      <address uri="http://127.0.0.1:9000/services/SimpleStockQuoteService"/>  
    </endpoint> 
    ```
    ![](attachments/119131519/119131521.png)

4.  Click **Save & Close** .

#### Create a scheduled message forwarding processor to forward messages to the defined endpoint

Following is the scheduled message forwarding processor configuration
used in this example:

```
<messageProcessor name="ForwardMessageProcessor" class="org.apache.synapse.message.processor.impl.forwarder.ScheduledMessageForwardingProcessor" targetEndpoint="SimpleStockQuoteService" messageStore="Orginal" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="interval">1000</parameter>
       <parameter name="client.retry.interval">1000</parameter>
       <parameter name="max.delivery.attempts">4</parameter>
       <parameter name="is.active">true</parameter>
       <parameter name="max.delivery.drop">Disabled</parameter>
       <parameter name="member.count">1</parameter>
</messageProcessor> 
```

![](attachments/119131519/119131520.png)

#### Create a scheduled failover message forwarding processor

When creating the scheduled failover message forwarding processor, you
need to specify the following two mandatory parameters that are
important in the failover scenario.

-   **Source Message Store**
-   **Target Message Store**

The scheduled failover message forwarding processor sends messages from
the failover store to the original store when it is available in the
failover scenario. In this configuration, the source message store
should be the failover message store and target message store should be
the original message store.

Following is the scheduled failover message forwarding processor
configuration used in this example:

```
    <messageProcessor name="FailoverMessageProcessor" class="org.apache.synapse.message.processor.impl.failover.FailoverScheduledMessageForwardingProcessor" messageStore="failover" xmlns="http://ws.apache.org/ns/synapse">
       <parameter name="interval">1000</parameter>
       <parameter name="client.retry.interval">60000</parameter>
       <parameter name="max.delivery.attempts">1000</parameter>
       <parameter name="is.active">true</parameter>
       <parameter name="max.delivery.drop">Disabled</parameter>
       <parameter name="member.count">1</parameter>
       <parameter name="message.target.store.name">Orginal</parameter>
    </messageProcessor> 
```

![](attachments/119131519/119131523.png)

#### Create the proxy service

A proxy service is used to send messages to the original message store via the store mediator. Following is the proxy service used in this example:

``` 
<proxy name="Proxy1" transports="https http" startOnLoad="true" trace="disable" xmlns="http://ws.apache.org/ns/synapse">    
      <target>  
        <inSequence>  
         <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>  
         <property name="OUT_ONLY" value="true"/>  
         <log level="full"/>  
         <store messageStore="Orginal"/>  
        </inSequence>  
      </target>  
</proxy>   
```

### Step 3: Package the artifacts

### Step 4: Build and run the artifacts

...

Send the request to the proxy service

1.  Navigate to the
    `          <EI_HOME>/samples/axis2Server/src/SimpleStockQuoteService         `
    directory, and run `          ant         ` to build and deploy the
    sample service in the Axis2 server.
2.  Navigate to the `           <EI_HOME>/samples/axis2client          `
    directory, and execute the following command to invoke the proxy
    service:

    ``` java
    ant stockquote -Daddurl=http://localhost:8280/services/Proxy1 -Dmode=placeorder  
    ```

    You will see the following message on the Axis2 server console:

    ``` java
    SimpleStockQuoteService :: Accepted order for : 7482 stocks of IBM at $ 169.27205579038733  
    ```

### Step 5: Test the use case

To test the failover scenario, shut down the JMS broker(i.e., the
original message store) and send a few messages to the proxy service.

You will see that the messages are not sent to the back-end since the
original message store is not available. You will also see that the
messages are stored in the failover message store.

If you analyze the the ESB profile log, you will see the failover
message processor trying to forward messages to the original message
store periodically. Once the original message store is available, you
will see that the scheduled failover message forwarding processor sends
the messages to the original store and then that the scheduled message
forwarding processor forwards the messages to the back-end service.
