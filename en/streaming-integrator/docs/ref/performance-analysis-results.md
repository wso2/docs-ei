# Performance Analysis Results

!!!note
    These performance statistics were taken when the load average was below 3.8 in the 4 core instance.

## Consume events using Kafka source

### Specifications of EC2 Instances

- Stream Processor : c5.xLarge
- Kafka server : c5.xLarge
- Kafka publisher : c5.xLarge

### Siddhi Application

```sql
@App:name("HelloKafka")

@App:description('Consume events from a Kafka Topic and publish to a different Kafka Topic')

@source(type='kafka',
    	topic.list='kafka_topic',
    	partition.no.list='0',
    	threading.option='single.thread',
    	group.id="group",
    	bootstrap.servers='172.31.0.135:9092',
    	@map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='log')
define stream KafkaSourceThroughputStream(count long);

from SweetProductionStream#window.timeBatch(5 sec)
select count(*)/5 as count
insert into KafkaSourceThroughputStream;
```

### Results

- Average Publishing TPS to Kafka : 1.1M
- Average Consuming TPS from Kafka: 180K


## Consuming messages from an HTTP Source

### Specifications of EC2 Instances

- Stream Processor : c5.xLarge
- JMeter: c5.xLarge

### Siddhi Application

```sql
@App:name("HttpSource")

@App:description('Consume events from http clients')

@source(type='http', worker.count='20', receiver.url='http://172.31.2.99:8081/service',
@map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='log')
define stream HttpSourceThroughputStream(count long);

from SweetProductionStream#window.timeBatch(5 sec)
select count(*)/5 as count
insert into HttpSourceThroughputStream;
```

### Results

- Average Publishing TPS to Http Source : 30K
- Average Consuming TPS from Http Source: 30K


## Sending HTTP requests and consuming the responses

### Specifications of EC2 Instances

- Stream Processor : c5.xLarge
- JMeter: c5.xLarge
- Web server : c5.xLarge

### Siddhi Application

```sql
@App:name("HttpRequestResponse")

@App:description('Consume events from an HTTP source, ')

@source(type='http', worker.count='20', receiver.url='http://172.31.2.99:8081/service',
@map(type='json'))
define stream SweetProductionStream (name string, amount double);

@sink(type='http-request', l, sink.id='production-request', publisher.url='http://172.17.0.1:8688//netty_echo_server', @map(type='json'))
define stream HttpRequestStream (batchNumber double, lowTotal double);

@source(type='http-response' , sink.id='production-request', http.status.code='200',
@map(type='json'))
define stream HttpResponseStream(batchNumber double, lowTotal double);

@sink(type='log')
define stream FinalThroughputStream(count long);

@sink(type='log')
define stream InputThroughputStream(count long);

from SweetProductionStream
select 1D as batchNumber, 1200D as lowTotal
insert into HttpRequestStream;

from SweetProductionStream#window.timeBatch(5 sec)
select count(*)/5 as count
insert into InputThroughputStream;

from HttpResponseStream#window.timeBatch(5 sec)
select count(*)/5 as count
insert into FinalThroughputStream;
```

### Results

- Average Publishing TPS to HTTP Source : 29K
- Average Publishing TPS from HTTP request sink: 29K
- Average Consuming TPS from HTTP response source: 29K











