# Installing Stream Processor Using Brew

The following sections cover how to install and run WSO2 Stream
Processor using Brew.

-   [Installing the
    package](#InstallingStreamProcessorUsingBrew-Installingthepackage)
-   [Running the
    product](#InstallingStreamProcessorUsingBrew-Runningtheproduct)

!!! tip

Before you begin:

The system requirements for installing WSO2 Stream Processor using Brew
is as follows:

-   3 GHz Dual-core Xeon/Opteron (or latest)
-   2 GB RAM


## Installing the package

To install the package, follow the steps below:

1.  Install Brew by following the instructions [here](https://brew.sh/)
    .
2.  Add the WSO2 repository as a tap if you have not already done so by
    issuing the following command.  

    ``` text
        brew tap wso2/wso2
    ```

      
    This enables you to download any WSO2 product via Homebrew.

3.  Install WSO2 SP by issuing the following command.  

    ``` text
            brew install wso2sp-4.3.0
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

| Component        | URL                                                                                             |
|------------------|-------------------------------------------------------------------------------------------------|
| Dashboard Portal | `                             https://localhost:9643/portal                           `         |
| Business Rules   | `                             https://localhost:9643/business-rules                           ` |
| Status Dashboard | `                             https://localhost:9643/monitoring                           `     |
| Policies         | `                             https://localhost:9643/policies                           `       |

####  Editor profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-editor        `

Once the dashboard profile starts, access the required component via
your favourite browser. The URLs for the components within this profile
are as follows.

| Component       | URL                                                                                              |
|-----------------|--------------------------------------------------------------------------------------------------|
| Editor          | `                             https://localhost:9390/editor                           `          |
| Template Editor | `                             https://localhost:9390/template-editor                           ` |

  

#### Manager profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-manager        `  

#### Worker profile

To run this profile, issue the following command in the terminal.

`         wso2sp-<SP_VERSION>-worker        `
