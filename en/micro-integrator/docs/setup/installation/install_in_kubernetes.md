
# Installing WSO2 Micro Integrator on Kubernetes

Kubernetes is an open source container orchestration system for
automating, scaling, and managing your application deployments. To run
your Micro Integrator solutions on Kubernetes, you need to first create
an **immutable** docker image with the required synapse artifacts,
configurations, and third-party dependencies by using the base Docker
image of WSO2 Micro Integrator. You can then use the immutable Docker image to create pods and to
configure the kubernetes deployments according to your requirements.

Let’s look at the steps to create a simple proxy service using Integration studio and deploy it in kubernetes cluster. 

1. First, we can use following proxy config and create a composite application.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="HelloWorld" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
    <target>
        <inSequence>
            <payloadFactory media-type="json">
                <format>{"Hello":"World"}</format>
                <args/>
            </payloadFactory>
            <respond/>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </target>
</proxy>
```

2. Now we can [create a kubernetes project](../../../docs/develop/create-docker-kubernetes-projects.md) with config following the steps. After the steps as in this guide, the image should be pushed to the docker repository. 

3. Now we can deploy the  created kubernetes project to a kubernetes cluster you have created. You can follow the guide on [kubernetes deployment](../../../docs/develop/kubernetes_deployment.md).

