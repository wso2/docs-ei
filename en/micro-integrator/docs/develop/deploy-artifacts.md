# Deploying Artifacts

Once you have your integration artifacts developed and [packaged in a composite exporter](../packaging-artifacts), you can deploy the composite exporter in your Micro Integrator server or your container environment.

## Deploy artifacts in the embedded Micro Integrator

The light-weight Micro Integrator is already included in your WSO2 Integration Studio package, which allows you to deploy and run the artifacts instantly.

See the instructions in [using the embedded Micro Integrator](../using-embedded-micro-integrator) of WSO2 Integration Studio. 

## Deploy artifacts in a remote Micro Integrator instance

Download and set up a Micro Integrator server in your VM and deploy the composite exporter with your integration artifacts. 

-	**Using Integration Studio**

    See the instructions in [using a remote Micro Integrator](../using-remote-micro-integrator).

-	**Using maven car deployment plugin**

    Note : "This capability is released as a product update on 24/05/2021. If you don't already have this update, you can [get the latest updates](https://updates.docs.wso2.com/en/latest/updates/overview/#!) now.

    Using maven-car-deploy-plugin 5.2.36 or higher and with latest wum updated wso2mi 1.2.0 can be used to deploy/undeploy .car files. Navigate to Composite Application Project's pom.xml file and add below plugin config.
    
    ```
    <plugin>
       <groupId>org.wso2.maven</groupId>
       <artifactId>maven-car-deploy-plugin</artifactId>
       <version>5.2.36</version>
       <extensions>true</extensions>
       <configuration>
          <carbonServers>
              <CarbonServer>
                  <trustStorePath>path_to_wso2carbon.jks</trustStorePath>
                  <trustStorePassword>wso2carbon</trustStorePassword>
                  <trustStoreType>JKS</trustStoreType>
                  <serverUrl>https://{remote_server_host}:{port}</serverUrl>
                  <userName>admin</userName>
                  <password>admin</password>
                  <operation>deploy</operation>
                  <serverType>mi</serverType>
              </CarbonServer>
            </carbonServers>
        </configuration>
    </plugin>
    
    ```
    
    Note: operation can be deploy or undeploy and provide the config values accordingly
    
    !!! Note
        `<serverType>` property is used to explicitly mention the server that your integration artifacts are getting deployed to. The default value is 'mi'

    Then run mvn clean deploy -Dmaven.deploy.skip=true -Dmaven.car.deploy.skip=false

## Deploy artifacts in Docker

Use the <b>Docker Exporter</b> module in WSO2 Integration Studio to build a Docker image of your Micro Integrator solution and push it to your Docker registry. You can then use this Docker image from your Docker registry to start Docker containers.

See the instructions on using the [Docker Exporter](../create-docker-project).

## Deploy artifacts in Kubernetes

Use the <b>Kubernetes Exporter</b> module in WSO2 Integration Studio to deploy a Docker image of your Micro Integrator Solution in a Kubernetes environment. 

See the instructions on using the [Kubernetes Exporter](../create-kubernetes-project).
