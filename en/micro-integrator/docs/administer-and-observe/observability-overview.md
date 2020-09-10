!!! note
    This section is still a work in progress and not tested.

# Observe and Manage Overview

This section explains how to set up the observability solutions and perform management tasks for WSO2 Enterprise Integrator.

## Observability

WSO2 Enterprise Integrator offers two observability solutions referred to as the cloud-native observability deployment and classic observability deployment.

**Cloud Native Observability Deployment**: If you are creating a new cloud-native deployment or if you have Prometheus, Grafana, and Jaeger as your in-house monitoring and observability tools, you can set  up and use the Cloud Native Observability Solution. For more information about this solution, see [Cloud Native Observability Solution](/cloud-native-observability-dashboards.md).

**Classic Observability Deployment**: If you already have an observability stack such as ELK, or if you want more business analytics and less operational observability, it is recommended for you to use the Classic Observability Deployment. For more information about this solution, see [Classic Observability Deployment](/observability.md).

## Management

You can monitor and manage various artifacts that you have deployed. The following are the options that enable you to do this.

- **[Micro Integrator Dashboard](/working-with-monitoring-dashboard.md)**: Allows you to perform administration tasks related to your Micro Integrator deployment
- **[Micro Integrator CLI](/using-the-command-line-interface.md)**: Allows you to perform various management and administration tasks from the command line.
- **[Using the Management API](/working-with-management-api.md)**: The Micro Integrator CLI and the Micro Integrator dashboard communicate with this service to obtain administrative information of the server instance and to perform various administration tasks. If you are not using the dashboard or the CLI, you can directly access the resources of the management API

## Integration with external tools

You can integrate with external tools to do the following.

**Monitoring Metrics**

- [JMX Monitoring](/jmx_monitoring.md)
- [SNMP Monitoring](/snmp_monitoring.md)
- [Prometheus for Monitoring](/monitoring_with_prometheus.md)

**TCP Message Monitoring**

- [Starting TCPMon](/tcp/starting_tcp_mon.md)
- [Message Monitoring with TCPMon](/tcp/message_monitoring_with_tcpmon.md)
- [Other Usages of TCPMon](/tcp/other_usages_of_tcpmon.md)
