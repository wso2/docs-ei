# Getting SI Running with MI in Five Minutes

This quick start guide explains how to trigger an integration flow using a message received by the Streaming Integrator.

In this example, the same message you send to the Micro Integrator goes through the `inSeq` defined, and uses the `respond` mediator to send back the response to the Streaming integrator.

!!!tip "Before you begin:"
    - Download and install both the Streaming Integrator and the Streaming Integrator Tooling from [here](Link).<br/>
    - Download and install both the the Micro Integrator from [here](Link).<br/>
    - Start the Streaming Integrator via one of the following methods depending on your operating system.<br/>
        - On MacOS/Linux/CentOS, open a terminal and issue the following command:<br/>
            `sudo wso2si`<br/>
        - On windows, go to **Start Menu -> Programs -> WSO2 -> Enterprise Integrator**. This opens a terminal. Start Streaming Integrator profile.<br/>
    - Start the Streaming Integrator Tooling via one of the following methods depending on your operating system..<br/>
        - On MacOS/Linux/CentOS, open a terminal and issue the following command:<br/>
            `sudo wso2si-tooling-<VERSION>`<br/>
        - On windows, go to **Start Menu -> Programs -> WSO2 -> Streaming Integrator Tooling**. A terminal opens.

To create and deploy Siddhi application that triggers an integration flow, and then try it out by sending events, follow the procedure below:

1. Access Streaming Integrator Tooling via the URL printed in the start up logs.

    !!!info
        The default URL is http://localhost:9390/editor.

    The Streaming Integrator Tooling opens as follows.

    ![Streaming Integrator Tooling Welcome Page](../images/getting-si-run-with-mi/Welcome-Page.png)

2. Open a new Siddhi file by clicking **New**. Then copy and paste the following Siddhi application to it.

    ```
    @App:name("grpc-call-response")
    @App:description("This siddhi app triggers integration flow from SI to MI")


    @source(type='http', receiver.url='http://localhost:8006/inputstream', @map(type = 'json'))
    define stream InputStream (message String, headers string);

    @sink(type='grpc-call', publisher.url = 'grpc://localhost:8888/org.wso2.grpc.EventService/process/inSeq', sink.id= '1', headers='{{headers}}', @map(type='json'))
    define stream FooStream (message String, headers string);

    @source(type='grpc-call-response', sink.id= '1', @map(type='json'))
    define stream BarStream (message String, headers string);

    @sink(type='log')
    define stream OutputStream (message String, headers string);

    from InputStream
    select *
    insert into FooStream;

    from BarStream
    select *
    insert into OutputStream
    ```

3. Save the Siddhi application.

4. Click the **Deploy** menu option and then click **Deploy to Server**. The **Deploy Siddhi Apps to Server** dialog box opens as shown in the example below.

    ![Deploy to Server dialog box](../images/getting-si-run-with-mi/deploy-to-server-dialog-box.png)

    1. In the **Add New Server** section, enter information as follows:

           | Field           | Value                            |
           |-----------------|----------------------------------|
           | **Host**        | Your host                        |
           | **Port**        | `9443`                           |
           | **User Name**   | `admin`                          |
           | **Password**    | `admin`                          |


        ![Add New Server](../images/getting-si-run-with-mi/add-new-server.png)

        Then click **Add**.

    2. Select the check boxes for the **grpc-call-response.siddhi** Siddhi application and the server you added as shown below.

        ![Deploy Siddhi Apps to Server](../images/getting-si-run-with-mi/select-siddhi-app-and-server.png)

    3. Click **Deploy**.

        When the Siddhi application is successfully deployed, the following message appears in the **Deploy Siddhi Apps to Server** dialog box.

        ![Deployment Status](../images/getting-si-run-with-mi/siddhi-application-deployment-status.png)

    As a result, the `grpc-call-response.siddhi` Siddhi application is saved in the `<SI_HOME>/wso2/server/deployment/siddhi-files` directory.

5. Deploy the required artifacts in the Micro Integrator as follows:

    1. Save the following GRPC inbound endpoint  as `grpcInboundEndpoint.xml` in the `<MI_Home>/repository/deployment/server/synapse-configs/default/inbound-endpoints` directory.

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <inboundEndpoint xmlns="http://ws.apache.org/ns/synapse" name="GrpcInboundEndpoint" sequence="inSeq" onError="fault" protocol="grpc" suspend="false">
            <parameters>
                <parameter name="inbound.grpc.port">8888</parameter>
            </parameters>
        </inboundEndpoint>
        ```

    2. Save the following sequence that includes a `respond` mediator in the `<MI_Home>/repository/deployment/server/synapse-configs/default/sequences` directory. You can name the file `InSeq.xml`.

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <sequence xmlns="http://ws.apache.org/ns/synapse" name="inSeq">
           <log level="full"/>
           <respond/>
        </sequence>
        ```


6. Issue the following CURL command to send an event to the Streaming Integrator. This is received via the `http` source configured in the `grpc-call-response` Siddhi application, and it triggers an integration flow with the Micro Integrator.

    `curl -X POST -d "{\"event\":{\"message\":\"http_curl\",\"headers\":\"'Content-Type:json'\"}}" http://localhost:8006/inputstream --header "Content-Type:application/json"`

    The following response is logged in the console in which you are running the SI server.

    ```
        INFO {io.siddhi.core.stream.output.sink.LogSink} - HelloMi : OutputStream : Event{timestamp=1573188022317, data=[http_curl, 'Content-Type:json'], isExpired=false}
    ```