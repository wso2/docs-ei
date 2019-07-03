# Installing Stream Processor 4.3.0 Using Vagrant

This topic provides instructions on how to install WSO2 Stream Processor
4.3.0 using Vagrant. You can install the product with updates or install
the WSO2 Stream Processor 4.3.0 general availability release (that does
not include updates).  

-   [**Vagrant Resources for WSO2 Stream Processor with
    Updates**](#3e62e91d5bb54ca5a44723de2492ed0b)
-   [**Vagrant Resources for General Availability
    Releases**](#283301f4684a4fa69c584c441650eeaa)

!!! tip

Before you begin:

The system requirements are as follows:

-   3 GHz Dual-core Xeon/Opteron (or latest)

-   16 GB RAM

-   10 GB free disk space


To install WSO2 Stream Processor with updates using Vagrant, follow the
steps below:

1.  Install Vagrant by following the [Vagrant installation
    guide](https://www.vagrantup.com/docs/installation/) .
2.  Install VirtualBox by following the [VirtualBox installation
    guide](https://www.virtualbox.org/wiki/Downloads) .
3.  Clone WSO2 Stream Processor Vagrant git repository and switch to the
    relevant resource directory by executing the following commands.

    ``` java
        git clone https://github.com/wso2/vagrant-sp
        cd vagrant-sp
    ```

4.  Execute the following Vagrant command to start the selected
    deployment.

    ``` java
            Vagrant --updates up
    ```

5.  Once the deployment is started, access the Editor using the
    following URL.

    ``` java
            https://localhost:9743/editor
    ```

    You can view the Stream Processor samples as well as implement new
    Siddhi applications using the Editor. These Siddhi applications can
    then be imported and deployed in the worker node. Follow [this
    document](https://docs.wso2.com/display/SP430/Exporting+a+Siddhi+File)
    for a detailed guide on how to export a Siddhi application. [This
    document](https://docs.wso2.com/display/SP430/Deploying+Streaming+Applications)
    provides details on deploying a Siddhi application using the Stream
    Processor Rest API.

    Use the worker node URL shown below when using the Stream Processor
    Rest API.

    ``` java
            https://localhost:9443/siddhi-apps
    ```

    Access the Stream Processor Dashboard via the following URL.

    ``` java
            https://localhost:9643/monitoring/
    ```

    Access the servers using the following credentials.

    ``` java
            Username: admin
            Password: admin
    ```

  

!!! tip

Before you begin:

The system requirements are as follows:

-   3 GHz Dual-core Xeon/Opteron (or latest)

-   16 GB RAM

-   10 GB free disk space


To install WSO2 Stream Processor without updates using Vagrant, follow
the steps below:

1.  Install Vagrant by following the [Vagrant installation
    guide](https://www.vagrantup.com/docs/installation/) .
2.  Install VirtualBox by following the [VirtualBox installation
    guide](https://www.virtualbox.org/wiki/Downloads) .
3.  Clone WSO2 Stream Processor Vagrant git repository and switch to the
    relevant resource directory by executing the following commands.

    ``` java
        git clone https://github.com/wso2/vagrant-sp
        cd vagrant-sp
    ```

4.  Execute the following Vagrant command to start the selected
    deployment.

    ``` java
            Vagrant --updates up
    ```

5.  Once the deployment is started, access the Editor using the
    following URL.

    ``` java
            https://localhost:9743/editor
    ```

    You can view the Stream Processor samples as well as implement new
    Siddhi applications using the Editor. These Siddhi applications can
    then be imported and deployed in the worker node. Follow [this
    document](https://docs.wso2.com/display/SP430/Exporting+a+Siddhi+File)
    for a detailed guide on how to export a Siddhi application. [This
    document](https://docs.wso2.com/display/SP430/Deploying+Streaming+Applications)
    provides details on deploying a Siddhi application using the Stream
    Processor Rest API.

    Use the worker node URL shown below when using the Stream Processor
    Rest API.

    ``` java
            https://localhost:9443/siddhi-apps
    ```

    Access the Stream Processor Dashboard via the following URL.

    ``` java
            https://localhost:9643/monitoring/
    ```

    Access the servers using the following credentials.

    ``` java
            Username: admin
            Password: admin
    ```

  

  

  
