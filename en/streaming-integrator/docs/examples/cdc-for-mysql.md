# Change Data Capturing for MySQL

Streaming Integrator allows you to capture changes to your data sources, in a streaming way.

This tutorial takes you through the different modes and  options you could use to perform Change Data Capturing (CDC).  

## Tutorial Outline
- Receive insert events on polling mode
- Receive update events on polling mode
- Receive delete events on polling mode
- Receive insert events on listening mode
- Receive update events on listening mode
- Receive delete events on listening mode

## Introduction

#### Polling mode and listening mode

There are two modes you could perform CDC using the Streaming Integrator: Polling mode and Listening mode. 

In polling mode, you periodically poll the datasource for CDC while on listening mode, the Streaming Integrator will keep listening for changes in the datasource.

#### Type of events captured

You could capture following type of events:
- Insert
- Update
- Delete  

## Receive insert events on polling mode