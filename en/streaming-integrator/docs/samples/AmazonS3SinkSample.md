# Publishing Aggregated Events to the Amazon AWS S3 Bucket

## Purpose:

This application demonstrates how to publish aggregated events to Amazon AWS S3 bucket via the [siddhi-io-s3 sink
extension](https://siddhi-io.github.io/siddhi-io-s3/). In this sample the events received to the `StockQuoteStream` stream are aggregated by the
`StockQuoteWindow` window, and then published to the Amazon S3 bucket specified via the `bucket.name` parameter.

!!!info "Before you begin:"
    - Create an AWS account and set the credentials following the instructions in the [AWS Developer Guide](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html).<br/>
    - Save the sample Siddhi application.<br/>

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



## Executing the Sample:

To execute the sample, follow the procedure below:

1. Update the sample Siddhi application as follows:

    1. Enter the credential.provider class name as the value for the `credential.provider` parameter. If the class is not specified, the default credential provider chain is used.

    2. For the `bucket.name` parameter, enter `AWSBUCKET` as the value.

    3. Save the Siddhi application, and then start it by clicking the **Start** button (shown below) or by clicking by clicking **Run** => **Run**.

        ![Start button](../../images/amazon-s3-sink-sample/start.png)

2. To open the Event Simulator, click the **Event Simulator** icon.

       ![Event Simulator Icon](../../images/Testing-Siddhi-Applications/Event_Simulation_Icon.png)

3. In the Event Simulator panel, click **Feed Simulation** -> **Create**.

        ![Feed Simulation tab](../../images/aggregate-over-time-sample/feed-simulation-tab.png)

4. In the new panel that opens, enter information as follows.

    ![Feed Simulation](../../images/amazon-s3-sink-sample/AmazonS3SinkSample-feed-simulation.png)

    1. In the **Simulation Name** field, enter `AmazonS3SinkSample` as the name for the simulation.

    2. Select **Random** as the simulation source and then click **Add Simulation Source**.

    3. In the **Siddhi App Name** field, select **AmazonS3SinkSample**.

    4. In the **Stream Name** field, select **StockQuoteStream**.

    5. In the **symbol(STRING)** field, select **Regex based**. Then in the **Pattern** field that appears, enter `(IBM|WSO2|Dell)` as the pattern.

        !!!tip
            When you use the `(IBM|WSO2|Dell)` pattern, only `IBM`, `WSO2`, and `Dell` are selected as values for the `symbol` attribute of the `StockQuoteStream`. Using a few values for the `symbol` attribute is helpful when you verify the output because the output is grouped by the symbol.

    6. In the **price(DOUBLE)** field, select **Static value**.

    7. In the **quantity(INT)** field, select **Primitive based**.

    8. Save the simulator configuration by clicking **Save**. Th.e simulation is added to the list of saved feed simulations as shown below.

        ![saved simulation](../../images/amazon-s3-sink-sample/simulation-list.png)

5. To simulate random events, click the **Start** button next to the **AmazonS3SinkSample** simulator.


## Testing the Sample

Once the events are sent, check the S3 bucket. Objects are created with 3 events in each.

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