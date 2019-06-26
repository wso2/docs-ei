# Monitoring the ESB Profile with Prometheus

Prometheus is an open source toolkit that can monitor systems and
produce useful information such as graphs and alerts. It collects
statistical data exposed over an HTTP endpoint in the form of multi
dimensional time series data, which can be then be visualized and
queried. For more information about Prometheus, see [Prometheus
Documentation](http://prometheus%202) .

-   [Overview](#MonitoringtheESBProfilewithPrometheus-Overview)
-   [Configuring the Prometheus
    server](#MonitoringtheESBProfilewithPrometheus-ConfiguringthePrometheusserver)
-   [Starting the ESB
    server](#MonitoringtheESBProfilewithPrometheus-StartingtheESBserver)
-   [Viewing
    statistics](#MonitoringtheESBProfilewithPrometheus-Viewingstatistics)

### Overview

The HTTP endpoint exposing the metric data is a service exposed by an
internal API, bundled as an OSGi component and added as a feature to the
WSO2 EI product. WSO2 ESB exposes its statistical data through JMX as
MBeans. The Prometheus publisher in WSO2 EI scrapes these bean data, and
converts them to the Prometheus format. The converted metrics are then
exposed through an HTTP endpoint, which is used by Prometheus to scrape
the statistical data.

![](attachments/119135424/119135461.png){width="878"}  

### Configuring the Prometheus server

1.  [Download and install](https://prometheus.io/download/) Prometheus.
2.  Open the `           <PROMETHEUS_HOME>/prometheus.yml          `
    file. Add a **scrape config** as to this file as shown below. The
    port number and the endpoint name should be as specified below.

    ``` xml
        scrape_configs:
         - job_name: "esb_stats"
           static_configs:
             - targets: ['localhost:9191']
           metrics_path: "metric-service/metrics"
    ```

3.  Save the configuration file.
4.  To start the Prometheus server, navigate to the
    `          <PROMETHEUS_HOME>         ` and execute the start-up
    script located there.

### Starting the ESB server

To start the ESB server with Prometheus enabled, navigate to the
`         <EI_HOME>/bin        ` directory and issue one of the
following commands.

-   For Windows:
    `          integrator.bat -DenablePrometheusApi         `
-   For Linux :
    `          ./integrator.sh -DenablePrometheusApi         `

### Viewing statistics

The stats can be viewed in following urls.

``` java
    http://localhost:9191/metric-service/metrics
```

You may also visit following url in Prometheus server to plot the
graphs.

``` java
    http://localhost:9191/graph
```
