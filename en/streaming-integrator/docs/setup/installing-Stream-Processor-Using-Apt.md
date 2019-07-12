# Installing Stream Processor Using Apt

To install WSO2 Stream Processor with Apt, follow the steps below:

1.  To add the Apt key, issue the following command.

    ``` text
        apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys "379CE192D401AB61"
    ```

2.  Add the repository to the source list by issuing the following
    command.  

    ``` text
            echo "deb https://dl.bintray.com/wso2/deb sp_430 release" | \
            sudo tee -a /etc/apt/sources.list
    ```

3.  Install the package by issuing the following command.  

    ``` text
            apt update
            apt install wso2sp-4.3.0
    ```

## Running the product

The following commands are available for running each runtime of the
WSO2 Stream Processor. Access the URLs on your favorite web browser. You
can use the following credentials to log in:

-   **Username** : `          admin         `
-   **Password** : `          admin         `

The instructions to run each profile are as follows:

#### Dashboard profile

Issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-dashboard        `

You can access the different components in this profile via the
following URLs.

| Component        | URL                                                                |
|------------------|--------------------------------------------------------------------|
| Dashboard Portal | `              https://localhost:9643/portal             `         |
| Business Rules   | `              https://localhost:9643/business-rules             ` |
| Status Dashboard | `              https://localhost:9643/monitoring             `     |
| Policies         | `              https://localhost:9643/policies             `       |

  

#### Worker profile

Issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-worker        `

#### Manager profile

Issue the following command in the terminal.

`         wso2sp-         <SP_VERSION>         -manager        `

#### Editor profile

Issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-editor        `

You can access the different components in this profile via the
following URLs.

| Component       | URL                                                                |
|:----------------|:-------------------------------------------------------------------|
| Editor          | `              http://localhost:9390/editor             `          |
| Template Editor | `              http://localhost:9390/template-editor             ` |
