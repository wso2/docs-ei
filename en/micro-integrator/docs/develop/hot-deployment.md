# Hot Deploying Artifacts

Hot deployment is the process of dynamically deploying your synapse artifacts (XML), dataservices(DBS), carbon applications (CAR), etc., in your Micro Integrator. That is, it is not required to restart the server for the artifact deployment to be effective.

Hot deployment is useful for testing your integration artifacts **in a VM environment**. Because you don't have to restart the server every time you deploy an artifact, the testing time will be shorter and efficient.

!!! Warning
    Do not enable hot deployment when the Readiness Probe is enabled for the server. The Readiness Probe verifies the readiness of the server during server startup. That is, if there are faulty artifacts deployed in the server, the Readiness Probe should be allowed to identify them during server startup. However, if hot deployment is enabled, faulty artifacts can be deployed in the server during the server runtime, which prevents the readiness probe from functioning accurately.  

Be sure to disable hot deployment in the server if you want the readiness probe to work as expected.

## Enabling hot deployment
Open the `deployment.toml` file from the `<MI_HOME>/conf` directory and add the following configuration parameter to enable hot deployment.

```toml
[server]
hot_deployment = true
```

See the [complete list of server configurations](../../references/synapse-properties/config-catalog).
