# Installing via the Binary

Follow the steps given below to install the WSO2 Micro Integrator runtime and its monitoring Dashboard by using the <b>binary distribution</b> of WSO2 Enterprise Integrator.

## Download and install

Go to the WSO2 Enterprise Integrator [product page](https://wso2.com/integration/#), click **Download**, and then cick **Binary** to download the product distribution as a ZIP file.

Extract the download ZIP file to a location on your computer. 
-	The <b>micro-integrator</b> folder inside the extracted ZIP file will be your <b>MI_HOME</b> directory.
-	The <b>micro-integrator-dashboard</b> folder inside the extracted ZIP file will be your <b>DASHBOARD_HOME</b> directory.

## Setting the Java_Home

Set up a [JDK that is compatible with WSO2 Enterprise Integrator](https://docs.wso2.com/display/compatibility/Tested+Operating+Systems+and+JDKs) and point the `java_home` variable to your JDK instance.

## Starting the MI server

1.  Before you execute the product startup script, be sure to set the
    JAVA HOME in your machine.
2.  Open a terminal and navigate to the `MI_HOME/bin/` directory, where `MI_HOME` is the home directory of the distribution you downloaded.
3.  Execute the relevant command:

    ```bash tab='On MacOS/Linux/CentOS'
    sh micro-integrator.sh
    ```
    
    ```bash tab='On Windows'
    micro-integrator.bat
    ```
      
By default, the HTTP listener port is 8290 and the default HTTPS listener port is 8253.

## Stopping the MI server

To stop the Micro Integrator runtime, press Ctrl+C in the command window.
