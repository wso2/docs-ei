# Tuning the RabbitMQ Transport

### Reuse the connection factory in the publisher

In the publisher url, set the connection factory name instead of the
connection parameters as specified below in the
`         rabbitmq.connection.factory        ` parameter . This reuses
the connection factories and thereby improves performance.

``` xml
    <address uri="rabbitmq://?rabbitmq.connection.factory=RabbitMQConnectionFactory&amp;rabbitmq.queue.name=queue1&amp;rabbitmq.queue.routing.key=queue1&amp;rabbitmq.replyto.name=replyqueue&amp;rabbitmq.exchange.name=ex1&amp;rabbitmq.queue.autodeclare=false&amp;rabbitmq.exchange.autodeclare=false&amp;rabbitmq.replyto.name=response_queue"/>
```
