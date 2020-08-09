# Performance Analysis Results

!!!note
    These performance statistics were taken when the load average was below 3.8 in the 4 core instance.

## Consuming events using Kafka source

### Specifications of EC2 Instances

- Stream Processor : c5.xLarge
- Kafka server     : c5.xLarge
- Kafka publisher  : c5.xLarge

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
- JMeter           : c5.xLarge

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
- JMeter           : c5.xLarge
- Web server       : c5.xLarge

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

- Average Publishing TPS to HTTP Source          : 29K
- Average Publishing TPS from HTTP request sink  : 29K
- Average Consuming TPS from HTTP response source: 29K

## Performing ETL tasks

### Specifications of EC2 Instances

- Stream Processor : m4.xlarge
- JMeter           : m4.xlarge
- Web server       : m4.xlarge


### Siddhi Application

This scenario was tested using two Siddhi applications that execute the process explained below.

![ETL Process](../images/performance-analysis-results/production-factory-etl.png)

The two Siddhi applications are as follows:

**ETLFIleRecordsCopier.siddhi**

```
@App:name('ETLFileRecordsCopier')
@App:description('This sample demonstrates on integrating a File in a particular location with a Database.')

@source(type='file', mode='LINE',
    dir.uri='file:/Users/wso2/demo/accurate-files',
    action.after.process='MOVE',
    move.after.process='file:/Users/wso2/demo/moved',
    tailing='false',
    header.present='true',
    @map(
        type='csv',
        delimiter='|',
        @attributes(code = '0', serialNo = '1', amount = '2', fileName = 'trp:file.path', eof = 'trp:eof')))
define stream FileReaderStream (code string, serialNo string, amount double, fileName string, eof string); -- Reads from file

@Store(type="rdbms",
      jdbc.url="jdbc:mysql://localhost:3306/batchInformation?useSSL=false",
      username="root",
      password="root" ,
      jdbc.driver.name="com.mysql.jdbc.Driver",
      isAutoCommit = 'true')
define table AccurateBatchTable(serialNo string, amount double, fileName string, status string, timestamp long);

@sink(type='log', prefix='File to DB copying has Started: ')
define stream FileReadingStartStream(fileName string);

@sink(type='log', prefix='File to DB copying has Finished: ')
define stream FileReadingEndStream(fileName string);


from FileReaderStream
select serialNo, amount, fileName, "test" as status, eventTimestamp() as timestamp, count() as rowNumber, eof
insert into DataStream;

from DataStream
select *
insert into DataStreamPassthrough;

-- Write to DB Passthrough
from DataStreamPassthrough#window.externalTimeBatch(timestamp, 5 sec, timestamp, 10 sec)
select serialNo, amount, fileName, status, timestamp, rowNumber, eof
insert into TemporaryTablePassthroughStream;

-- Log First Record
from TemporaryTablePassthroughStream[rowNumber == 1]
select fileName
insert into FileReadingStartStream;

-- Log Every 100000th Record
from TemporaryTablePassthroughStream
select fileName, rowNumber as rows
insert into FileReadingInProgressStream;

-- Log Last Record
from TemporaryTablePassthroughStream[eof == 'true']
select fileName
insert into FileReadingEndStream;

-- Write to DB
from TemporaryTablePassthroughStream#window.batch()
select serialNo, amount, fileName, status, timestamp
insert into AccurateBatchTable;
```

**ETLFileAnalyzer.siddhi**

```
@App:name('ETLFileAnalyzer')
@App:description('This sample demonstrates on moving files to a specific location comparing its content with the header values.')

@source(type='file', mode='REGEX',
    dir.uri='file:/Users/wso2/demo/new',
    action.after.process='MOVE',
    move.after.process='file:/Users/wso2/demo/header-processed',
    tailing='false',
    @map(
        type='text',
        fail.on.missing.attribute = 'false',
        regex.A='HDprod-[a-zA-z]*-[0-9][0-9][0-9][0-9]-[0-9][0-9]-[0-9][0-9]-([0-9]+)',
        @attributes(
            expectedRowCount = 'A[1]',
            fileName = 'trp:file.path')))
define stream HeaderReaderStream (fileName string, expectedRowCount long);

@source(type='file', mode='LINE',
    dir.uri='file:/Users/wso2/demo/header-processed',
    tailing='false',
    header.present='true',
    @map(
        type='csv',
        delimiter='|',
        @attributes(code = '0', serialNo = '1', amount = '2', fileName = 'trp:file.path', eof = 'trp:eof')))
define stream FileReaderStream (code string, serialNo string, amount double, fileName string, eof string);

@sink(type='log', prefix='Accurate Batch: ')
define stream AccurateFileNotificationStream (fromPath string);

@sink(type='log', prefix='Inaccurate Batch: ')
define stream InaccurateFileNotificationStream (fromPath string);

@sink(type='log', prefix='Batch checking started: ')
define stream ExpectedRowCountsStream (fileName string, expectedRowCount long);

define stream AnalyzingLogStream (fileName string, rowCount long);

define table ExpectedRowCountsTable (fileName string, expectedRowCount long, existingRowCount long);

@sink(type='log', prefix='Batch checking finished: ')
define stream ExistingRowCountsStream (fileName string, existingRowCount long);

-- Expected Row Count reader. Moves file from 'new' to 'header-processed'
from HeaderReaderStream[NOT(expectedRowCount is null) and NOT(fileName is null)]
select *
insert into ExpectedRowCountsStream;

from ExpectedRowCountsStream
select fileName, expectedRowCount, -1L as existingRowCount
insert into ExpectedRowCountsTable;

-- Existing Row Count calculator. Moves file from 'header-processed' to 'rows-counted'
from FileReaderStream
select *
insert into FileDataStream;

partition with (fileName of FileDataStream)
begin
    from FileDataStream
    select fileName, count() as rowCount, eof
    insert into #ThisFileRowCounts;
    
    from #ThisFileRowCounts
    select fileName, rowCount
    insert into AnalyzingLogStream;

    from #ThisFileRowCounts[eof == 'true']
    select fileName, rowCount as existingRowCount
    insert into ExistingRowCountsStream;
end;

-- Existing vs. Expected Row Counts comparer
from ExistingRowCountsStream as S inner join ExpectedRowCountsTable as T on str:replaceFirst(S.fileName, 'header-processed', 'new') == T.fileName
select S.fileName as fromPath, T.expectedRowCount as expectedRowCount, S.existingRowCount as existingRowCount
insert into FileInfoMatcherStream;

from FileInfoMatcherStream
select fromPath, existingRowCount
update ExpectedRowCountsTable
    set ExpectedRowCountsTable.existingRowCount = existingRowCount
    on ExpectedRowCountsTable.fileName == fromPath;
    
-- Accurate file mover
from FileInfoMatcherStream[expectedRowCount == existingRowCount]
select fromPath
insert into AccurateFileStream;

from AccurateFileStream#file:move(fromPath, '/Users/wso2/demo/accurate-files/')
select fromPath
insert into AccurateFileNotificationStream;

-- Inaccurate batch file mover
from FileInfoMatcherStream[expectedRowCount != existingRowCount]
select fromPath
insert into InaccurateFileStream;

from InaccurateFileStream#file:move(fromPath, '/Users/wso2/demo/inaccurate-files/')
select fromPath
insert into InaccurateFileNotificationStream;
```
For a detailed description of this scenario, see the [Streaming ETL with WSO2 Streaming Integrator article](https://wso2.com/articles/streaming-etl-with-wso2-streaming-integrator/)

### Results

The performance statistics of this scenario are as follows:

- Lines     : 6,140,031
- Size      : 124MB
- Database  : AWS RDS instance with oracle-ee 12.1.0.2.v15
- Duration  : 1.422 minutes (85373ms)


- Average Publishing TPS to HTTP Source : 29K
- Average Publishing TPS from HTTP request sink: 29K
- Average Consuming TPS from HTTP response source: 29K

