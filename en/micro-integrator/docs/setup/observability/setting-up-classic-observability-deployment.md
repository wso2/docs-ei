# Setting up EI Analytics for Observability

Follow the instructions given below to enable observability for your Micro Integrator deployment using <b>EI Analytics</b>. 

![EI-Analytics Observability](../../assets/img/monitoring-dashboard/classic-observability-architecture.png)

EI Analytics consists of two components: **Server** and **Portal**. The server processes the data streams that are sent from the Micro Integrator and publishes the statistics to a database. The portal reads the statistics published by the worker and displays the statistics. The server and portal are connected through the database.

This solution is more suitable if you already have an observability stack such as ELK,  or if you want more business analytics and less operational observability. To select the most appropriate observability solution for your deployment, see [Observability Deployment Strategy](../../../setup/observability/observability-deployment-strategy).

## System requirements

You will be running three servers (EI Analytics server, EI Analytics portal, and the Micro Integrator) for this solution. Be sure that you have the required system specifications to run each server.

-   For the Analytics **Server**:

    <table>
    <tbody>
    <tr class="odd">
    <th>Memory</th>
    <td><p><ul><li>~ 4 GB per worker node<li>It is recommended to allocate 4 cores.</li></li><li>~ 2 GB is the initial heap (-Xms)  required for the server startup. The maximum heap size is 4 GB (-Xmx)</li></ul></p></td>
    </tr>
    <tr class="even">
    <th>Disk</th>
    <td><p><li>~ 480 MB, excluding space allocated for log files and databases.</li></p></td>
    </tr>
    </tbody>
    </table>

-   For the Analytics **Portal**:

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

-   For the Micro Integrator, see the [installation prerequsites](../../../setup/installation/install_prerequisites).

## Download the servers

-   To download EI Analytics:
    1.  Go to the WSO2 Enterprise Integrator <a href="https://wso2.com/integration/">product page</a>, click <b>Download</b>, and then go to the <b>Other Resources</b> section.
    2.  Click <b>Integration Analytics</b> to download the distribution.

        <img src="../../../assets/img/observability/download-ei-analytics.png">

    !!! Info
        The location of your Analytics installation will be referred to as `<EI_ANALYTICS_HOME>`.

-   Download and [install the Micro Integrator](../../../setup/installation/install_in_vm_installer) of EI 7.1.

## Configuring the Micro Integrator
    
### Enabling statistics monitoring

To enable statistics monitoring for the Micro Integrator, add the following parameters in the `deployment.toml` file of your Micro Integrator. This file is stored in the `MI_HOME/conf`.

```toml
[mediation]
flow.statistics.enable=true
stat.tracer.collect_payloads=true
stat.tracer.collect_mediation_properties=true
```

### Enabling statistics for ALL artifacts

If you want to collect statistics for **all** your integration artifacts, be sure to add the following parameter under the `[mediation]` header in the `deployment.toml` file in addition the [parameters explained above](#configuring-the-micro-integrator):

```toml
flow.statistics.capture_all=true
```

Alternatively, you can enable statistics for selected artifacts as explained below.

### Enabling statistics for specific artifacts

Let's use the integration artifacts from the [service chaining](../../../use-cases/tutorials/exposing-several-services-as-a-single-service) tutorial.

!!! Warning
    It is not recommended to enable **tracing** in production environments as it generates a large number of events that reduces the performance of the analytics profile. Therefore, tracing should only be enabled in development environments.

If you did not try the [service chaining](../../../use-cases/tutorials/exposing-several-services-as-a-single-service) tutorial yet:

1.  Download the [pre-packaged project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/Integration-Tutorial-Artifacts-EI7.1.0/service-orchestration-tutorial.zip) for the **service chaining** use case.
2.  [Open WSO2 Integration Studio](../../../develop/installing-WSO2-Integration-Studio) and [import the pre-packaged project](../../../develop/importing-projects).

Follow the steps below to enable statistics and tracing for the **REST API** artifact:

1.  Select `HealthcareAPI` in the canvas of WSO2 Integration Studio to open the **Properties** tab.
2.  Select **Statistics Enabled** and (if required) **Trace Enabled** as shown below.

    <img src="../../../assets/img/ei-analytics/restapi-properties.png" alt="rest api properties" width="500">

Follow the steps below to enable statistics for the **endpoint** artifacts:

1.  Select the required endpoint artifacts from the project explorer. 
2.  Select **Statistics Enabled** and (if required) **Trace Enabled** as shown below.
     ![endpoint properties](../../assets/img/ei-analytics/endpoint-properties.png)

## What's Next?

If you have successfully set up your anlaytics deployment, see the instructions on [using the analytics portal](../../../administer-and-observe/using-the-analytics-dashboard).
