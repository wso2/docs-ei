# Step 8: Monitor Statistics

This step shows how you can monitor the CDC and file statistics of the WSO2 Streaming Integrator deployment you started and the `SweetFactoryApp` Siddhi application you created and deployed in the previous steps.

!!! tip "Before you begin:"
    As instructed in [# Step 1: Download Streaming Integrator and Dependencies](download-install-and-start-si.md), you need to first start the Prometheus server and then the Grafana.
    
1. To enable the `SweetFactoryApp` Siddhi application to publish statistics to Prometheus, add the `@App:statistics(reporter = 'prometheus')` annotation to it below the `@App:name` annotation as shown below:

    ```text
        @App:name('SweetFactoryApp')
        @App:statistics(reporter = 'prometheus')
    ```

2. Access Grafana via `http://localhost:3000/.`



    

