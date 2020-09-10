# Observability Solutions

Observability can be viewed as a superset of monitoring where monitoring is enriched with capabilities to perform debugging and profiling through rich context, log analysis, correlation, and tracing. Modern day observability resides on three pillars of **logs**, **metrics**, and **tracing**. Modern businesses require observability systems to self-sufficiently emit their current state(overview), generate alerts for any abnormalities detected to proactively identify failures, and to provide information to find the root causes of a system failure.

WSO2 Enterprise Integrator offers two observability solutions referred to as the cloud-native observability deployment and classic observability deployment.

<img src="../../assets/img/observability/observability-mi.png" title="Observability Solution" width="500" alt="Observability Solution"/>

**Cloud Native Observability Deployment**

This solution is more suitable in the following scenarios:

- If you are creating a new cloud native deployment
- If you have Prometheus, Grafana and Jaeger as you in-house monitoring and observability tools.

The cloud native observability solution can be set up to have any of the following combination of operations.

- Metrics only
- Metrics + Logging
- Metrics + Tracing
- Metrics + Logging + Tracing

**Classic Observability Deployment**

This solution is more suitable in the following scenarios:
    
- If you require more business analytics and less operation observability.    
- If you want a simpler deployment
- If you already have an observability stack such as ELK

The classic observability solution can be setup to have only metrics, and have logging and tracing as add-ons.

## Understanding observability solutions

WSO2 Enterprise Integrator 7.0.0 and older versions offer an analytics distribution that mainly provides business analytics functionality together with a few observability related features. Clients with comprehensive observability requirements had to rely on external tools/stacks such as ELK, Prometheus, AppDynamics, Jaeger, Zipkin, etc. This resulted in multiple scattered systems to observe the system where debugging and troubleshooting were not sufficiently stream-lined.

To address that limitation, WSO2 Enterprise Integrator 7.1.0 introduced an observability solution that utilizes a selected set of external tools together with the older analytic distribution intact. This section explains the features and usage of both solutions. 

The older analytics distribution is referred to as the Classic Observability Deployment, and the newer solution introduced with WSO2 Enterprise Integrator 7.1.0 is referred to as the Cloud Native Observability Deployment.

## What's Next

* If you are creating a new cloud-native deployment or if you have Prometheus, Grafana, and Jaeger as your in-house monitoring and observability tools, you can set  up and use the Cloud Native Observability Solution. For more information about this solution, see [Cloud Native Observability Solution](/cloud-native-observability-dashboards.md).

* If you already have an observability stack such as ELK, or if you want more business analytics and less operational observability, it is recommended for you to use the Classic Observability Deployment. For more information about this solution, see [Classic Observability Deployment](/using-the-analytics-dashboard.md).

* For instructions to set up the above observability solutions, see [Setting Up the Observability Solutions](../setup/observability/setting-up-minimum-basic-observability-deployment.md)


