# Hot Deploying Artifacts

Hot deploying in micro integrator allows you to dynamically deploy synapse artifacts(.xml), dataservices(.dbs), carbon applications(.car), etc without restarting the server.

This feature would be helpful mostly during the testing phase in the development environment when the MI is configured to run in a VM environment, as it reduces the overhead of restarting the server for each new synapse artifacts, dss artifacts, or CApp deployment.

**Note:** However as a best practice in Micro Integrator, we are not encouraging to enable both readiness probe and the hot deployment at the same time because it might affect the correctness of the results that the readiness probe will be giving. Readiness probe will give results based on the deployment that takes place only during the server startup time, but when the hot deployment is enabled, there is a chance that the previously failed carbon apps (faulty carbon apps) being valid because of the dynamic deployment behavior. 

Therefore, make sure you disable hot deployment in the server if you want the readiness probe to be worked perfectly as expected.

##Enabling Hot Deployment
You can enable hot deployment in Micro Integrator by configuring the following deployment parameter in the deployment.toml file.

```toml
[server]
hot_deployment = true
```

Please visit [product configuration catalog](../../references/synapse-properties/config-catalog) to see more on the **hot deployment** parameter in Micro Integrator.
