# Step 8: Monitor Statistics

This step shows how you can monitor the CDC and file statistics of the WSO2 Streaming Integrator deployment you started and the `SweetFactoryApp` Siddhi application you created and deployed in the previous steps.

1. Download Prometheus from the [Prometheus site](https://prometheus.io/download/). For instructions, see [the Prometheus Getting Started Guide](https://prometheus.io/docs/prometheus/latest/getting_started/).

2. Extract the downloaded file. The directory that opens as a result is referred to as the `<PROMETHEUS_HOME>` from here on.

3. To enable statistics for the Prometheus reporter, open the `<SI_HOME>/conf/server/deployment.yaml` file and set the `enabled` parameter in the `wso2.metrics` section to `true`, and update the other parameters in the section as shown below. You also need to add the `metrics.prometheus:` as shown.

    ```
     wso2.metrics:
       # Enable Metrics
       enabled: true
       reporting:
         console:
           - # The name for the Console Reporter
             name: Console
    
             # Enable Console Reporter
             enabled: false
    
             # Polling Period in seconds.
             # This is the period for polling metrics from the metric registry and printing in the console
             pollingPeriod: 2
    
     metrics.prometheus:
      reporting:
        prometheus:
          - name: prometheus
            enabled: true
            serverURL: "http://localhost:9005"
    ```
   
4. Open the `<PROMETHEUS_HOME>/prometheus.yml` file and add the following configuration in the `scrape_configs:` section.

    ```
     scrape_configs:
       - job_name: 'prometheus'
         static_configs:
         - targets: ['localhost:9005']
    ```
   
5. In the terminal, navigate to the `<PROMETHEUS_HOME` and issue the following command to start the Prometheus server.

    `./prometheus`
    
!!! info
    The above steps to configure and start Prometheus need to be performed before you start the Grafana server.
    
## Downloading the required dashboards



    

