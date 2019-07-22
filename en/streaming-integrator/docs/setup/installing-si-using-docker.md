# Installing Streaming Integrator Using Docker

Before you begin:

-   You need the following system requrements:
    -   3 GHz Dual-core Xeon/Opteron (or latest)
    -   8 GB RAM
    -   10 GB free disk space
-   Install Docker by following the instructions provided
    [here](https://docs.docker.com/install/) .


WSO2 provides open source Docker images to run WSO2 Streaming Integrator in
Docker Hub. You can view these images from
[here](https://hub.docker.com/u/wso2/) .

## Downloading and installing WSO2 Streaming Integrator

Provide the following commands to pull the required WSO2 Streaming Integrator
distribution with updates from the Docker image.

```
docker pull wso2/streaming-integrator
```

## Running WSO2 Streaming Integration

To run WSO2 Streaming Integrator, execute the follow the command below:

```
docker run -it wso2/streaming-integrator
```

!!! tip
    to expose the required ports via docker when running the docker container please use the following command. 
    
    ```bash
        docker run -it \
        -p 9443:9443   \
        -p 9090:9090   \
        -p 7070:7070   \
        -p 7443:7443   \
        -p 9712:9712   \
        -p 7711:7711   \
        -p 7611:7611   \
        wso2/streaming-integrator
    ```
    For more details regarding the ports in Streaming Integrator please refer to ["Configuring Default Ports"](https://docs.wso2.com/display/SP4xx/Configuring+Default+Ports) 