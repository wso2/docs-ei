#K8s-ei-operator Example 3

## JMS Sender/Receiver Scenario

Let's define a JMS (sender and receiver) scenario using WSO2 Micro Integrator and deploy it on your Kubernetes environment.

Follow the steps given below to deploy and run the integration solution on Kubernetes.

1.  Create a Maven Multi Module Project using WSO2 Integration Studio.

    ![Create Maven Multi Module Project](../../../assets/img/create_project/docker_k8s_project/create-maven-project.png) 
    
2.  Create an **ESB Config Project** inside the Maven Multi Module Project.
    **New → Project → ESB Config Project**
    
    ![Create ESB Config Project](../../../assets/img/create_project/docker_k8s_project/esb-config.png) 
    
3.  Add the following proxy service configuration to your ESB Config Project. This service listens to messages from ActiveMQ and publishes to another queue in ActiveMQ.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml version="1.0" encoding="UTF-8"?>
    <proxy name="JmsSenderListner" startOnLoad="true" transports="jms" xmlns="http://ws.apache.org/ns/synapse">
        <target>
            <inSequence>
                <send>
                    <endpoint>
                        <address uri="jms:/secondQueue?transport.jms.ConnectionFactoryJNDIName=QueueConnectionFactory&amp;java.naming.factory.initial=org.apache.activemq.jndi.ActiveMQInitialContextFactory&amp;java.naming.provider.url=failover:(tcp://localhost:61616,tcp://localhost:61617)?randomize=false&amp;transport.jms.DestinationType=queue">
                            <suspendOnFailure>
                                <initialDuration>-1</initialDuration>
                                <progressionFactor>-1</progressionFactor>
                                <maximumDuration>0</maximumDuration>
                            </suspendOnFailure>
                            <markForSuspension>
                                <retriesBeforeSuspension>0</retriesBeforeSuspension>
                            </markForSuspension>
                        </address>
                    </endpoint>
                </send>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </target>
        <parameter name="transport.jms.SessionAcknowledgement">AUTO_ACKNOWLEDGE</parameter>
        <parameter name="transport.jms.Destination">$SYSTEM:destination</parameter>
        <parameter name="transport.jms.ConnectionFactoryType">firstQueue</parameter>
        <parameter name="transport.jms.ContentType">$SYSTEM:contenttype</parameter>
        <parameter name="java.naming.provider.url">$SYSTEM:jmsurl</parameter>
        <parameter name="transport.jms.SessionTransacted">false</parameter>
        <parameter name="transport.jms.ConnectionFactoryJNDIName">$SYSTEM:jmsconfac</parameter>
        <parameter name="transport.jms.UserName">$SYSTEM:jmsuname</parameter>
        <parameter name="transport.jms.Password">$SYSTEM:jmspass</parameter>
    </proxy>
    ```
    **Note**: Update the **tcp://localhost:61616** URL given above with the actual/connecting URL that will be reachable from the Kubernetes pod.
    
4.  Create a **Composite Application Project** inside the Maven Multi Module Project. Be sure to select the above configuration(s) under Dependencies.
    **New → Project → Composite Application Project**
    
    ![Create Composite Application Project](../../../assets/img/create_project/docker_k8s_project/composite-proj.png)    

5.  Create a **Docker/Kubernetes Project** inside the Maven Multi Module Project.
    **New → Project → Docker/Kubernetes Project** and select the **New Kubernetes Project** option. 
    
    ![Create Docker/Kubernetes Project](../../../assets/img/create_project/docker_k8s_project/k8s-proj.png)    

6.  Navigate to the Kubernetes project and open the **pom.xml** file. Select the multiple composite applications you want to add to the docker image under **Dependencies**.

    ![Select composite projects](../../../assets/img/create_project/docker_k8s_project/select-dependency.png) 
     
7.  Uncomment the following two commands in the Dockerfile inside the Kubernetes project.
    ```bash
    COPY Libs/*.jar /home/wso2carbon/wso2mi/lib/
    COPY Conf/* /home/wso2carbon/wso2mi/conf/
    ```
8.  Download [Apache ActiveMQ](http://activemq.apache.org/).

9. Copy the following client libraries from the `<ACTIVEMQ_HOME>/lib` directory to the `<MAVEN_MULTI_MODULE>/<KUBERNETES_PROJECT>/Lib` directory.

    **ActiveMQ 5.8.0 and above** 
    -   activemq-broker-5.8.0.jar
    -   activemq-client-5.8.0.jar
    -   activemq-kahadb-store-5.8.0.jar  
    -   geronimo-jms_1.1_spec-1.1.1.jar
    -   geronimo-j2ee-management_1.1_spec-1.0.1.jar
    -   geronimo-jta_1.0.1B_spec-1.0.1.jar
    -   hawtbuf-1.9.jar
    -   Slf4j-api-1.6.6.jar
    -   activeio-core-3.1.4.jar (available in the <ACTIVEMQ_HOME>/lib/optional directory)  
     
    **Earlier version of ActiveMQ**
    -   activemq-core-5.5.1.jar
    -   geronimo-j2ee-management_1.0_spec-1.0.jar    
    -   geronimo-jms_1.1_spec-1.1.1.jar
    
10. Create a file named `deployment.toml` inside the` <MAVEN_MULTI_MODULE>/<KUBERNETES_PROJECT>/Conf` directory and add the following content:
    ```toml
    [server]
    hostname = "localhost"
    
    [keystore.tls]
    file_name = "wso2carbon.jks"
    password = "wso2carbon"
    alias = "wso2carbon"
    key_password = "wso2carbon"
    
    [truststore]
    file_name = "client-truststore.jks"
    password = "wso2carbon"
    alias = "symmetric.key.value"
    algorithm = "AES"
    
    [[transport.jms.listener]]
    name = "default"
    parameter.initial_naming_factory = "org.apache.activemq.jndi.ActiveMQInitialContextFactory"
    parameter.provider_url = "tcp://localhost:61616"
    parameter.connection_factory_name = "QueueConnectionFactory"
    parameter.connection_factory_type = "queue"
    
    [[custom_transport.sender]]
    protocol = "jms"
    class="org.apache.axis2.transport.jms.JMSSender"
    ```   
    **Note**: Update the **tcp://localhost:61616** URL in the above configuration with the actual/connecting URL that will be reachable from the Kubernetes pod.
    
11.  Start the Docker daemon in the host machine.

12.  Navigate to the Maven multi module project and run the following command to build the project. It will create a docker image with provided target repository and tag once it build successfully.
    ```bash
    mvn clean install -Dmave.test.skip=true
    ```
    
13.  Run the following command to verify whether or not the docker image has been built.
     ```bash
     docker image ls
     ```

14.  Navigate to the Kubernetes project inside `MavenParentProject` and the following command to the push docker image to the remote docker registry.
     ```bash
     mvn dockerfile:push -Ddockerfile.username={username} -Ddockerfile.password={password}
     ``` 

     Else, you can use Kubernetes [Build and Push Docker Images](../../../../develop/create-kubernetes-project/#build-and-push-docker-images) section to build and push docker images to the remote registries.

15.  Open the **kubernetes_cr.yaml** file and verify that the following content is available.
     ```yaml 
     ---
     apiVersion: "integration.wso2.com/v1alpha1"
     kind: "Integration"
     metadata:
       name: "jms"
     spec:
       replicas: 1
       image: "Docker/image/path/to/the/JMSSenderListner"
       port: 8290
       env:
       - name: "jmsconfac"
         value: "TopicConnectionFactory"
       - name: "jmsuname"
         value: "admin"
       - name: "destination"
         value: "queue"
       - name: "jmsurl"
         value: "tcp://localhost:61616"
       - name: "jmspass"
         value: "admin"
       - name: "contenttype"
         value: "application/xml"
     ```
     **Note**: Update the **tcp://localhost:61616** URL shown in the above configuration with the actual/connecting URL that will be reachable from the Kubernetes pod.
     
16.  Start ActiveMQ:
     -  Navigating to the `<ACTIVEMQ_HOME>/bin` directory and execute the following command: 
        ```bash
        ./activemq console
        ```
     -  Alternative, you can deploy an ActiveMQ pod inside the Kubernetes cluster.

17.  Follow the **[Kubernetes Deployment using k8s-ei-operator](../../../../setup/deployment/kubernetes_deployment)** documentation to deploy and run the integration solution inside the Kubernetes environment.
     This will create a new queue called **firstQueue** in ActiveMQ. 
     
Send a message to this queue. The proxy service you added in **step 3** above will listen to this message and send that message to a new queue called **secondQueue**.
