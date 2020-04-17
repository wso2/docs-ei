# Streaming Integration Overview

This section helps you to understand streaming integration and how to perform it with WSO2 Streaming Integrator in less than 30 minutes

First, let's understand the concept of streaming integration. For this, let's consider an example where the temperature of multiple rooms are being monitored via sensors.

![Streams](../../images/quick-start-guide-101/stream.png)

Each reading that reports the room ID, the device and the temperature of the room that is read by the device represents an **event**.

Many such events form a **stream**.

In Streaming typically involves handling data received/published via streams in high speed and high volume. In this example, a single device may do multiple temperature readings per second, and the stream reports the readings by multiple devices in a given second.

WSO2 Streaming Integrator allows data to be received from multiple sources such as files, databases, cloud-based applications, and streaming messaging systems (such as Kafka) in a streaming manner. It uses the [Siddhi Query Language](https://siddhi.io/en/v4.x/docs/query-guide/), to process this data, and then publishes the results in a streaming manner to multiples destinations such as files, databases, cloud-based applications, and streaming messaging systems in a streaming manner.

**Streaming Integration** involves processing streaming data and incorporating them into integration flows so that action can be taken based on the output derived. In this example, you may want to calculate the average temperature for every three readings to get a better idea of whether the temperature is on a rising trend or not.

To understand how Streaming Integration is performed via the WSO2 Streaming Integrator, follow the sections below.

[**Step 1: Download and Install Streaming Integrator**](download-install-and-start-si.md)<br/><br/>
[**Step 2: Create a Siddhi Application**](create-the-siddhi-application.md)<br/><br/>
[**Step 3: Test the Siddhi Application**](test-siddhi-application.md)<br/><br/>
[**Step 4: Deploy the Siddhi Application**](deploy-siddhi-application.md)<br/><br/>
[**Extend Streaming Integrator**](extend-si.md)<br/><br/>
