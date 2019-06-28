# Installing Stream Processor Using Docker

!!! tip

Before you begin:

-   You need the following system requrements:
    -   3 GHz Dual-core Xeon/Opteron (or latest)
    -   8 GB RAM
    -   10 GB free disk space
-   Install Docker by following the instructions provided
    [here](https://docs.docker.com/install/) .


WSO2 provides open source Docker images to run WSO2 Stream Processor in
Docker Hub. You can view these images from
[here](https://hub.docker.com/u/wso2/) .

## Downloading and installing WSO2 Stream Processor

Issue the following commands to pull tghe required WSO2 Stream Processor
profile with updates from the Docker image.

| Profile   | Command                                                      |
|-----------|--------------------------------------------------------------|
| worker    | `             docker pull wso2/wso2sp-worker            `    |
| manager   | `             docker pull wso2/wso2sp-manager            `   |
| editor    | `             docker pull wso2/wso2sp-editor            `    |
| dashboard | `             docker pull wso2/wso2sp-dashboard            ` |

## Running WSO2 Stream Processor

To run WSO2 SP, follow the steps below:

1.  To start each WSO2 Stream Processor profile in a Docker container,
    issue the following commands:

    -   **For dashboard:  
        **

        ``` text
                docker run -it -p 9643:9643  wso2/wso2sp-dashboard
        ```

    -   **For editor:  
        **

        ``` text
                    docker run -it \
                    -p 9390:9390 \
                    -p 9743:9743 \
                    wso2/wso2sp-editor
        ```

    -   **For manager:  
        **

        ``` text
                    docker run -it wso2/wso2sp-manager
        ```

    -   **For worker:**

        ``` text
                    docker run -it wso2/wso2sp-worker
        ```

2.  Once the container is started, access the UIs of each profile via
    the following URLs on your favourite browser. You can enter
    `          admin         ` as both the username and the password.
    -   **Dashboard**
        -   **Business Rules** :
            `              https://localhost:9643/business-rules             `
        -   **Dashboard Portal** :
            `              https://localhost:9643/portal             `
        -   **Status Dashboard** :
            `              https://localhost:9643/monitoring             `
    -   **Editor**
        -   **Steam Processor Studio** :
            `              https://localhost:9390/editor             `
        -   **Template Editor** :
            `              https://localhost:930/template-editor             `
