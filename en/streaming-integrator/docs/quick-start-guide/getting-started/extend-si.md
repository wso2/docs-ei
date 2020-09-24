# Step 5: Extend the Streaming Integrator

The Streaming Integrator is by default shipped with most of the available Siddhi extensions by default. If a Siddhi extension you require is not shipped by default, you can download and install it.

In [Step 2: Create the Siddhi Application](create-the-siddhi-application.md), the Siddhi application you created processes data recived from a database and publishes the output to a file. Assume that you need to enable certain parties (e.g., product managers, brand managers, etc.) to subscribe for production data related to specific products of their choice. To do this, you need to use a messaging system such as Kafka so that you can filter production data for each product and publish it in a separate topic, allowing each party to subscribe only for topics of their choice.

WSO2 Streaming Integrator is not fully configured to integrate with Kafka by default. Therefore, let's install the `siddhi-io-kafa` extension and its dependencies as follows.

!!! tip "Before you begin:"
    You need to download Kafka from [the Apache site](https://www.apache.org/dyn/closer.cgi?path=/kafka/2.3.0/kafka_2.12-2.3.0.tgz) as already instructed under [Step 1: Download Streaming Integrator and Dependencies](download-install-and-start-si.md).

1. Be sure that the WSO2 Streaming Integrator server is started.

2. Navigate to the `<SI_HOME>bin` directory and issue the following command.

    - **For Linux**: `./extension-installer.sh install`<br/>
    - **For Windows**: `extension-installer.bat install`
    
    When the extension is successfully installed, the following is displayed in the log.
    
    ```
    Installation completed with status: INSTALLED. Please restart the server.
    ```
3. Restart the Streaming Integrator server as instructed.

## What's Next?

To update the `SweetFactoryApp` Siddhi application you created so that it can publish to Kafka topics via the Kafka extension you installed, proceed to [Step 6: ]
