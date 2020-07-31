# Publish unacked messages to Dead Letter Exchange

This sample demonstrates how you can configure a RabbitMQ consumer proxy to move the failed messages into a 
dead letter exchange. When a message is sent to a consumer, if the message fails to deliver, it will be routed to the DLX.

## Synapse configurations

See the instructions on how to [build and run](#build-and-run) this example.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
    name="OrdersService"
    startOnLoad="true"
    statistics="disable"
    trace="disable"
    transports="rabbitmq">
   <target>
    <inSequence>
      <log level="custom">
          <property expression="$body" name="Message Received"/>
      </log>
      <call blocking="true">
          <endpoint>
            <http uri-template="http://localhost:8280/orders"/>
          </endpoint>
      </call>
      <log level="custom">
          <property name="Status" value="The message process successfully"/>
      </log>
    </inSequence>
    <faultSequence>
      <log level="custom">
          <property name="Status" value="Could not process the message"/>
      </log>
      <property name="SET_ROLLBACK_ONLY" scope="axis2" value="true"/>
    </faultSequence>
   </target>
   <parameter name="rabbitmq.queue.autodeclare">false</parameter>
   <parameter name="rabbitmq.exchange.name">orders-exchange</parameter>
   <parameter name="rabbitmq.queue.auto.ack">false</parameter>
   <parameter name="rabbitmq.queue.name">orders</parameter>
   <parameter name="rabbitmq.connection.factory">AMQPConnectionFactory</parameter>
   <description/>
</proxy>
```

## Build and run

1. Make sure you have a RabbitMQ broker instance running.
2. Create an exchange with the name `orders-exchange` to route orders.
    ```bash
    rabbitmqadmin declare exchange --vhost=/ --user=guest --password=guest name=orders-exchange type=direct durable=true
    ```

3. Declare a queue to store orders.
    ```bash
    rabbitmqadmin declare queue --vhost=/ --user=guest --password=guest name=orders durable=true
    ```

4. Bind orders with orders-exchange
    ```bash
    rabbitmqadmin declare binding --vhost=/ --user=guest --password=guest source=orders-exchange destination=orders routing_key=orders
    ```

5. Declare exchange to route orders-error.
    ```bash
    rabbitmqadmin declare exchange --vhost=/ --user=guest --password=guest name=orders-error-exchange type=direct durable=true
    ```

6. Declare queue to store orders-error.
    ```bash
    rabbitmqadmin declare queue --vhost=/ --user=guest --password=guest name=orders-error durable=true
    ```

7. Bind orders-error with orders-error-exchange.
    ```bash
    rabbitmqadmin declare binding --vhost=/ --user=guest --password=guest source=orders-error-exchange destination=orders-error routing_key=orders-error
    ```

8. Set the DLX to orders queue using policy.
    ```bash
    rabbitmqctl set_policy DLX "^orders$" '{"dead-letter-exchange":"orders-error-exchange", "dead-letter-routing-key":"orders-error"}' --apply-to queues
    ```
9. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
10. [Create an integration project](../../../../develop/create-integration-project) with an <b>ESB Configs</b> module and an <b>Composite Exporter</b>.
11. Create the [proxy service](../../../../develop/creating-artifacts/creating-a-proxy-service) with the configurations given above.
12. Enable the RabbitMQ sender and receiver in the Micro-Integrator from the deployment.toml. Refer the 
 [configuring RabbitMQ documentation](../../../setup/brokers/configure-with-rabbitMQ.md) for more information.
13. [Deploy the artifacts](../../../../develop/deploy-artifacts) in your Micro Integrator.
14. Make the `http://localhost:8280/orders` endpoint unavailable temporarily. 
15. Publish a message to the orders queue.
16. You will see that the failed message has been moved to the dead-letter-exchange.
