# Aggregating Data Over Time

## Purpose:
This application demonstrates how to simulate random events via Feed Simulation and calculate running aggregates such as `avg`, `min`, `max`, etc. The aggregation is executed on events within a time window. A sliding time window of 10 seconds is used in this sample. For more information on windows see [Siddhi Query Guide - Window](https://wso2.github.io/siddhi/documentation/siddhi-4.0/#window). The `group by` clause helps to perform aggregation on events grouped by a certain attribute. In this sample, the trading information per trader is aggregated and summarized, for a window of 10 seconds.

!!!info "Before you begin:"
    Save the sample Siddhi application.

        ```sql
        @App:name("AggregateOverTime")

        @App:description('Simulate multiple random events and calculate aggregations over time with group by')

        define stream TradesStream(trader string, quantity int);
        @sink(type='log')
        define stream SummarizedTradingInformation(trader string, noOfTrades long, totalTradingQuantity long, minTradingQuantity int, maxTradingQuantity int, avgTradingQuantity double);

        --Find count, sum, min, max and avg of quantity per trader, during the last 10 seconds
        @info(name='query1')
        from TradesStream#window.time(10 sec)
        select trader, count() as noOfTrades, sum(quantity) as totalTradingQuantity, min(quantity) as minTradingQuantity, max(quantity) as maxTradingQuantity, avg(quantity) as avgTradingQuantity
        group by trader
        insert into SummarizedTradingInformation;
        ```


## Executing the Sample:

To execute the sample:

1. Start the Siddhi application by clicking **Run** => **Run**.

2. If the Siddhi application starts successfully, the following message appears in the console.

   `AggregateOverTime.siddhi - Started Successfully!.`

## Testing the Sample:

Configure random event simulation as follows:

1. To open the Event Simulator, click the **Event Simulator** icon.

   ![Event Simulator Icon](../../images/Testing-Siddhi-Applications/Event_Simulation_Icon.png)

2. In the Event Simulator panel, click **Feed Simulation** -> **Create**.

    ![Feed Simulation tab](../../images/aggregate-over-time-sample/feed-simulation-tab.png)

3. In the new panel that opens, enter information as follows:

    ![Random Simulation](../../images/aggregate-over-time-sample/aggregate-over-time-random-simulation.png)

    1. In the **Simulation Name** field, enter `AggregateOverTime` as the name for the simulation.

    2. Select **Random** as the simulation source and then click **Add Simulation Source**.

    3. In the **Siddhi App Name** field, select **AggregateOverTime**.

    4. In the **Stream Name** field, select **Trade Stream**.

    5. In the **trader(STRING)** field, select **Regex based**. Then in the **Pattern** field that appears, enter `(Bob|Jane|Tom)` as the pattern.

        !!!tip
            When you use the `(Bob|Jane|Tom)` pattern, only `Bob`, `Jane`, and `Tom` are selected as values for the `trader` attribute of the `TradeStream`. Using a few values for the `trader` attribute is helpful when you verify the output because the output is grouped by the trader.

    6. In the **quantity(INT)** field, select **Primitive based**.

    7. Save the simulator configuration by clicking **Save**.

    The newly created simulation is now listed under the **Active Feed Simulations** list as shown below.

    ![Newly Created Simulation](../../images/aggregate-over-time-sample/active-feed-simulation-list.png)

4. Click the start button next to the **AggregateOverTime** simulation to start generating random events.

    ![Start](../../images/aggregate-over-time-sample/start.png)

    In the **Run or Debug** dialog box that appears, select **Run** and click **Start Similation**.

    ![Start Simulation](../../images/aggregate-over-time-sample/start-simulation-dialog-box.png)

## Viewing the Results

Once you start the simulator, the output is logged in the console as shown in the sample below. The output reflects the aggregation for the events sent during the last 10 seconds.

![Sample Random Events](../../images/aggregate-over-time-sample/sample-random-events.png)