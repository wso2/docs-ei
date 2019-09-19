# Getting the Streaming Integrator Running in Kubernetes in 5 Minutes

## Introduction

This quick start guide gets you to start and run the Streaming Integrator in a Kubernetes cluster in 5 minutes.

!!!tip "Before you begin:"
    - Create a Kubernetes cluster. In this quick start guide, you can do this via Minikube as follows.

        1. Install Minikube  and start a cluster by following the [Minikube Documentation](https://minikube.sigs.k8s.io/docs/start/).

        2. Enable ingress on Minikube by issuing the following command.

            `minikube addons enable ingress`

    - Make sure that you have admin privileges to install the [Siddhi operator](https://siddhi.io/en/v5.0/docs/siddhi-as-a-kubernetes-microservice/#!).


## Installing the Siddhi Operator for the Streaming Integrator

To install the Siddhi Operator, follow the

1. To install the Siddhi Kubernetes operator for streaming integrator issue the following commands:

    `kubectl apply -f https://github.com/wso2/streaming-integrator/releases/download/v1.0.0-m1/00-prereqs.yaml`

    `kubectl apply -f https://github.com/wso2/streaming-integrator/releases/download/v1.0.0-m1/01-siddhi-operator.yaml`


2. To verify whether the Siddhi operator is successfully installed, issue the following command.

    `kubectl get deployment`

    If the installation is successful, the following deployments should be running in the Kubernetes cluster.

    ![Siddhi Operator Installment](../images/hello-world-with-kubernetes/verify-siddhi-operator-installation.png)


## Deploying Siddhi applications in Kubernetes

You can deploy multiple Siddhi applications in one or more selected containers via Kubernetes. In this example, let's deploy just one Siddhi application in one container for the ease of understanding how to run the Streaming Integrator in a Kubernetes cluster.

1. First, let's design a simple Siddhi application that consumes events via HTTP to detect power surges. It filters events for a specific device type (i.e., dryers) and that also report a value greater than 600 for `power`.

    ```sql
        @App:name("PowerSurgeDetection")
        @App:description("App consumes events from HTTP as a JSON message of { 'deviceType': 'dryer', 'power': 6000 } format and inserts the events into DevicePowerStream, and alerts the user if the power level is greater than or equal to 600W by printing a message in the log.")
        /*
            Input: deviceType string and powerConsuption int(Watt)
            Output: Alert user from printing a log, if there is a power surge in the dryer. In other words, notify when power is greater than or equal to 600W.
        */


        @source(
          type='http',
          receiver.url='${RECEIVER_URL}',
          basic.auth.enabled='false',
          @map(type='json')
        )

        define stream DevicePowerStream(deviceType string, power int);

        @sink(type='log', prefix='LOGGER')
        define stream PowerSurgeAlertStream(deviceType string, power int);

        @info(name='surge-detector')
        from DevicePowerStream[deviceType == 'dryer' and power >= 600]
        select deviceType, power
        insert into PowerSurgeAlertStream;
    ```

2. The above Siddhi application needs to be deployed via a YAML file. Therefore, enter basic information for the YAML file and include the Siddhi application in a section named `spec` as shown below.

    ```xml
    apiVersion: siddhi.io/v1alpha2
    kind: SiddhiProcess
    metadata:
      name: streaming-integrator
    spec:
      apps:
        - script: |
            @App:name("PowerSurgeDetection")
            @App:description("App consumes events from HTTP as a JSON message of { 'deviceType': 'dryer', 'power': 6000 } format and inserts the events into DevicePowerStream, and alerts the user if the power level is greater than or equal to 600W by printing a message in the log.")
            /*
                Input: deviceType string and powerConsuption int(Watt)
                Output: Alert user from printing a log, if there is a power surge in the dryer. In other words, notify when power is greater than or equal to 600W.
            */

            @source(
              type='http',
              receiver.url='${RECEIVER_URL}',
              basic.auth.enabled='false',
              @map(type='json')
            )
            define stream DevicePowerStream(deviceType string, power int);
            @sink(type='log', prefix='LOGGER')
            define stream PowerSurgeAlertStream(deviceType string, power int);
            @info(name='surge-detector')
            from DevicePowerStream[deviceType == 'dryer' and power >= 600]
            select deviceType, power
            insert into PowerSurgeAlertStream;
    ```

3. Add a section named `container' and and parameters with values to configure the container in which the Siddhi application is to be deployed.

    ```
    container:
    env:
      -
        name: RECEIVER_URL
        value: "http://0.0.0.0:8080/checkPower"
      -
        name: BASIC_AUTH_ENABLED
        value: "false"
    ```

    Here, you are specifying that Siddhi applications running within the container should receive events to the `http://0.0.0.0:8080/checkPower` URL and basic authentication is not enabled for them.

4. Add a `runner` section and add configurations related to authorization such as users and roles. For this example, you can configure this section as follows.

    ```
    runner: |
    auth.configs:
      type: 'local'        # Type of the IdP client used
      userManager:
        adminRole: admin   # Admin role which is granted all permissions
        userStore:         # User store
          users:
          -
            user:
              username: root
              password: YWRtaW4=
              roles: 1
          roles:
          -
            role:
              id: 1
              displayName: root
      restAPIAuthConfigs:
        exclude:
          - /simulation/*
          - /stores/*
    ```

    ???info "To view the complete file, click here."
        apiVersion: siddhi.io/v1alpha2
        kind: SiddhiProcess
        metadata:
          name: streaming-integrator-app
        spec:
          apps:
            - script: |
                @App:name("PowerSurgeDetection")
                @App:description("App consumes events from HTTP as a JSON message of { 'deviceType': 'dryer', 'power': 6000 } format and inserts the events into DevicePowerStream, and alerts the user if the power level is greater than or equal to 600W by printing a message in the log.")
                /*
                    Input: deviceType string and powerConsuption int(Watt)
                    Output: Alert user from printing a log, if there is a power surge in the dryer. In other words, notify when power is greater than or equal to 600W.
                */

                @source(
                  type='http',
                  receiver.url='${RECEIVER_URL}',
                  basic.auth.enabled='false',
                  @map(type='json')
                )
                define stream DevicePowerStream(deviceType string, power int);
                @sink(type='log', prefix='LOGGER')
                define stream PowerSurgeAlertStream(deviceType string, power int);
                @info(name='surge-detector')
                from DevicePowerStream[deviceType == 'dryer' and power >= 600]
                select deviceType, power
                insert into PowerSurgeAlertStream;
          container:
            env:
              -
                name: RECEIVER_URL
                value: "http://0.0.0.0:8080/checkPower"
              -
                name: BASIC_AUTH_ENABLED
                value: "false"

          runner: |
            auth.configs:
              type: 'local'        # Type of the IdP client used
              userManager:
                adminRole: admin   # Admin role which is granted all permissions
                userStore:         # User store
                  users:
                  -
                    user:
                      username: root
                      password: YWRtaW4=
                      roles: 1
                  roles:
                  -
                    role:
                      id: 1
                      displayName: root
              restAPIAuthConfigs:
                exclude:
                  - /simulation/*
                  - /stores/*

5. Save the file as `siddhi-process.yaml` in a preferred location

6. To apply the configurations in this YAML file to the Kubernetes cluster, issue the following command.

    `kubectl apply -f <PATH_to_siddhi-process.yaml>`

    !!!info
        This file overrules the configurations in the `<SI_HOME>|<SI_TOOLING_HOME>/conf/server/deployment.yaml` file.

## Invoking the Siddhi application

To invoke the `PowerSurgeDetection` Siddhi application that you deployed in the Kubernetes cluster, follow the steps below.

1. First, get the external IP of minikube by issuing the following command.

    `minikube ip`

    Add the IP it returns to the `/etc/hosts` file in your machine.

2. Issue the following CURL command to invoke the `PowerSurgeDetection` Siddhi application.

    ```
    curl -X POST \
      http://siddhi/streaming-integrator-0/8080/checkPower \
        -H 'Accept: */*' \
        -H 'Content-Type: application/json' \
        -H 'Host: siddhi' \
        -d '{
            "deviceType": "dryer",
            "power": 600
            }'
    ```

3. To monitor the associated logs for the above siddhi application, get a list of the available pods by issuing the following command.

    `kubectl get pods'

    This returns the list of pods as shown in the example below.

    ```
    NAME                                        READY    STATUS    RESTARTS    AGE
    streaming-integrator-app-0-b4dcf85-npgj7     1/1     Running      0        165m
    streaming-integrator-5f9fcb7679-n4zpj        1/1     Running      0        173m
    ```

4. To monitor the logs for the required pod, issue a command similar to the following. In this example, the pod to be monitored is `streaming-integrator-app-0-b4dcf85-npgj7`.

    `streaming-integrator-app-0-b4dcf85-npgj7`