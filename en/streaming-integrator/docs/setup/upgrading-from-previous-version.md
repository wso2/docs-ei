# Upgrading from Streaming Integrator 1.0.1

To upgrade from Streaming Integrator 1.0.1 to Streaming Integrator 1.1.0, follow the steps below:

!!! tip "Before you begin:"
    Download Streaming Integrator 1.1.0 version from the [Streaming Integrator Page](https://wso2.com/integration/streaming-integrator/)

## Step 1: Deploy the Siddhi applications

To deploy the Siddhi applications you have been running in Streaming Integrator 1.0.1 to Streaming Integrator 1.1.0, follow the procedure below:

1. Open the `<SI 1.0.1_HOME>/wso2/server/deployment/siddhi-files` directory. Then copy all the siddhi files in it.

2. Paste all the Siddhi files that you copied in the `<SI 1.1.0_HOME>/wso2/server/deployment/siddhi-files` directory.

Now your Siddhi applications are deployed in Streaming Integrator 1.1.0.

## Step 2: Update configuration files

To configure Streaming Integrator 1.1.0 the same way as Streaming Integrator 1.0.1, open the `<SI 1.0.1_HOME>/conf/server/deployment.yaml` file. Then read each line, and update the `<SI 1.1.0_HOME>/conf/server/deployment.yaml` file with the same values

!!! note
    The deployment.yaml files must not be copied directly between servers due to certain differences in the parameters included in the two Streaming Integrator versions.

## Step 3: Start the SI 1.1.0 server and install required 

The purpose of this step is to Start the Streaming Integrator and identify any further reqirements to run the Siddhi applications that are deployed in it.

1. Navigate to the `<SI 1.1.0_HOME>/bin` directory and issue the appropriate command based on your operating system:

    - **For Windows**     : `server.bat`
    - **For Linux/MacOS** :`./server.sh`
   
2. To install all the missing extensions that are required to run the Siddhi applications currently deployed, navigate to the `<SI 1.1.0_HOME>/bin` directory and issue the appropriate command based on your operating system:

    - **For Windows**     : `extension-installer.bat install`
    - **For Linux/MacOS** : `./extension-installer.sh install` 
    
    As a result, the following message is logged.
    
    ![Not-installed extensions in Siddhi applications](../images/downloading-and-installing-siddhi-extensions/not-installed-but-used-extensions.png)
    
    If you enter `y` to specify that you want to proceed with the installation, the following message appears to inform you of the status of the installation and to prompt you to restart the WSO2 Streaming Integrator server once the installation is complete.
    
    ![installed missing extension](../images/downloading-and-installing-siddhi-extensions/installed-missing-extension-message.png)

## Step 4: Test the migration

To test the migration, simulate events for the Siddhi applications you have deployed and verify whether they generate the expected results. For instructions to simulate events, see [Testing Siddhi Applications](../develop/testing-a-Siddhi-Application.md). 

