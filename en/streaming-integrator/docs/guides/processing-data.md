# Processing Data

Processing data involves making changes to data to generate a required output. WSO2 Streaming Integrator allows you to carry out a wide range of operations to process streaming data. These operations are supported via [Siddhi Extensions](https://siddhi.io/en/v5.1/docs/extensions/). These operations can be broadly categorized into five categories as follows:

- Cleansing
- Transforming
- Enriching
- Aggregating
- Correlating

## Cleansing data

When you receive input data via the Streaming Integrator, it may consist of data that is not required to generate the required output, null values for certain attributes, etc. Cleansing data refers to refining the input data received by assigning values where there are missing values (if there are applicable values), filtering out the data that is not required, etc.



## Transforming data

The Streaming Integrator allows you to perform a wide range of transformations to the input data received. A main type of transformation supported is transforming a message from one format to another. In addition, you can perform a range of mathematical, regex, string, unit conversion, map, json, etc., transformations via [Siddhi Extensions](https://siddhi.io/en/v5.1/docs/extensions/). You can also write a custom script to perform an required transofrmation of data.

## Enriching data

Enriching data involves integrated the data received into a streaming integration flow with data from other medium such as a data store, another data stream, or an external service to derive an expected result.

## Summarizing data

Summarizing data refers to obtaining aggregates in an incremental manner for a specified set of time periods.



## Correlating data

The streaming integrator can correlate data in order to detect patterns and trends in streaming data. Correlating can be done via patterns as well as sequences.