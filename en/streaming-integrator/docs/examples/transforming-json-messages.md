# Transforming Json Messages

## Introduction

Consuming Json messages and producing Json messages is a common requirement in streaming integration scenarios. 

This tutorial takes you through how you can consume, process and produce - simple as well as complex Json messages, using the Streaming Integrator (SI).   

## Tutorial Outline

- [Preparing the server](#preparing-the-server)
- [Consuming Json messages](#Consuming-Json-messages)
    - [Consuming Json messages in default format](#Consuming-Json-messages-in-default-format)
    - [Consuming Json messages in custom format](#Consuming-Json-messages-in-custom-format)
- [Producing Json messages](#Producing-Json-messages)
    - [Producing Json messages in default format](#Producing-Json-messages-in-default-format)
    - [Producing Json messages in custom format](#Producing-Json-messages-in-custom-format)
- [Processing Json messages](#Processing-Json-messages)
    
## Consuming Json messages

### Consuming Json messages in default format

In this section, you will consume a Json message in following format:
```
{
    "event":{
        "symbol":WSO2,
        "price":55.6,
        "volume":100
    }
}
```
This is called the `default` format because the SI is able to directly map above json message into an event, with attributes `symbol`, `price` and `volume`. 

Let's create a simple Siddhi app which consumes a json message in default format.




