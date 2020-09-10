# Observability Deployment Startegy

WSO2 Enterprise Integrator 7.1 offers two observability solutions for monitoring and managing your integration deployments in the Micro Integrator. You can choose one of the two solutions depending on the nature of your Micro Integrator deployment. 

Given below is a comparison of the two solutions that will help you choose the appropriate observability stategy.

## Cloud-Native Observability Deployment

This solution is more suitable in the following scenarios:

- If you are creating a new cloud-native Micro Integrator deployment. Refer the <a href="../../../setup/deployment/kubernetes_deployment_patterns">Micro Integrator Kubernetes Deployment</a> guide for details on how to setup a cloud-native Micro Integrator deployment on Kubernetes.
- If you already have Prometheus, Grafana, and Jaeger as you in-house monitoring and observability tools.

If you choose this option, see the instructions on <a href="../../../setup/observability/setting-up-minimum-basic-observability-deployment">setting up a cloud-native observability deployment</a>.

## Classic Observability Deployment

This solution is more suitable in the following scenarios:

- If you require more business analytics and less operational observability.
- If you want a simpler deployment.
- If you already have an observability stack such as ELK.

If you choose this option, see the instructions on <a href="../../../setup/observability/setting-up-classic-observability-deployment">setting up a classic observability deployment</a>.

