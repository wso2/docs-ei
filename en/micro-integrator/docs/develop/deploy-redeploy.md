# Deploy/Redeploy Artifacts

## Deploying in embedded Micro Integrator
Once you have build and run your integration artifacts in the Micro Integrator, you can later redeploy modified artifacts:

1. Go to the **Servers** tab and select the running Micro Integrator instance.
2. Right-click and click **Add/Deploy**. 
3. In the dialog that opens, you can select the new/modified CAR files that you want to deploy in the running server.

## Deploying in WSO2 Micro Integrator

The light-weight Micro Integrator is already included in your WSO2 Integration Studio package, which allows you to [deploy and run the artifacts instantly](#testing-build-and-run-the-integration). 

However, when your solutions are ready to be moved to your production environments, it is recommended to [use a CICD pipeline](../develop/using-cicd-pipeline.md). Alternatively, you can [export your packaged artifacts](#exporting-packaged-synapse-artifacts) and deploy the CAR file in the **MI_HOME/repository/deployment/server/carbonapps/** directory, where **MI_HOME** is the root folder of your Micro Integrator installation.