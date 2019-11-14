# Getting the Streaming Integrator Running with Docker in 5 Minutes


## Introduction

This guide shows you how to run Streaming Integrator in Docker. This involves installing Docker, running the Streaming Integrator in Docker and then deploying and running a Siddhi application in the Docker environment.

!!!tip "Before you begin:"
    - The system requirements are as follows:
        -   3 GHz Dual-core Xeon/Opteron (or latest)
        -   8 GB RAM
        -   10 GB free disk space

    - Install Docker by following the instructions provided in [here](https://docs.docker.com/install/).

## Downloading and installing the Streaming Integrator

In this scenario, you are downloading and installing the Streaming Integrator via Docker.

WSO2 provides open source Docker images to run WSO2 Streaming Integrator in Docker Hub. You can view these images [In Docker Hub - WSO2](https://hub.docker.com/u/wso2/).

To run the Streaming Integrator in the  open source image that is available for it

1. To pull the required WSO2 Streaming Integrator distribution with updates from the Docker image, issue the following command.

    `docker run -it wso2/streaming-integrator`

2. Expose the required ports via docker when running the docker container. In this scenario, you need to expose the following ports:
    - The 9443 port where the Streaming Integrator server is run.
    - The 8006 HTTP port from which Siddhi application you are deploying in this scenario receives messages.

    To expose these ports, issue the following command.

    `docker run -p 9443:9443 -p 8006:8006 wso2/streaming-integrator`


## Creating and deploying the Siddhi application

Let's create a simple Siddhi application that receives an HTTP message, does a simple transformation to the message, and then logs it in the SI console.

1. Start the Streaming Integrator Tooling via one of the following methods depending on your operating system:

    - On MacOS/Linux/CentOS, open a terminal and issue the following command:

        `sudo wso2si-tooling-<VERSION>`

    - On windows, go to **Start Menu -> Programs -> WSO2 -> Streaming Integrator Tooling**. A terminal opens.

    Then access the Streaming Integration Tooling via the `http://<HOST_NAME>:<TOOLING_PORT>/editor` URL.

    !!!info
            The default URL is `http://<localhost:9390/editor`.

    The Streaming Integration Tooling opens as shown below.
       ![Streaming Integrator Tooling Welcome Page](../images/quick-start-guide-101/Welcome-Page.png)


2. Click **New** and copy-paste the following Siddhi application to the new file you opened.

    ```
    @App:name('MySimpleApp')

    @App:description('Receive events via HTTP transport and view the output on the console')

    @Source(type = 'http', receiver.url='http://0.0.0.0:8006/productionStream', basic.auth.enabled='false',
       @map(type='json'))
    define stream SweetProductionStream (name string, amount double);

    @sink(type='log')
    define stream TransformedProductionStream (nameInUpperCase string, amount double);

    -- Simple Siddhi query to transform the name to upper case.
    from SweetProductionStream
    select str:upper(name) as nameInUpperCase, amount
    insert into TransformedProductionStream;
    ```

    !!!info "Note"
        Note the following about this Siddhi application.
        - The Siddhi application operates in Docker. Therefore, the HTTP source configured in it uses a receiver URL where the host number is `0.0.0.0`.
        - The `8006` port of the receiver URL is the same HTTP port that you previously exposed via Docker.


3. Save the Siddhi application by clicking **File** => **Save**.


4. To deploy the Siddhi application, click the **Deploy** menu option and then click **Deploy to Server**. The **Deploy Siddhi Apps to Server** dialog box opens as shown in the example below.

    ![Deploy to Server dialog box](../images/getting-si-run-with-mi/deploy-to-server-dialog-box.png)

    1. In the **Add New Server** section, enter information as follows:

          | Field           | Value                            |
          |-----------------|----------------------------------|
          | **Host**        | 0.0.0.0                        |
          | **Port**        | `9443`                           |
          | **User Name**   | `admin`                          |
          | **Password**    | `admin`                          |


        ![Add New Server](../images/getting-si-run-with-mi/add-new-server.png)

        Then click **Add**.

    2. Select the check boxes for the **MySimpleApp** Siddhi application and the server you added as shown below.

        ![Deploy Siddhi Apps to Server](../images/getting-si-run-with-mi/select-siddhi-app-and-server.png)

    3. Click **Deploy**.

        When the Siddhi application is successfully deployed, the following message appears in the **Deploy Siddhi Apps to Server** dialog box.

        ![Deployment Status](../images/getting-si-run-with-mi/siddhi-application-deployment-status.png)


        The following is logged in the console in which you started the Streaming Integrator in Docker.

        ![Deployment Status](../images/hello-world-with-docker/siddhi-app-deployed-in-docker-log.png)



## Trying-out the Siddhi application

To try out the `MySimpleApp` Siddhi application you deployed in Docker, issue the following CURL command.

```
curl -X POST -d "{\"event\": {\"name\":\"sugar\",\"amount\": 20.5}}"  http://0.0.0.0:8006/productionStream --header "Content-Type:application/json"
```

The following output appears in the console in which you started the Streaming Integrator in Docker.

![HTTP Response](../images/hello-world-with-docker/http-response.png)

