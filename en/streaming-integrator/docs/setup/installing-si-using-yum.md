# Installing Stream Processor Using Yum

The following sections cover how to install and run WSO2 Stream
Processor using Yum.

-   [Prerequisites](#InstallingStreamProcessorUsingYum-Prerequisites)
-   [Installing the
    package](#InstallingStreamProcessorUsingYum-Installingthepackage)
-   [Running the
    product](#InstallingStreamProcessorUsingYum-Runningtheproduct)

## Prerequisites

wget needs to be pre-installed.

## Installing the package

To install the package, follow the steps below:

1.  To generate the repo file, issue the following command.

    ``` text
        wget https://bintray.com/wso2/rpm/rpm -O bintray-wso2-rpm.repo
        sudo mv bintray-wso2-rpm.repo /etc/yum.repos.d/
    ```

2.  To install the package, issue the following command.

    ``` text
            yum update
            yum install wso2sp-4.3.0.x86_64
    ```

## Running the product

The following subsections provide instructions to run each runtime of 
WSO2 Stream Processor. Access the URLs via your favorite web browser by
using the following credentials:

-   **Username** : `          admin         `
-   **Password** : `          admin         `

#### Dashboard profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-dashboard        `

Once the dashboard profile starts, access the required component via
your favourite browser. The URLs for the components within this profile
are as follows.

| Component        | URL                                                                |
|------------------|--------------------------------------------------------------------|
| Dashboard Portal | `              https://localhost:9643/portal             `         |
| Business Rules   | `              https://localhost:9643/business-rules             ` |
| Status Dashboard | `              https://localhost:9643/monitoring             `     |
| Policies         | `              https://localhost:9643/policies             `       |

####  Editor profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-editor        `

Once the dashboard profile starts, access the required component via
your favourite browser. The URLs for the components within this profile
are as follows.

| Component       | URL                                                                 |
|-----------------|---------------------------------------------------------------------|
| Editor          | `              https://localhost:9390/editor             `          |
| Template Editor | `              https://localhost:9390/template-editor             ` |

  

#### Manager profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-manager        `  

#### Worker profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-worker        `  
