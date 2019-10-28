
# Run WSO2 Micro Integrator on Kubernetes

Kubernetes is an open source container orchestration system for
automating, scaling, and managing your application deployments. To run
your Micro Integrator solutions on Kubernetes, you need to first create
an **immutable** docker image with the required synapse artifacts,
configurations, and third-party dependencies by using the base Docker
image of WSO2 Micro Integrator. You can then use the immutable Docker image to create pods and to
configure the kubernetes deployments according to your requirements.

Let's create a **Kubernetes project** for an integration solution and run it on a Kubernetes cluster. You will use [WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio) and the [base Docker image](../../../setup/installation/run_in_docker/#base-docker-images) of the Micro Integrator for this process.

1. [Create a proxy service](../../../develop/creating-artifacts/creating-a-proxy-service) with the following configuration, and package it in a [composite application](../../../develop/packaging-artifacts).

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

2. Now, we can [create a kubernetes project](../../develop/create-kubernetes-project.md) for your solution. The Docker image is now pushed to the docker repository. 

3. Now, we can deploy the created kubernetes project to a kubernetes cluster you have created. You can follow the guide on [kubernetes deployment](../../setup/deployment/kubernetes_deployment.md).

