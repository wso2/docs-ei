# Deploy and run the integration

You can deploy the artifacts and run it in the embedded Micro Integrator, or a remote instance of the Micro Integrator.

## Using the embedded Micro Integrator

You can test artifacts by deploying the [packaged artifacts](packaging-artifacts.md) in the built-in Micro Integrator:

1.  Be sure to create a **composite application project** and include
    your artifacts.
2.  Right-click the composite application project and click **Export
    Project Artifacts and Run** .  
    ![Create docker image](../../assets/img/create_project/testing_export_run.png)
3.  In the dialog that opens, select the artifacts form the composite
    application project that you want to deploy.  
    ![Create docker image](../../assets/img/create_project/testing_artifact_selection.png)
4.  Click **Finish** . The artifacts will be deployed in the WSO2 Micro
    Integrator and the server will start. See the startup log in the
    **Console** tab:  
    ![Create docker image](../../assets/img/create_project/testing_log.png)
5.  If you find errors in your mediation sequence, use the [debugging features](debugging-mediation.md)
    to troubleshoot.

## Using a remote Micro Integrator

The light-weight Micro Integrator is already included in your WSO2 Integration Studio package, which allows you to [deploy and run the artifacts instantly](#using-the-embedded-micro-integrator). 

However, when your solutions are ready to be moved to your production environments, it is recommended to use a **CICD pipeline**. Alternatively, you can [export your packaged artifacts](packaging-artifacts.md) and deploy the CAR file in the `MI_HOME/repository/deployment/server/carbonapps/` directory, where **MI_HOME** is the root folder of your Micro Integrator installation.

## Redeploy artifacts
Once you have build and run your integration artifacts in the Micro Integrator, you can later redeploy modified artifacts:

1. Go to the **Servers** tab and select the running Micro Integrator instance.
2. Right-click and click **Add/Deploy**. 
3. In the dialog that opens, you can select the new/modified CAR files that you want to deploy in the running server.