# Using a Remote Micro Integrator

The light-weight Micro Integrator is already included in your WSO2 Integration Studio package, which allows you to [deploy and run the artifacts instantly](using-embedded-micro-integrator.md). 

The following instructions can be used to run your artifacts in a remote Micro Integrator instance.

## Deploy and run artifacts in a remote instance

1.	[Download and install](../../setup/installation/install_in_vm) the Micro Integrator server and on your computer. 
2.	[Export your packaged artifacts](packaging-artifacts.md) from WSO2 Integration Studio.
3.	Copy the exported CAR file to the `<MI_HOME>/repository/deployment/server/carbonapps/` folder where <MI_HOME> is the root folder of your Micro Integrator installation.

However, when your solutions are ready to be moved to your production environments, it is recommended to use a **CICD pipeline**.

## Redeploy artifacts in a remote instance

Hot deployment is enabled in the Micro Integrator by default. This allows you to redeploy artifacts without restarting the server. You can simply copy a new export of the packaged artifacts (CAR file) to the deployment directory (`<MI_HOME>/repository/deployment/server/carbonapps/` folder).

Note that if you have applied changes to the server configurations and libraries, the server will restart.

## Disable graceful shutdown (Only for testing)

By default, the graceful shutdown capability is enabled in the Micro Integrator distribution. This means that the server will not immediately shut down when there are incomplete HTTP messaging transactions that are still active. These are transactions that are processed by the HTTP/S passthru transport.

For example, consider a delay in receiving the response from the backend (which should be returned to the messaging client). Because graceful shutdown is enabled, the Micro Integrator will wait until the time specified by the following parameter in the server configuration file (`deployment.toml` file) is exceeded before shutting down.

```toml
[transport.http]
socket_timeout = 180000
```

You can disable this feature by using the following system property when you start the server:

!!! Warning
	Disabling graceful shutdown is only recommended for a development environment for the purpose of making the development and testing process faster. Be sure to have graceful shutdown enabled when you move to production.

```bash
-DgracefulShutdown=false
``` 
