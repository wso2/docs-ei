# Installing Stream Processor Using Docker Compose

This topic provides instructions on how to install WSO2 Stream Processor
using Docker Compose. You can install the product with updates or
install the WSO2 Stream Processor general availability release (that
does not include updates).

!!! tip

System Requirements

-   3 GHz Dual-core Xeon/Opteron (or latest)
-   8 GB RAM
-   10 GB free disk space


To install the Stream Processor using Docker Compose, follow the steps
below:

1.  Install Docker by following the instructions provided
    [here](https://docs.docker.com/install/) .
2.  Login to the WSO2 Docker registry by executing following Docker
    command. When prompted, enter the username and password of your free
    trial account.  

    ``` text
        docker login docker.wso2.com
    ```

3.  Clone the WSO2 Analytics Docker resource Git repository and switch
    to the relevant resource directory by executing the following
    commands.

    ``` text
            git clone https://github.com/wso2/docker-sp
            cd docker-sp/docker-compose
    ```

      

4.  Switch to the docker-compose/editor-worker-dashboard directory.  

    1.  Editor with Worker and Dashboard.  

        `             cd editor-worker-dashboard            `

    2.  WSO2 Stream Processor Distributed deployment.  

            cd sp-distributed

          

5.  Execute the following Docker command to start the selected
    deployment.

        docker-compose up

6.  Once WSO2 Stream Processor is successfully deployed, access the
    following URLs on your web browser using the following credentials:

    -   username: `             admin            `

    -   password: `             admin            `

    ``` text
            Editor: https://localhost:9390/editor
            Dashboard:  https://localhost:9643/monitoring
    ```

    You can view the WSO2 Stream Processor samples as well as implement
    new Siddhi applications using the editor. These Siddhi applications
    can then be imported and deployed in the worker node. Follow [this
    document](https://docs.wso2.com/display/sp430/Exporting+a+Siddhi+File)
    for a detailed guide on how to export a Siddhi application. [This
    document](https://docs.wso2.com/display/sp430/Deploying+Streaming+Applications)
    provides details on deploying a Siddhi application using the WSO2
    Stream Processor Rest API.

    Use the worker node URL shown below when using the WSO2 Stream
    Processor Rest API.

    ``` text
            https://localhost:9443/siddhi-apps
    ```

7.  Access the stream processor dashboard via the following URL using
    the following credentials:  

    -   username: `             admin            `

    -   password: `             admin            `

    ``` text
            https://localhost:9643/monitoring/
    ```

      

    To start using WSO2 Stream Processor, read the [Quick Start
    Guide](http://docs.wso2.com/stream-processor/Quick%20Start%20Guide)
    .
