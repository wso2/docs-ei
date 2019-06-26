# Concepts

##Data Streaming

Data streaming refers to the continuous transfer of data in high volumes and in high speed. The following are a few
 examples: <br/>
 
 - Reading information such as the temperature, humidity, etc., per millisecond from a sensor.
 - Capturing the daily transations recorded in an ATM as and when they occur.
 - Tracking information relating to the speed and location of vehicles and other mobile objects.
 - Recording the sales of ononline store in real time.
 
 ## Streaming Integration
 Streaming integration takes place when data from various sources and in different formats flowing at high speed and 
 in high volumes are integrated together. The Stream Processor profile of WSO2 EI performs streaming integration as 
 follows:
 
 <DIAGRAM>
 
 + **Allowing downstream applications to access streaming data**<br/>
 Different sources of data publish their data via different transports and in different formats. They can disseminate 
 their data in a stream, load it to a data store such as RDBMS, push it into a JMS topic, etc. The Stream Processor 
 profile has the capability to make this data consumable by the different downstream applications, regardless of the 
 transport, the format and the manner in which it is shared. To do make data from a specific source accessible to a 
 downstream application,  the Stream Processor may redirect that data to another stream, a data store, a topic etc., 
 that is accessible by that downstream application. To do so, it can communicate with the data source via the relevant 
 transport. The Stream Processor may also do the required transformations to convert the data to a format that can be 
 read by the application.
 
 + **Allowing downstream applications to publish streaming data**<br/>
 Once downstream applications process data, you may need to publish the output in different ways. The interfaces to 
 which the output needs to be published may require the data to be delivered via specific transports and presented in 
 specific formats The transport via which the data is published and the data format to use can differ based on the 
 client that receives the output. The Stream Processor profile of WSO2 EI can perform the required transformations to 
 the output so that they are in the format supported by the client, and deliver it via the required transport. It can 
 also direct the output as a stream, to a data store, to a JMS topic etc., as required.
 
 
 To support the above, the Stream Processor profile is shipped with over 60 connectors (i.e.,JMS, HTTP, Kafka, TCP, 
 etc.) that enable it to receive and publish data streams in many formats and protocols, and to store data in both 
 relational and non-relational databases.

## Event-based messaging

An event is a request or a response that adheres to a specific schema. To understand this, consider the example where
 following information about cash withdrawals from an ATM are captured.
 
 <TABLE>
 
 Here, the same details are captured for each transaction. In this scenario, each transaction is considered and event,
  and the details (referred to as atributes) listed above form the event schema. A collection of such events form a 
  stream as depicted in the image below.
  
<PICTURE>

Event-based messaging is carried out when the all the messages transferred are events that adhere to a specifc event schema.



## Event-driven integration

The Streaming Integration profile allows WSO2 EI to carry out event-driven integration via event based messaging.

The stream processing capabilities of the Streaming integratiion profile allows you to detect simple occurrences 
(e.g.,The number of requests for a specific service exceeding the target), as well as complex events such as a sequence
 of related events or patterns. Once an occurence, sequence, or pattern is detected, it often requires a response. This
  response can be a simple response such as the genration of an alert, or the initiation of a complex integration flow.
  
  e.g., The following diagram depicts how the streaming integration profile can detect a credit card fraud, and triggers
   mnultiple actions in response.
   
   <DIAGRAM>

## Stateful integration

<DIAGRAM>

Stateful integration takes place when the integration flow retains information about the previous requests it handled 
in memory. This information is taken into account when making decisions in the future. WSO2 EI achieves this via the Stream Processor profile. To understand how stateful integration is useful in real world scenarios, consider an example where you need to throttle requests for a service if it receives more than a fixed number of requests during a defined time span. To do this, the system needs to be aware of the number of requests already received within the defined span of time.

