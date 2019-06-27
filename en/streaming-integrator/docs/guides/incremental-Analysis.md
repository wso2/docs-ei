# Incremental Analysis

Incremental aggregation allows you to obtain aggregates in an
incremental manner for a specified set of time periods.

This not only allows you to calculate aggregations with varied time
granularity, but also allows you to access them in an interactive manner
for reports, dashboards, and for further processing. Its schema is
defined via the aggregation definition.

-   [Configuring aggregation
    queries](#IncrementalAnalysis-Configuringaggregationqueries)
-   [Incremental aggregation in single-node deployments and HA
    deployments](#IncrementalAnalysis-Incrementalaggregationinsingle-nodedeploymentsandHAdeployments)
-   [Scaling through distributed
    aggregations](#IncrementalAnalysis-ScalingthroughdistributedaggregationsPartitionAggregation)

## Configuring aggregation queries

This section explains how to calculate aggregates in an incremental
manner for different sets of time periods, store the calculated values
in a data store and then retrieve that information.

To demonstrate this, consider a scenario where a business that sells
multiple brands stores its sales data in a physicaldatabase for the
purpose of retrieving them later to perform sales analysis. Each sales
transaction is received with the following details:

-   `          symbol         ` : The symbol that represents the brand
    of the items sold.
-   `          price         ` : the price at which each item was sold.
-   `          amount         ` : The number of items sold.

The Sales Analyst needs to retrieve the total number of items sold of
each brand per month, per week, per day etc., and then retrieve these
totals for specific time durations to prepare sales analysis reports.

To address the above use case, Siddhi queries need to be designed in two
steps as follows:

-   [Step 1: Calculate and persist time-based aggregate
    values](#IncrementalAnalysis-Step1:Calculateandpersisttime-basedaggregatevalues)
-   [Step 2: Retrieve calculated and persisted aggregate
    values](#IncrementalAnalysis-Step2:RetrievecalculatedandpersistedaggregatevaluesRetrieveAggregations)

### Step 1: Calculate and persist time-based aggregate values

!!! tip

Before you begin:

Before creating queries to calculate and persist aggregate values, the
following prerequisites need to be completed:

-   When you are defining a physical store to store aggregate values,
    you need to first download, install and set up the required database
    type. For more information about setting up the database, see
    [Defining Tables for Physical
    Stores](_Defining_Tables_for_Physical_Stores_) .
-   If the aggregation query included in the Siddhi application is only
    for read-only purposes, disable data purging.


To write the required Siddhi queries to calculate and persist aggregate
values required for the scenario outlined above, follow the steps below:

1.  To capture the input events based on which the aggregations are
    calculated, define an input stream as follows. The stream definition
    includes attributes named symbol, price and amount to capture the
    details described above.

    `             define stream TradeStream (symbol string, price double, quantity long, timestamp long);            `

    The stream definition includes attributes named
    `           symbol          ` , `           price          ` and
    `           amount          ` to capture the details described
    above. In addition, it has an attribute named
    `           timestamp          ` to capture the time at which the
    sales transaction occurs. The aggregations are executed based on
    this time.

        !!! info
    
        This attribute's value could either be a long value (reflecting the
        Unix timestamp in milliseconds), or a string value adhering to one
        of the following formats.
    
        -   **`             <yyyy>-<MM>-<dd> <HH>:<mm>:<ss> <Z>            `**
            : This format can be used if the timezone needs to be specified
            explicitly. Here the ISO 8601 UTC offset must be provided for
            \<Z\> . e.g., +05:30 reflects the India Time Zone. If time is
            not in GMT, this value must be provided.)
        -   **`             <yyyy>-<MM>-<dd> <HH>:<mm>:<ss>            `** :
            This format can be used if the timezone is in GMT.
    

2.  To persist the aggregates that are calculated via your Siddhi
    application, include a store definition as follows, if not the data
    is stored in-memory and lost when siddhi app is stopped.

    `             define stream TradeStream (symbol string, price double, quantity long, timestamp long);            `

    **`              @store(type='rdbms', jdbc.url="jdbc:                             mysql://localhost:3306/TestDB                            ", username="root", password="root" ,                             jdbc.driver.name                            ="com.mysql.jdbc.Driver")             `**

3.  Define an aggregation as follows. You can name it
    `           TradeAggregation          ` .

        !!! info
    
        When you save aggregate values in a store, the system uses the
        aggregation name you define here as part of the database table name.
        Table name is
        `           <Aggregation_Name>_<Granularity>          ` . Some
        database types have length limitations for table names (e.g., for
        Oracle, it is 30 characters). Therefore, you need to check whether
        the database type you have used for the store has such table name
        length limitation, and make sure that the aggregation name does not
        exceed that maximum length.
    

        define aggregation TradeAggregation

4.  To calculate aggregations, include a query as follows:
    1.  To get input `             events            ` from the
        `             TradeStream            ` stream that you
        previously defined, add a `             from            ` clause
        as follows.

        `               define aggregation TradeAggregation              `

        **`                 from TradeStream                `**

    2.  To select attributes to be included in the output event, add a
        `             select            ` clause as follows.

        `               define aggregation TradeAggregation              `

        `                from TradeStream               `  
        **`                 select symbol, avg(price) as avgPrice, sum(quantity) as total                `**

        Based on this, each output event has the following attributes.

        | Attribute                                   | Description                                                                                                                                                                                                      |
        |---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
        | `                 symbol                `   | The symbol representing the product sold is included in each output event.                                                                                                                                       |
        | `                 avgPrice                ` | The average price at which the brand is sold. This is derived by applying the `                 avg()                ` function to the `                 price                ` attribute with each input event. |
        | `                 total                `    | The total number of items sold for each product. This is derived by applying the `                 sum()                ` function to the quantity attribute with each input event.                              |

        The average price and the total number of items are the
        aggregates calculated in this scenario.

    3.  To group the output by the symbol, add a
        `             group by            ` clause as follows.

        `               define aggregation TradeAggregation              `

        `                from TradeStream               `  
        `                select symbol, avg(price) as avgPrice, sum(quantity) as total               `

        **`                group by symbol               `**

        The group by clause is optional in incremental aggregations. If
        it is not provided, all the events are aggregated together for
        each duration.

    4.  The timestamp included in each input event allows you to
        calculate aggregates for the range of time granularities
        seconds-years. Therefore, to calculate aggregates for each time
        granularity within this range, add the
        `             aggregate by            ` clause to this aggregate
        query as follows.

        `               define aggregation TradeAggregation              `

        `                from TradeStream               `  
        `                select symbol, avg(price) as avgPrice, sum(quantity) as total               `

        `               group by symbol              `

        **`                aggregate by timestamp every sec ... year;               `**

          

        This results in the average price and the total number of items
        sold being calculated per second, minute, hour, day, month and
        year. These time-based calculations are saved in the MySQL store
        you defined.

                !!! warning
        
                If you exclude one or more time granularities from this range,
                you cannot later retrieve aggregates for them. e.g., You cannot
                aggregate by timestamp every
                `             sec ... day            ` in the aggregate
                definition, and then attempt to retrieve per
                `             months            ` . This is because monthly
                values are not calculated and persisted.
        
                !!! info
        
                You can also provide comma-separated values such as
                `             every day, month,            ` ...
        

        The complete Siddhi application with all the definitions and
        queries added looks as follows.

        ``` sql
                @App:name('TradeApp')
        
                define stream TradeStream (symbol string, price double, quantity long, timestamp long);
        
                @store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/TestDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")
        
                define aggregation TradeAggregation
                from TradeStream
                select symbol, avg(price) as avgPrice, sum(quantity) as total
                group by symbol
                aggregate by timestamp every sec ... year;
        ```

The following is a list of optional annotations supported for
incremental aggregation.

-   **`            @store(type=<store type>, ...)           `**  
    The aggregated results are stored in the database defined via this
    annotation in tables corresponding to each aggregate duration. For
    more information on how to define a store, see [Defining Tables for
    Physical Stores](_Defining_Tables_for_Physical_Stores_) .

        !!! info
    
        When these database entries are created, the primary key is defined
        internally. Therefore, you must not include an
        `           @PrimaryKey          ` annotation within this
        `           @store          ` annotation in the aggregation
        definition.
    

    If a store is not defined via this annotation, the aggregated
    results are stored in in-memory tables by default. Adding a specific
    store definition is useful in a production environment. This is
    because the aggregations stored in in-memory tables can be lost if a
    system failure occurs.  
      
    The names of the tables are created in the
    `           <AGGREGATION_NAME>_<DURATION>          ` format. In this
    scenario, the following tables are created:

    -   `             TradeAggregation_SECONDS            `

    -   `             TradeAggregation_MINUTES            `

    -   `             TradeAggregation_HOURS            `

    -   `             TradeAggregation_DAYS            `

    -   `             TradeAggregation_MONTHS            `

    -   `             TradeAggregation_YEARS            `

    The primary keys of these tables are determined as follows:  
    ![](attachments/112390719/112390725.png){width="500"}

    `           AGG_TIMESTAMP          ` and
    `           AGG_EVENT_TIMESTAMP          ` are internally calculated
    values that reflect the time bucket of an aggregation for a
    particular duration. e.g. for the `           day          `
    aggregations, `           AGG_TIMESTAMP = 1515110400000          `
    reflects that it is the aggregation for the 5th of January 2018 (
    `           1515110400000          ` is the Unix time for
    `           2018-01-05 00:00:00          ` ). All aggregations are
    based on GMT timezone.

      
    The other values stored in the table would be aggregations and other
    function calculations done in the aggregate definition. If any such
    function calculation is not an aggregation, the output value
    corresponds with the function calculation for the latest event of
    that time bucket. e.g., If a multiplication is carried out, the
    multiplication value corresponds with the latest event's
    multiplication as per the duration.  
      
    Certain aggregations are internally stored as a collection of other
    aggregations. e.g., the average aggregation is internally calculated
    as a function of sum and count. Hence, the table only reflects a sum
    and a count. The actual average is returned when the user retrieves
    the aggregate values as described in [Step 2: Retrieve calculated
    and persisted aggregate
    values](#IncrementalAnalysis-RetrieveAggregations) .

    The other values stored in the database table are aggregations and
    other function calculations carried out via the aggregation
    definition.

    In the scenario described in this section, a timestamp is assigned
    to each input event via the `           timestamp          `
    attribute, and `           symbol          ` is the
    `           group by          ` key. Therefore, the
    `           TestDB          ` store defined via the
    `           @store          ` annotation uses the
    `           AGG_EVENT_TIMESTAMP          ` and
    `           symbol          ` attributes as primary keys. Each
    record . in this store must have a unique combination of values for
    these two attributes.

-   **`           @purge          `**  

    This specifies whether automatic data purging is enabled or not. If
    automatic data purging is enabled, you need to specify the time
    interval at which the data purging should be carried out. In
    addition, you need to specify the time period for which the data
    should be retained based on the granularity by including the
    `           @retentionPeriod          ` annotation (described
    below). If this annotation is not included in an incremental
    aggregation query, the data purging is carried out every 15 minutes
    based on the default retention periods mentioned in the description
    of the `           @retentionPeriod          ` annotation.

        !!! tip
    
        If you want to disable automatic data purging, you can use this
        annotation as follows:
    
        `           @purge(enable="false")          `
    
        You should disable data purging if the aggregation query is included
        in the Siddhi application for read-only purposes.
    

    <table>
    <colgroup>
    <col style="width: 100%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td><table>
    <caption> </caption>
    <tbody>
    <tr class="odd">
    <td><img src="attachments/112390719/112390721.png" class="gliffy-macro-image" /></td>
    </tr>
    </tbody>
    </table></td>
    </tr>
    </tbody>
    </table>

      

-   **`           @retentionPeriod          `**  

    This specifies the time period for which data needs to be retained
    before it is purged, based on the granularity. The retention period
    defined for each granularity should be greater than, or equal to the
    minimum retention period as given below:

    -   `            second           ` : 120 seconds
    -   `            minute           ` : 120 minutes
    -   `            hour           ` : 25 hours
    -   `            day           ` : 32 days
    -   `            month           ` : 13 months
    -   `            year           ` : none

    If the retention period is not specified, the default retention
    period for each granularity is applied as given below:

    -   `            second           ` : 120 seconds
    -   `            minute           ` : 24 hours
    -   `            hour           ` : 30 days
    -   `            day           ` : 1 year
    -   `            month           ` : All
    -   `            year           ` : All

  

### Step 2: Retrieve calculated and persisted aggregate values

This step involves retrieving the aggregate values that you calculated
and persisted in Step 1. To do this, let's add the Siddhi definitions
and queries required for retrieval to the `         TradeApp        `
Siddhi application that you have already started creating for this
scenario.

!!! info

Retrieval logic for the same aggregation can be defined in different
Siddhi app. However, only one aggregation should carry out the
processing (i.e. the aggregation input stream should only feed events to
one aggregation definition).


For this scenario, let's assume that the Sales Analyst needs to retrieve
the sales totals for the time duration between 15th February 2014 and
16th March 2014.

1.  To retrieve aggregations, you need to make retrieval requests. To
    capture these requests as events, let's define a stream as follows.

    `             @App:name('TradeApp')                          define stream TradeStream (symbol string, price double, quantity long, timestamp long);                         `

    **`              define stream TradeSummaryRetrievalStream (symbol string);             `**

    `             @store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/TestDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")                          define aggregation TradeAggregation                          from TradeStream select symbol, avg(price) as avgPrice, sum(quantity) as total group by symbol aggregate by timestamp every sec ... year;            `

2.  To process the events captured via the
    `           TradeSummaryRetrievalStream          ` stream you
    defined, add a new query as follows.

        from TradeSummaryRetrievalStream as b join TradeAggregation as a
        on a.symbol == b.symbol 
        within "2014-02-15 00:00:00 +05:30", "2014-03-16 00:00:00 +05:30" 
        per "days" 
        select a.symbol, a.total, a.avgPrice 
        insert into TradeSummaryStream;

    This query matches events in the
    `           TradeSummaryRetrievalStream          ` stream and the
    `           TradeAggregation          ` aggregation that was defined
    in step 1 based on the value for the `           symbol          `
    attribute, and performs a join for each matching pair. Based on that
    join, it produces an output event(s) with the
    `           symbo          ` l, `           total          ` and
    `           avgPrice          ` attributes for the day granularity
    within the time range `           2014-02-15 00:00:00          ` to
    `           2014-03-16 00:00:00          ` . The time zone is
    represented by `           +05:30          ` .  
      
    Note the following about the query given above:

    -   The **`             on            `** condition is optional for
        retrieval.
    -   You can provide the **`             within            `**
        duration in two ways as follows:
        -   **`               within <start_time>, <end_time>              `**
            : The `              <start_time>             ` and
            `              <end_time>             ` can be
            `              STRING             ` or
            `              LONG             ` values. If it is a string
            value, the format needs to be
            `              <yyyy>-<MM>-<dd> <HH>:<mm>:<ss> <Z>             `
            ( `              <Z>             ` represents the timezone.
            It can be omitted if the time is in GMT).  You can provide
            long values if you need to specify the times as Unix
            timestamps (in milliseconds). In the above query, you are
            specifying the time duration via this method in the
            `              STRING             ` format.
        -   **`               within <within_duration>              `**
            : This method allows you to enter the time duration only as
            a STRING value. The format can be one of the following:  
            -   `                <yyyy>-**-** **:**:** <Z>               `
            -   `                <yyyy>-<MM>-** **:**:** <Z>               `
            -   `                <yyyy>-<MM>-<dd> **:**:** <Z>               `
            -   `                <yyyy>-<MM>-<dd> <HH>:**:** <Z>               `
            -   `                <yyyy>-<MM>-<dd> <HH>:<mm>:** <Z>               `
            -   `                <yyyy>-<MM>-<dd> <HH>:<mm>:<ss> <Z>                                               `

            e.g., If the duration is specified as
            `              2018-01-** **:**:**             ` , it means
            within the first month of 2018. This is equal to the
            `              2018-01-01 00:00:00", "2018-02-01 00:00:00             `
            clause provided as per the previous method.  
              
            You do not need to specify the timezone via
            `              <Z>             ` if the time is in GMT.
    -   The `            per           ` clause specifies the time
        granularity for which the aggregations need to be retrieved. The
        value for this clause can be `            seconds           ` ,
        `            minutes           ` ,
        `            hours           ` , `            days           ` ,
        `            months           ` , or
        `            years           ` . The time granularity for which
        you want to retrieve values must have been included in the time
        range you specified when calculating and persisting aggregates.
        For more information, see [Calculating and persisting time-based
        aggregate values - Step 4, substep
        d](#IncrementalAnalysis-SelectTimeRange) .
    -   The output contains the `            total           ` and
        `            avgPrice           ` per
        `            symbol           ` for all the days falling within
        the given time range.

      

!!! tip

The following are other ways in which you can construct the query to
retrieve aggregate values:

-   Instead of providing the within and per values as constants in the
    query itself, you can retrieve them via attributes in the input
    stream definition as shown below.
    1.  Define the input stream as follows.

            define stream TradeSummaryRetrievalStream (symbol string, startTime long, endTime long, perDuration string);

        The above schema allows you to enter the start time and the end
        time of the time duration for which you want to retrieve
        aggregates in the aggregate retrieval request by including
        values for the `             startTime            ` and
        `             endTime            ` attributes. The time
        granularity for which the aggregations need to be retrieved can
        be specified as the value for the
        `             perDuration            ` attribute.

    2.  Then add the query as follows.

        `               from TradeSummaryRetrievalStream as b join TradeAggregation as a on a.symbol == b.symbol                               within b.startTime, b.endTime per b.perDuration                              select a.symbol, a.total, a.avgPrice  insert into TradeSummaryStream;              `

        Here, the `             within            ` and
        `             per            ` clauses refer to the attributes
        in the input stream that specify the time duration and the per
        duration.

-   If you want the retrieved aggregates to be sorted in the ascending
    order based on time, include the
    `           AGG_TIMESTAMP          ` in the
    `           select          ` clause as shown below. Once it is
    included in the `           select          ` clause, you can add
    the `           order by AGG_TIMESTAMP          ` clause.

    `             from TradeSummaryRetrievalStream as b join TradeAggregation as a on a.symbol == b.symbol  within "2014-02-15 00:00:00 +05:30", "2014-03-16 00:00:00 +05:30"  per "days"  select                           AGG_TIMESTAMP                          , a.symbol, a.total, a.avgPrice                           order by AGG_TIMESTAMP                          insert into TradeSummaryStream;            `


## Incremental aggregation in single-node deployments and HA deployments

In order to understand how incremental aggregation is carried out in
different deployment, consider the following example that explains how
the aggregation is executed internally.

Assume that six events arrive in the following sequence, with the given
timestamps.

-   event 0 → 2018-01-01 05:59:58
-   event 1 → 2018-01-01 05:59:58
-   event 2 → 2018-01-01 05:59:59
-   event 3 → 2018-01-01 06:00:00
-   event 4 → 2018-01-01 06:00:01
-   event 5 → 2018-01-01 06:00:02

In this scenario, the aggregation is done for second, minute and hour
durations. Therefore, based on the above timestamps, the second, minute,
and hour during which each event occured is as follows.

![](attachments/112390719/112390724.png){width="500"}

As mentioned before, when storing, time based aggregatesa table is
created for each time granularity in the
`         <AGGREGATE_NAME>_<TIME_GRANUARITY>        ` format. Assuming
that the name of the aggregation in `         TradeAggregation        `
, three tables are created as follows.

-   `          TradeAggregation_SECONDS         `
-   `          TradeAggregation_MINUTES         `
-   `          TradeAggregation_HOURS         `

The incremental analysis related execution that is carried out by the
system with the arrival of each event is described in the table below.

<table style="width:100%;">
<colgroup>
<col style="width: 5%" />
<col style="width: 94%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: center;">Arriving Event</th>
<th>Execution</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">0</td>
<td>The system initially processes event 0 at the <code>             SECOND            </code> executor level. The aggregation is retained in memory until the 58th second elapses.</td>
</tr>
<tr class="even">
<td style="text-align: center;">1</td>
<td>This event also occurs during the 58th second (same as event 0). Therefore, the system aggregates events 0 and 1 together.</td>
</tr>
<tr class="odd">
<td style="text-align: center;">2</td>
<td><p>Event 2 arrives during the 59th second. The 58th second has elapsed at the time of this event arrival. Therefore, the system does the following:</p>
<ul>
<li>Writes the aggregation for the 58th second (i.e., the total aggregation for events 0 and 1) to the <code>               TradeAggregation_SECONDS              </code> table and then forwards the aggregation to be processed at the <code>               MINUTE              </code> executor level.</li>
<li>Processes event 2 in the in-memory table at t he <code>               SECOND              </code> executor level for the 59th second.</li>
</ul></td>
</tr>
<tr class="even">
<td style="text-align: center;">3</td>
<td><p>Event 3 arrives during the second 0 of 06.00 minute. With this arrival, the aggregations for the 59th second and 59th minute expire. Therefore, the system does the following:</p>
<ul>
<li>Writes the aggregation for the 59th second (i.e., aggregation of event 2) to the <code>               TradeAggregation_SECONDS              </code> table, and forwards the aggregation to be processed at the <code>               MINUTE              </code> executor level .</li>
<li>Writes the aggregation for the 59th minute to the <code>               TradeAggregation_MINUTES              </code> table, and forwards the aggregation to be processed at the <code>               HOUR              </code> execution level. The <code>               HOUR              </code> executor processes this for the 5th hour.</li>
<li>Processes event 3 in the in-memory table at the <code>               SECOND              </code> executor level.<br />
<br />
</li>
</ul></td>
</tr>
<tr class="odd">
<td style="text-align: center;">4</td>
<td><p>At the time event 4 arrives during the 1st second of the 06.00 minute, second 0 of the same minute has elapsed. Therefore, the system does the following:</p>
<ul>
<li>Writes the aggregation for second 0 to the <code>               TradeAggregation_SECONDS              </code> table, and forwards the aggregation to be processed at the <code>               MINUTE              </code> executor level.</li>
<li>Processes event 4 in the in-memory table at the <code>               SECOND              </code> executor level.</li>
</ul></td>
</tr>
<tr class="even">
<td style="text-align: center;">5</td>
<td><p>Event 5 arrives during the 2nd second. At this time, the 1st second has elapsed. Therefore, the system does the following:</p>
<ul>
<li>Writes the aggregation for the 1st second (i.e., aggregation for event 4) to the <code>               TradeAggregation_SECONDS              </code> table, and forwards the aggregation to be processed at the <code>               MINUTE              </code> executor level. Once the aggregation for event 4 is forwarded to the <code>               MINUTE              </code> executor level, events 3 and 4 are aggregated together because they occured during the same minute.</li>
<li>Processes event 5 in the in-memory table at the <code>               SECOND              </code> executor level.<br />
</li>
</ul></td>
</tr>
</tbody>
</table>

The following is the aggregation status after all six events have
arrrived.

<table>
<thead>
<tr class="header">
<th>In-memory Table</th>
<th>Available Aggregation Records</th>
<th>Processing In-Memory</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             TradeAggregation_SECONDS            </code></td>
<td><ol>
<li>Aggregation for <code>               2018-01-01 05:59:58              </code> (Aggregation of event 0 and 1)</li>
<li>Aggregation for <code>               2018-01-01 05:59:59              </code> (event 2)</li>
<li>Aggregation for <code>               2018-01-01 06:00:00              </code> (event 3)</li>
<li>Aggregation for <code>               2018-01-01 06:00:01              </code> (event 4)</li>
</ol></td>
<td><p>Aggregation for <code>              2018-01-01 06:00:02             </code> (event 5)</p></td>
</tr>
<tr class="even">
<td><code>             TradeAggregation_MINUTES            </code></td>
<td><p>Aggregation for <code>              2018-01-01 05:59:00             </code> (Aggregation of event 0, 1 and 2)</p></td>
<td><p>Aggregation for 2018-01-01 06:00:00 (i.e., minute 0 of the 6th hour. Here, event 3 and 4 are aggregated together.)</p></td>
</tr>
<tr class="odd">
<td><code>             TradeAggregation_HOURS            </code></td>
<td>None</td>
<td>Aggregation for 2018-01-01 05:00:00 (i.e., the 5th hour. Here, events 0,1, and 2 are aggregated together.)</td>
</tr>
</tbody>
</table>

Now let's consider how the scenario described above works in a single
node deployment and an HA deployment.

-   **Single node deployment**  
    If the `          @store         ` configurtation is not provided,
    the  system stores older aggregations (i.e., aggregations that are
    already completed for a particular second, minute etc., and
    therefore not running in-memory) in in-memory tables. In such a
    situation, a server failure results in all these aggregations done
    up to the time of the failure being lost.  
      
    If a database configuration is provided via the
    `          @store         ` annotation, the older aggregations are
    stored in the external database. Therefore, if the node fails, the
    system recreates the in-memory running aggregations from this stored
    data once you restart the server. In the given scenario, in-memory
    aggregations for the `          MINUTE         ` execution level can
    be recreated with data in the
    `          TradeAggregation_SECONDS         ` table (i.e., points 3
    and 4 in the above aggregation status summary table). Similarly,
    in-memory aggregations for the `          HOUR         ` execution
    level can be recreated from the
    `          TradeAggregation_MINUTES         ` table.  
      
    However, the recreation described above cannot be done for the most
    granular duration for which aggregation done. e.g., In the given
    scenario, the system cannot recreate  the in-memory aggregations for
    the `          SECOND         ` execution level because there is no
    database table for a prior duration.  
      
    Assume that in the given scenario, a server failure occurs after all
    five events have arrived. Once you restart the server, only event 5
    is lost because that was the only aggregation being executed for the
    `          SECOND         ` execution level in the in-memory tables
    at that time.
-   **HA deployment**

        !!! info
    
        To understand how WSO2 SP runs as a minimum HA cluster, see [Minimum
        High Availability (HA)
        Deployment](https://docs.wso2.com/display/SP400/Minimum+High+Availability+%2528HA%2529+Deployment)
        .
    

    In a minimum HA setup, one runs as an active node and the other is
    passive. No events are lost even if the
    `           @store          ` configuration is not provided due to
    snapshoting the current state of SP. When a snapshot iof the SP
    state is created, the system does not recreate aggregations from
    tables because it is not required. The newly active node (which was
    previously passive) continues to process the new events that arrive
    after the failure of the other node as in a situation where no
    server failura has occured.

The following is a summary of the retrievability of aggregates based on
how they are stored and how WSO2 SP is deployed.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><table>
<caption> </caption>
<tbody>
<tr class="odd">
<td><img src="attachments/112390719/112390723.png" class="gliffy-macro-image" /></td>
</tr>
</tbody>
</table></td>
</tr>
</tbody>
</table>

## Scaling through distributed aggregations

Distributed aggregations partially process aggregations in different
nodes. This allows you to assign one node to process only a part of an
aggregation (regional scaling, etc.). In order to do this all the
aggregations must have a physical database and must be linked to the
same database.

#### Syntax

The `         @PartitionById        ` annotation must be added before
the aggregation definition as shown below.

`           @store(type="<store type>", ...)                                `

**`            @PartitionById(enable='true')           `**

`           define aggregation <aggregator name>          `  
`           from <input stream>          `

`           select <attribute name>, <aggregate function>(<attribute name>) as <attribute name>, ...          `

`           group by <attribute name>          `

`           aggregate by <timestamp attribute> every <time periods> ;          `

You can enable all Siddhi applications to partition aggregations in this
manner by adding the following system parameters in the
`         <SP_HOME>/conf/<PROFILE/deployment.yaml        ` file under
`         siddhi        ` .

``` xml
    siddhi:
      properties
        partitionById: true
        shardId: wso2-sp
```

A unique ID must be provided for each node via a system parameter named
`         shardId        ` . This parameter is required when
partitioning aggregations is enabled for a single Siddhi application as
well as when it is enabled for all Siddhi applications.

!!! tip

To maintain data consistency, do not change the shard IDs after the
first configuration.


When you enable the aggregation partitioning feature, a new column ID
named `         SHARD_ID        ` is introduced to the aggregation
tables. Therefore, you need to do one of the following options after
enabling this feature to avoid errors occuring due to the differences in
the table schema.

-   Delete all the aggregation tables for `          SECONDS         ` ,
    `          MINUTES         ` , `          HOURS         ` ,
    `          DAYS         ` , `          MONTHS         ` ,
    `          YEARS         ` .
-   Edit the aggregation tables by adding a new column named
    `          SHARD_ID         ` , and specify it as a primary key.

#### Example

The following query can be included in two Siddhi applications in two
different nodes that are connected to the same database. Separate input
events are generated for both nodes. Each node performs the aggregations
and stores the results in the database. When the aggregations are
retrieved, the collective result of both nodes are considered.

``` sql
    define stream TradeStream (symbol string, price double, quantity long, timestamp long);
    
    @store(type='rdbms', jdbc.url="jdbc:mysql://localhost:3306/TestDB", username="root", password="root" , jdbc.driver.name="com.mysql.jdbc.Driver")
    @PartitionById(enable='true')
    define aggregation TradeAggregation
    from TradeStream
    select symbol, avg(price) as avgPrice, sum(quantity) as total
    group by symbol
    aggregate by timestamp every sec ... year;
```

Let's assume that the following input events were generated for the two
nodes during a specific hour.

**Node 1**

| Event | symbol | price | quantity |
|-------|--------|-------|----------|
| 1     | wso2   | 100   | 10       |
| 2     | wso2   | 100   | 20       |

**Node 2**

| Event | symbol | price | quantity |
|-------|--------|-------|----------|
| 1     | wso2   | 100   | 10       |
| 2     | wso2   | 100   | 30       |

Here, node 1 calculates an hourly total of 30, and node 2 calculates an
hourly total of 40. When you retrieve the total for this hour via a
retrieval query, the result is 70.

!!! info

For more information about incremental aggregation, see [Siddhi Query
Guide - Incremental
Aggregation](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#incremental-aggregation)
.

