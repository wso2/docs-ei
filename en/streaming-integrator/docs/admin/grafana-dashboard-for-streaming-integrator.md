#Monitoring Streaming Integrator using Grafana Dashboard

The Streaming Integrator Grafana Dashboard allows you to monitor the metrics of a single Streaming Integrator instance or a set of instances. This involves monitoring whether all processes of the Siddhi setup are working in a healthy manner, monitoring the current status of a Siddhi instance, and viewing statistics relating to the history of an instance. Both JVM level metrics or Siddhi application level metrics can be viewed from the Streaming Integrator Grafana Dashboard.

##Prometheus Reporter

The Prometheus Reporter publish the metric statistics to a URL while the Prometheus Server automatically scrapes and exposes the metrics collected in the HTTP endpoint by allowing user to view the metrics in a visual representation.

###Configuring Prometheus Reporter

To enable statistics for the Prometheus Reporter, add the following configuration to the Streaming Integrator Configurations YAML file, and pass that during startup. The Streaming Integrator Configuration YAML file is located in `<SI_HOME>/conf/server/deployment.yaml`

``` xml
metrics:
     enabled: true

    metrics.prometheus:
     reporting:
       prometheus:
         - name: prometheus
           enabled: true
           serverURL: "http://localhost:9005"
```
<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>               name              </td>
<td><code>               prometheus              </code></td>
<td>               The name for the Prometheus Reporter              </td>
</tr>
<tr class="even">
<td>               enabled              </td>
<td><code>               true              </code></td>
<td>               Enable the Prometheus Reporter for metrics collection              </td>
</tr>
<tr class="odd">
<td>               serverURL              </td>
<td><code>               "http://localhost:9005"              </code></td>
<td>URL of the HTTP endpoint where the Prometheus Reporter exposes the metrics.</td>
</tr>
</tbody>
</table>

 A sample configuration is as follows.

![Streaming Integrator Deployment Configuration](../images/streaming-integrator-grafana-dashboard/deployment_configuration_sample.png)

###Configuring Siddhi application for metrics collection

To enable Siddhi application level metrics for a Siddhi application, users need to add the `@App:statistics` annotation below the Siddhi application name in the Siddhi file as shown in the example below.

``` xml
@App:name('TestMetrics')
@App:statistics(reporter = 'prometheus')
define stream TestStream (message string);
```
![Siddhi app level configuration](../images/streaming-integrator-grafana-dashboard/app_level_configuration.png)

A sample Siddhi application is as follows.
``` xml
@App:name('TestMetrics')
@App:statistics(reporter = 'prometheus')

@Source(type = 'http', receiver.url = 'http://localhost:8011/productionStream', basic.auth.enabled = 'false',
    @map(type = 'json'))
define stream TestStream (message string);

@sink(type = 'log')
define stream ResultStream(result string);

from TestStream
select message as result
insert into ResultStream;
```
![Sample Siddhi application](../images/streaming-integrator-grafana-dashboard/siddhi_app_sample.png)

Add the Siddhi app to the `<SI_HOME>/deployment/siddhi-files` location.
Send the events to the stream `TestStream` using the curl command by opening a new terminal.

`curl -X POST -d "{\"event\": {\"message\":\"Hello\"}}"  http://localhost:8011/productionStream --header "Content-Type:application/json"`

The terminal displays the following output if the event is successfully sent to the `TestMetrics` app.

![Terminal with events sent](../images/streaming-integrator-grafana-dashboard/siddhi_app_events_terminal.png)

###View statistics

Open a terminal at `<SI_HOME>/bin` and run the Streaming Integrator by executing the command `./server.sh`. The terminal displays the following output if the Prometheus Reporter is started successfully.

``` xml
[2020-01-16 14:47:26,846]  INFO {io.siddhi.distribution.metrics.prometheus.reporter.config.PrometheusReporterConfig} - Creating Prometheus Reporter 'prometheus' for Metrics at 'http://localhost:9005'
[2020-01-16 14:47:26,897]  INFO {io.siddhi.distribution.metrics.prometheus.reporter.impl.PrometheusReporter} - Prometheus Server has successfully connected at http://localhost:9005

```
![Prometheus Reporter running](../images/streaming-integrator-grafana-dashboard/prometheus_reporter_terminal.png)

The terminal displays the following output if the Siddhi app is deployed successfully.

![Siddhi app deployed](../images/streaming-integrator-grafana-dashboard/siddhi_app_deployment.png)

The output metrics collection is displayed in the given URL `localhost:9005` as follows. The HTTP endpoint displays a series of metrics collected from JVM statistics and Siddhi statistics.

![Prometheus Reporter output](../images/streaming-integrator-grafana-dashboard/prometheus_reporter_output.png)

The output when the events are sent to the example app `TestMetrics`. The throughput of the Test Stream increases by 1 as shown below.

![Throughput output](../images/streaming-integrator-grafana-dashboard/throughput_output.png)

###View statistics in Prometheus Server

Install the Prometheus Server using the following [link](https://prometheus.io/docs/prometheus/latest/getting_started/).

Add the Prometheus Reporter Configuration to the `prometheus.yml` file as follows.

``` xml
- job_name: 'PrometheusReporter'
 scrape_interval: "15s"
 static_configs:
   - targets: ['localhost:9005']
```

![Prometheus Server configuration](../images/streaming-integrator-grafana-dashboard/prometheus_server_configuration.png)

The configuration modification is displayed in `status > Configuration` of the Prometheus Server URL `localhost:9090` if the configuration has added successfully.

![Prometheus Server configuration view](../images/streaming-integrator-grafana-dashboard/configuration_view.png)

You can copy, paste and execute the metrics collected in the HTTP endpoint in order to view the graphs of the collected metrics over time as follows.

![Prometheus graph](../images/streaming-integrator-grafana-dashboard/prometheus_server_graph.png)

##Streaming Integrator Grafana Dashboard

The Streaming Integrator Grafana Dashboard monitors and visually represents the overall statistics published to Prometheus for metrics collection. It consists of metrics collected from JVM statistics and Siddhi statistics. The dashboard consists of an overview of overall summary statistics and it links to other sub dashboards which represent server statistics and other Siddhi element statistics in detailed manner.

Install Grafana using the following [link](https://grafana.com/docs/grafana/latest/installation/)

###Create a Prometheus Datasource

You can create a data source for Prometheus to scrape metrics by entering the URL where The Prometheus Server is hosted for the URL field. When creating the data source, enter the name as `PrometheusGrafana` as the dashboard refers to the data source with that name.

Reference to create a [data source](https://grafana.com/docs/grafana/latest/features/datasources/prometheus/) for Prometheus in Grafana.

![Grafana datasource](../images/streaming-integrator-grafana-dashboard/grafana_datasource.png)

###Import the Streaming Integrator Grafana Dashboard

The set of Siddhi Grafana Dashboards can be imported to Grafana Server by importing the dashboard json files using the import option. The dashboard jsons can be downloaded from the following [link](https://drive.google.com/drive/folders/1Q0z51fOVJPY5ZKZ4rTy5FCUVmHQhKamw?usp=sharing). 

![Grafana Dashboard import](../images/streaming-integrator-grafana-dashboard/grafana_dashboard_import.png)

###View Statistics in Dashboard

![Dashboard Overview](../images/streaming-integrator-grafana-dashboard/dashboard_overview.png)

Siddhi Dashboard consists of 10 sub dashboards.

1. Siddhi Server Statistics

2. Siddhi Stream Statistics

3. Siddhi Query Statistics

4. Siddhi Source Statistics

5. Siddhi Sink Statistics

6. Siddhi Table Statistics

7. Siddhi Window Statistics

8. Siddhi Aggregation Statistics

9. Siddhi Trigger Statistics

10. Siddhi On-Demand Query Statistics

###Siddhi Server Statistics

Siddhi Server Statistics Dashboard represents a detailed view of the Server instances active. It also includes the JVM metrics related to the active servers.



The dashboard includes the following components.

1.Servers Up/Down

![Servers up/down](../images/streaming-integrator-grafana-dashboard/active_servers_graph.png)

2.Siddhi App Count

![Siddhi app count](../images/streaming-integrator-grafana-dashboard/siddhi_app_count.png)

3.Server Statistics Summary Table

![Server statistics summary](../images/streaming-integrator-grafana-dashboard/server_statistics_summary.png)

4.Overall Throughput

![Overall throughput](../images/streaming-integrator-grafana-dashboard/overall_throughput_graph.png)

5.System Load Average

![System load average](../images/streaming-integrator-grafana-dashboard/system_load_average_graph.png)

6.CPU Usage

![CPU usage](../images/streaming-integrator-grafana-dashboard/cpu_usage_graph.png)

7.Memory Usage

![Memory usage](../images/streaming-integrator-grafana-dashboard/memory_usage_graph.png)

8.JVM Physical Memory

![JVM physical memory](../images/streaming-integrator-grafana-dashboard/jvm_physical_memory_usage.png)

9.JVM Threads

![JVM threads](../images/streaming-integrator-grafana-dashboard/jvm_threads.png)

10.JVM Class Load

![JVM class load](../images/streaming-integrator-grafana-dashboard/jvm_class_load.png)

11.JVM Swap Space

![JVM swap space](../images/streaming-integrator-grafana-dashboard/jvm_swap_space.png)

###Siddhi Element Statistics

There are 9 other dashboards linked to the Siddhi overall statistics dashboard which represent the elements and their statistics. The Siddhi Grafana Dashboard represents the memory, latency and throughput statistics for the elements as follows.

<table>
<thead>
<tr class="header">
<th>No.</th>
<th>Element</th>
<th>Memory</th>
<th>Latency</th>
<th>Throughput</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>               01              </td>
<td>               Stream              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               02              </td>
<td>               Query              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
<tr class="odd">
<td>               03              </td>
<td>               Source              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               04              </td>
<td>               Sink              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="odd">
<td>               05              </td>
<td>               Source Mapper              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
<tr class="even">
<td>               06              </td>
<td>               Sink Mapper              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
<tr class="odd">
<td>               07              </td>
<td>               Table              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               08              </td>
<td>               Window              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
</tr>
<tr class="odd">
<td>               09              </td>
<td>               Aggregation              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
<td>               ✔️              </td>
</tr>
<tr class="even">
<td>               10              </td>
<td>               Trigger              </td>
<td>               -              </td>
<td>               -              </td>
<td>               ✔️              </td>
</tr>
<tr class="odd">
<td>               11              </td>
<td>               On-Demand Query              </td>
<td>               -              </td>
<td>               ✔️              </td>
<td>               -              </td>
</tr>
</tbody>
</table>

####Siddhi Stream Statistics

![Stream statistics dashboard](../images/streaming-integrator-grafana-dashboard/stream_statistics_dashboard.png)

####Siddhi Query Statistics

![Query statistics dashboard](../images/streaming-integrator-grafana-dashboard/query_statistics_dashboard.png)

####Siddhi Source Statistics

![Source statistics dashboard](../images/streaming-integrator-grafana-dashboard/source_statistics_dashboard.png)

####Siddhi Sink Statistics

![Sink statistics dashboard](../images/streaming-integrator-grafana-dashboard/sink_statistics_dashboard.png)

####Siddhi Table Statistics

![Table statistics dashboard](../images/streaming-integrator-grafana-dashboard/table_statistics_dashboard.png)

####Siddhi Window Statistics

![Window statistics dashboard](../images/streaming-integrator-grafana-dashboard/window_statistics_dashboard.png)

####Siddhi Aggregation Statistics

![Aggregation statistics dashboard](../images/streaming-integrator-grafana-dashboard/aggregation_statistics_dashboard.png)

####Siddhi Trigger Statistics

![Trigger statistics dashboard](../images/streaming-integrator-grafana-dashboard/trigger_statistics_dashboard.png)

####Siddhi On-Demand Query Statistics

![On-Demand Query statistics dashboard](../images/streaming-integrator-grafana-dashboard/on_demand_query_statistics_dashboard.png)

###Dashboard Filters

The dashboard allows filtering based on the active server instances and the Siddhi applications running using the dropdowns on the top of the dashboard.

![Dashboard filter](../images/streaming-integrator-grafana-dashboard/dashboard_filters.png)

The filtered dashboard is as follows.

![Filtered dashboard](../images/streaming-integrator-grafana-dashboard/filtered_dashboard.png)
	