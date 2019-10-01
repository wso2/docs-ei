*
## Purpose:
This app demonstrates how to publish aggregated events to Amazon AWS S3 bucket using siddhi-io-s3 sink
extension. In this sample the events received by the StockQuoteStream will be aggregated by the
StockQuoteWindow and they will be published to Amazon S3 bucket specified in bucket.name parameter.

## Prerequisites:
1. Create a AWS account and set the credentials as mentioned in
## https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html.

## Executing the Sample:
1. Set the credential.provider class name. If the class is not given, default credential provider chain
will be used.
2. Set the bucket name for the bucket.name parameter.
3. Run the AmazonS3SinkSample.siddhi file.
4. In the event simulator select "AmazonS3SinkSample" as the Siddhi App Name and "StockQuoteStream" from the Stream
Name.
5. Send multiple events.

## Testing the Sample:
Once the events are sent, check the S3 bucket. Objects will be created with 3 events each.

```sql
@App:name("AmazonS3SinkSample")
@App:description("Publish events to Amazon AWS S3")


define window StockQuoteWindow(symbol string, price double, quantity int) lengthBatch(3) output all events;

define stream StockQuoteStream(symbol string, price double, quantity int);

@sink(type='s3', bucket.name='<BUCKET_NAME>', object.path='stocks',
      credential.provider='com.amazonaws.auth.profile.ProfileCredentialsProvider', node.id='zeus',
    @map(type='json', enclosing.element='$.stocks',
        @payload("""{"symbol": "{{symbol}}", "price": "{{price}}", "quantity": "{{quantity}}"}""")))
define stream StorageOutputStream (symbol string, price double, quantity int);

from StockQuoteStream
insert into StockQuoteWindow;

from StockQuoteWindow
select *
insert into StorageOutputStream;
```