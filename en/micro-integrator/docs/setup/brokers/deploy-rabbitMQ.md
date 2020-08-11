# RabbitMQ Deployment 

This section describes how to setup a rabbitmq deployment.
   
## Setting up a single instance VM Based Deployment for Testing (version 3.8.2) on Unix OS

1. Download RabbitMQ distribution to the desired location

    ```bash
    wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.2/rabbitmq-server-generic-unix-3.8.2.tar.xz
    ```
    
2. Extract the distribution

    ```bash
    tar -xf rabbitmq-server-generic-unix-3.8.2.tar.xz
    ```

3. Install the erlang distribution

    ```bash
    wget -O- https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo apt-key add -
    echo "deb https://packages.erlang-solutions.com/ubuntu bionic contrib" | sudo tee /etc/apt/sources.list.d/rabbitmq.list && \
    sudo apt update && \
    sudo apt -y install erlang
    ```

4. Navigate to `$RABBITMQ_HOME/sbin` and execute the following command. Here `$RABBITMQ_HOME` is the location where the extraction was done in step 2.

    ```bash
    sudo ./rabbitmq-server -detached
    ```
5. To enable management plugin, execute the following command

    ```bash
    sudo ./rabbitmq-plugins enable rabbitmq_management
    ```
6. Visit the following url to view the UI

    ```bash
    http://localhost:15672/#/
    ```
    
## Production Guideline

For deployment please refer the [Downloading and Installing RabbitMQ](https://www.rabbitmq.com/download.html) official guide of RabbitMQ.

### High-Availability with RabbitMQ 

For highly availability, rabbitmq servers need to be clustered. Please refer the [Clustering Guide](https://www.rabbitmq.com/clustering.html).

!!! Tip
     - Minimum node count recommended for clustering is 3 since RabbitMQ uses a [Quorum based distributed consensus](https://www.rabbitmq.com/clustering.html#node-count).
     - Queues need to be mirrored for high availability and for fault tolerance. Refer [Mirrored Queues](https://www.rabbitmq.com/ha.html).
 