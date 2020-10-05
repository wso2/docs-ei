# Step 4: Extend the Streaming Integrator

The Streaming Integrator is by default shipped with most of the available Siddhi extensions by default. If a Siddhi extension you require is not shipped by default, you can download and install it via the Extension Installer tool.

To check the extensions you need in your use case and install them, follow the steps below:

1. Be sure that the WSO2 Streaming Integrator server is started.

2. Navigate to the `<SI_HOME>bin` directory and issue the following command.

    - **For Linux**: `./extension-installer.sh install`<br/>
    - **For Windows**: `extension-installer.bat install`
    
    Enter `Y` when you are prompted to confirm whether WSO2 Streaming Integrator should proceed to install the extensions listed.
    
    When the extension is successfully installed, the following is displayed in the log.
    
    ```
    Installation completed with status: INSTALLED. Please restart the server.
    ```
3. You can also install a selected installment individually. To try this out, let's issue the following command from the `<SI_HOME>bin` directory and install the `kafka` extension that you will be using later in this scenario.<br/><br/>

    `./extension-installer.sh install install kafka`
   
4. Restart the Streaming Integrator server as instructed.

## What's Next?

Now you can run the `SweetFactoryApp` Siddhi application in a production environment and see whether it functions as expected. To do this, proceed to [Step 5: Test the Siddhi Application](test-siddhi-application.md).
