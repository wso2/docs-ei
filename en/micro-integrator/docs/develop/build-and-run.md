# Testing: Build and run the integration

You can test artifacts by deploying the [packaged artifacts](#packaging-synapse-artifacts) in the built-in Micro
Integrator:

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
5.  If you find errors in your mediation sequence, use the [debugging features](../use-cases/tasks/sequences/debugging-mediation.md)
    to troubleshoot.