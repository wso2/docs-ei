
# Installing WSO2 EI on Kubernetes

Kubernetes is an open source container orchestration system for
automating, scaling, and managing your application deployments. To run
your Micro Integrator solutions on Kubernetes, you need to first create
an **immutable** docker image with the required synapse artifacts,
configurations, and third-party dependencies by using the base Docker
image of WSO2 Micro Integrator as explained
[above](#InstallingWSO2MicroIntegrator-RunningtheMicroIntegratoroncontainers)
. You can then use the immutable Docker image to create pods and to
configure the kubernetes deployments according to your requirements.

Follow the [Quick Start
Guide](https://docs.wso2.com/display/EI650/Quick+Start+with+WSO2+Micro+Integrator)
to run a simple use case on a Kubernetes cluster.