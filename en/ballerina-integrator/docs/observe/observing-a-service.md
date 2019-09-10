# Observing a Service with Prometheus

There are mainly two systems involved in collecting and visualizing the metrics. [Prometheus](https://prometheus.io/) is used to collect the
metrics from the Ballerina service and [Grafana](https://grafana.com/) can connect to Prometheus and visualize the metrics in the dashboard.

To understand how you can observe Ballerina services, let’s consider a service that converts JSON to XML.

### Create the project structure

Ballerina is a complete programming language that can have any custom project structure that you require. For this example, let's use the following module structure.

```
asynchronous-invocation
    └── guide
        ├── stock_quote_data_backend
        │   ├── stock_backend.bal
        │   └── tests
        │       └── stock_backend_test.bal
        ├── stock_quote_summary_service
        │   ├── async_service.bal
        │   └── tests
        │       └── async_service_test.bal
        └── tests
            └── integration_test.bal
```

- Create the above directories in your local machine and also create empty `.bal` files.

- Then open the terminal and navigate to `asynchronous-invocation/guide` and run Ballerina project initializing toolkit.

```bash
   $ ballerina init
```

### Set up Prometheus

**Step 1:** Create a `prometheus.yml` file in `/tmp/` directory.

**Step 2:** Add the following content to `/tmp/prometheus.yml`.

```yaml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['a.b.c.d:9797']
```

Here the targets `'a.b.c.d:9797'` should contain the host and port of the `/metrics` service that's exposed from 
Ballerina for metrics collection. Add the IP of the host in which the Ballerina service is running as `a.b.c.d` and its
port (default `9797`).

> **Tip**: If you need more information refer [official documentation of Prometheus](https://prometheus.io/docs/introduction/first_steps/).

**Step 3:** Start the Prometheus server in a Docker container with below command.

```bash
$ docker run -p 19090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```
    
**Step 4:** Go to <http://localhost:19090/> and check whether you can see the Prometheus graph.
Ballerina metrics should appear in Prometheus graph's metrics list when Ballerina service is started.

### Set up Grafana

Let’s use [Grafana] to visualize metrics in a dashboard. For this, we need to install Grafana, and configure
Prometheus as a datasource. Follow the below provided steps and configure Grafana.

**Step 1:** Start Grafana as Docker container with below command.

```bash
$ docker run -d --name=grafana -p 3000:3000 grafana/grafana
```
For more information refer [Grafana in Docker Hub](https://hub.docker.com/r/grafana/grafana/).

**Step 2:** Go to <http://localhost:3000/> to access the Grafana dashboard running on Docker.

**Step 3:** Login to the dashboard with default user, username: `admin` and password: `admin`

**Step 4:** Add Prometheus as datasource with `Browser` access configuration as provided below.

![Grafana Prometheus Datasource](images/grafana-prometheus-datasource.png "Grafana Prometheus Datasource")

**Step 5:** Import the Grafana dashboard designed to visualize Ballerina metrics from [https://grafana.com/dashboards/5841](https://grafana.com/dashboards/5841).
This dashboard consists of service and client invocation level metrics in near real-time view. 

Ballerina HTTP Service Metrics Dashboard Panel will be as below.
![Ballerina Service Metrics](images/grafana-ballerina-metrics-1.png "Ballerina Sample Service Metrics Dashboard")

Ballerina HTTP Client Metrics Dashboard Panel will be as below.
![Ballerina Client Metrics](images/grafana-ballerina-metrics-3.png "Ballerina Sample Client Metrics Dashboard")

Ballerina SQL Client Metrics Dashboard Panel will be as below.
![Ballerina SQL Client Metrics](images/grafana-ballerina-metrics-2.png "Ballerina Sample SQL Client Metrics Dashboard") 



### Metrics
Metrics and alarts are built-in with ballerina. We will use Prometheus as the monitoring tool.
Follow the below steps to set up Prometheus and view metrics for Ballerina restful service.

- You can add the following configurations for metrics. Note that these configurations are optional if you already have the basic configuration in `ballerina.conf` as described under `Observability` section.

```ballerina
   [b7a.observability.metrics]
   enabled=true
   reporter="prometheus"

   [b7a.observability.metrics.prometheus]
   port=9797
   host="0.0.0.0"
```

- Create a file `prometheus.yml` inside `/tmp/` location. Add the below configurations to the `prometheus.yml` file.
```
   global:
     scrape_interval:     15s
     evaluation_interval: 15s

   scrape_configs:
     - job_name: prometheus
       static_configs:
         - targets: ['172.17.0.1:9797']
```

   NOTE : Replace `172.17.0.1` if your local Docker IP differs from `172.17.0.1`
   
- Run the Prometheus Docker image using the following command
```
   $ docker run -p 19090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml \
   prom/prometheus
```

- Navigate to `asynchronous-invocation/guide` and run the asynchronous-invocation using the following command
```
  $ ballerina run --config stock_quote_summary_service/ballerina.conf stock_quote_summary_service/
```

- You can access Prometheus at the following URL
```
   http://localhost:19090/
```

NOTE:  Ballerina will by default have following metrics for HTTP server connector. You can enter following expression in Prometheus UI
-  http_requests_total
-  http_response_time


