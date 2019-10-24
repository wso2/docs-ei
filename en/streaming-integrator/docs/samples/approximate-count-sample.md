#Calculating the Approximate Count of Events within a Siddhi Window

## Purpose:
This application demonstrates how to calculate the approximate count(frequency) of eventS within a Siddhi window.

!!!info "Before you begin:"
    1. Copy the `<SI_TOOLING_HOME>/samples/artifacts/ApproximateComputingSample/Test-approximate-count.json` file and paste it in the `<SI_TOOLING_HOME>/wso2/server/deployment/simulation-configs` directory.
    2. Copy the `<SI_TOOLING_HOME>/samples/artifacts/ApproximateComputingSample/approximate-count.csv` file and paste it in the `<SI_TOOLING_HOME>/wso2/server/deployment/csv-files` directory.
    3. Save the sample Siddhi application.

        ```sql
            @App:name("approximate-count-sample")
            @APP:description("Demonstrates how to calculate the approximate count(frequency) of eventS within a Siddhi window.")

            define stream transactionStream (userId int, amount double);

            @sink(type='log')
            define stream OutputStream(count long, countLowerBound long, countUpperBound long);

            @info(name = 'query1')
            from transactionStream#window.length(50)#approximate:count(userId, 0.005, 0.9)
            select count, countLowerBound, countUpperBound
            insert into OutputStream;
        ```


## Executing the Sample

To execute the sample, start the `approximate-count-sample` Siddhi application by clicking the **Start** button (shown below) or by clicking by clicking **Run** => **Run**.

![Start button](../../images/amazon-s3-sink-sample/start.png)

If the Siddhi application starts successfully, the following message appears in the console.

`approximate-count-sample.siddhi - Started Successfully!`

## Testing the Sample

To test the sample Siddhi application, simulate multiple events for it from a CSV file via the Streaming Integrator Tooling as follows:

1. To open the Event Simulator, click the **Event Simulator** icon.

   ![Event Simulator Icon](../../images/Testing-Siddhi-Applications/Event_Simulation_Icon.png)

2. In the Event Simulator panel, click **Feed Simulation**. **Test-approximate-count** is available in the **Active Feed Simulations** list as shown below.

3. Click the start button next to the **Test-approximate-count** simulation.
3. Select Test-approximate-count under 'Active Feed Simulations'
4. Click on the start button (Arrow symbol) next to the simulator

## Viewing the Results:

e output is logged i the console as shown in the extract below.
