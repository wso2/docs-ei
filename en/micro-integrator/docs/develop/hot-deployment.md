# Hot Deploying Artifacts

Hot deployment is the process of dynamically deploying your synapse artifacts (XML), dataservices(DBS), carbon applications (CAR), etc., in your Micro Integrator. That is, it is not required to restart the server for the artifact deployment to be effective.

Hot deployment is useful for testing the integration artifacts **in a VM environment**. With hot deployment, no 
server restart is required each time you deploy an artifact and the testing time will be shorter and efficient. Hence this is enabled by default in VM distribution. This is disabled in base docker image to honor the immutable nature in the container world.

!!! Note
    If  Readiness Probe is used with hot deployment, it will not detect the faulty carbon apps. If you want your server 
    to be detected as not ready with faulty applications, make sure to disable the hot deployment.

## Disabling hot deployment
Open the `deployment.toml` file from the `<MI_HOME>/conf` directory and set hot_deployment property to false.

```toml
[server]
hot_deployment = false
```

See the [complete list of server configurations](../../references/config-catalog/#deployment).
