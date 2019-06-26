
# Overview of WSO2 Streaming Integrator


The Streaming Integrator profile of WSO2 EI consumes streaming data, applies stream processing techniques to process 
this data, and then publishes the processed data to downstream applications reliably, or for the purpose of triggering 
an action.

The streaming data can be consumed from applications, cloud, databases, files and from streams in multiple formats. The
 Streaming Integrator applies the Stream Processing logic of the Siddhi Query Language to process this data. The 
 processed data is in turn can be published to multiple applications, clouds, databases, files and streams in multiple 
 formarts.
 
 <DIAGRAM>
 
 The main use cases addressed by the Streaming Integrator profile are as follows:
 
 + Transforming Data </b> 
 This involves transforming ther data from one format to another (e.g., to/from XML, JSON, AVRO, etc.)
 
 + Enriching data
 This involves enriching data received from a specific source by combining it with databases, services, and via inline calculations

 + Creating joins
 This involves joining multiple data streams to create an aggregate stream.
 
 + Cleansing data
 This involves cleaning data by filtering it and applying custom mappings.
 
 + Deriving insights
 This involves deriving insights via Patterns and Sequences.
 
 + Summarizing data.
 This involves summarizing data as an when it is generated using time windows or length windows.