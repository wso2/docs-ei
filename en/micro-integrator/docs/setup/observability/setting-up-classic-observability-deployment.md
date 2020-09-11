# Setting Up the Classic Observability Deployment

This section provides you instructions to set up the Classic Observability Deployment Solution. This is one of the two observability solutions offered by WSO2 EI. This solution is more suitable if you already have an observability stack such as ELK,  or if you want more business analytics and less operational observability. For morer information about the observability solution, see [Observability Overview](../../administer-and-observe/observability-overview.md).

## System requirements

- For EI nodes, see [Installation Prerequisites](../../setup/installation/install_prerequisites).

- For the Analytics worker:

    <table>
    <tbody>
    <tr class="odd">
    <th>Memory</th>
    <td><p><ul><li>~ 2 GB per worker node (and therefore, 4 GB for the recommended Minimum HA cluster<li>2 CPU cores minimum. It is recommended to allocate 4 cores.</li></li><li>~ 2 GB is the initial heap (-Xms)  required for the server startup. The maximum heap size is 4 GB (-Xmx)</li></ul></p></td>
    </tr>
    <tr class="even">
    <th>Disk</th>
    <td><p><li>~ 480 MB, excluding space allocated for log files and databases.</li></p></td>
    </tr>
    </tbody>
    </table>

- For the Analytics Dashboard:

    <table>
    <tbody>
    <tr class="odd">
    <th>Memory</th>
    <td><p><ul><li>~ 2 GB minimum, 4 GB Maximum<li>2 CPU cores minimum. It is recommended to allocate 4 cores.</li></li><li>~ 512 MB heap size. This is generally sufficient to process typical SOAP messages but the requirements vary with larger message sizes and  the number of messages processed concurrently.</li></ul></p></td>
    </tr>
    <tr class="even">
    <th>Disk</th>
    <td><p><li>~ 480 MB, excluding space allocated for log files and databases.</li></p></td>
    </tr>
    </tbody>
    </table>
    

## Setting up the Analytics Dashboard

### Configuring the Micro Integrator

To enable statistics monitoring for the Micro Integrator, add the following parameters in the `deployment.toml` file of your Micro Integrator. This file is stored in the `MI_HOME/conf`.

!!! Tip
    When you run the embedded Micro Integrator of [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio), the `MI_HOME/conf` directory is as follows: `MI_TOOLING_HOME/Contents/Eclipse/runtime/microesb/conf/` (in MacOS/Linux/CentOS) or `MI_TOOLING_HOME/runtime/microesb/conf` (in Windows) directory. 

```toml
[mediation]
flow.statistics.enable=true
stat.tracer.collect_payloads=true
stat.tracer.collect_mediation_properties=true
```

### Enabling statistics for artifacts

You must enable statistics/tracing for the integration artifacts that you wish to monitor.

#### Enabling statistics for ALL artifacts

If you want to collect statistics for **all** your integration artifacts, be sure to add the following parameter to the `deployment.toml` file in addition the [parameters explained above](#configuring-the-micro-integrator):

```toml
[mediation]
flow.statistics.capture_all=true
```

Alternatively, you can enable statistics for selected artifacts as explained below.

#### Enabling statistics for specific artifacts

Let's use the integration artifacts from the [service chaining](../../use-cases/tutorials/exposing-several-services-as-a-single-service) tutorial.

!!! Warning
    It is not recommended to enable **tracing** in production environments as it generates a large number of events that reduces the performance of the analytics profile. Therefore, tracing should only be enabled in development environments.

If you did not try the [service chaining](../../use-cases/tutorials/exposing-several-services-as-a-single-service) tutorial yet:

1.  Download the [pre-packaged project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/Integration-Tutorial-Artifacts-EI7.1.0/service-orchestration-tutorial.zip) for the **service chaining** use case.
2.  [Open WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) and [import the pre-packaged project](../../develop/importing-projects).

Follow the steps below to enable statistics and tracing for the **REST API** artifact:

1.  Select `HealthcareAPI` in the canvas of WSO2 Integration Studio to open the **Properties** tab.
2.  Select **Statistics Enabled** and (if required) **Trace Enabled** as shown below.
     ![rest api properties](../assets/img/ei-analytics/restapi-properties.png)

Follow the steps below to enable statistics for the **endpoint** artifacts:

1.  Select the required endpoint artifacts from the project explorer. 
2.  Select **Statistics Enabled** and (if required) **Trace Enabled** as shown below.
     ![endpoint properties](../assets/img/ei-analytics/endpoint-properties.png)