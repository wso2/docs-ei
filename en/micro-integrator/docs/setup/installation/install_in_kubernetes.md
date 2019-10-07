
# Installing WSO2 Micro Integrator on Kubernetes

Kubernetes is an open source container orchestration system for
automating, scaling, and managing your application deployments. To run
your Micro Integrator solutions on Kubernetes, you need to first create
an **immutable** docker image with the required synapse artifacts,
configurations, and third-party dependencies by using the base Docker
image of WSO2 Micro Integrator. You can then use the immutable Docker image to create pods and to
configure the kubernetes deployments according to your requirements.

Follow the [Quick Start Guide](../../overview/quick-start-guide.md) to run a simple use case on a Kubernetes cluster.