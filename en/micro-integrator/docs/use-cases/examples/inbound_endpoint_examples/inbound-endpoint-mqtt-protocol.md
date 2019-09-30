# Inbound Endpoint MQTT Protocol Sample
## Example use case

This sample demonstrates how the MQTT connector publishes a message on a
particular topic and how a MQTT client that is subscribed to that topic
receives it. You can try this sample using the following message
brokers:

### Using Mosquitto as the Message Broker

Following sections demonstrate how you can try this sample using the
Mosquitto server as the Message Broker.

#### Prerequisites

Follow the steps below before starting the MQTT sample configurations.

1.  Download and install WSO2 EI. For instructions, see [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
2.  Download and install Mosquitto. (This sample is tested for
    [Mosquitto1.4.3 version](https://mosquitto.org/download/) .)
3.  Download [MQTT client
    library](http://repo.spring.io/plugins-release/org/eclipse/paho/mqtt-client/0.4.0/)
    (i.e. `          mqtt-client-0.4.0.jar         ` ) and add it to the
    `          <EI_HOME>/lib/         ` directory.

#### Building the sample

1.  Start the MQTT-supported server. (E.g. Mosquitto)  
    -   Execute the following command to prepare Mosquitto server for
        the launch:
        `            ln -sfv /usr/local/opt/mosquitto/*.plist ~/Library/LaunchAgents           `
    -   Execute the following command to run the Mosquitto server:
        `            launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mosquitto.plist           `
2.  Open a new Terminal and start the ESB profile of WSO2 EI. For
    instructions, see [Starting the ESB
    Profile](https://docs.wso2.com/display/EI620/Running+the+Product#RunningtheProduct-StartingtheESBprofile)
    .

3.  Log in to the Management Console, and click **Main** → **Inbound
    Endpoints** → **Add inbound Endpoint** .

4.  Enter a name for the Inbound Endpoint (e.g.,
    `           SampleInbound          ` ), select MQTT as the type, and
    click **Next** .  
    ![enter the inbound endpoint
    type](attachments/119129721/119129727.png)

5.  Enter the following details, and click **Save** .

    -   **Sequence** : `            TestIn           `
    -   **Error Sequence** : `            fault           `
    -   **coordination** : `            false           `
    -   **mqtt.connection.factory** :
        `            mqttConFactory           `
    -   **mqtt.server.host.name** : `            localhost           `
    -   **mqtt.server.port** : `            1883           `
    -   **mqtt.topic.name** : `            esb.test           `

    ![inbound endpoint of Mosquitto
    method](attachments/119129721/119129725.png "inbound endpoint of Mosquitto method")

6.  Click **Main** → **Sequences** → **Add Sequence** .

7.  Enter the name of the Sequence as `           TestIn          ` .

8.  Construct the following sequence.

    ![](images/icons/grey_arrow_down.png){.expand-control-image} Follow
    the steps below to add the mediators to the sequence.

    1.  Add a log mediator as shown below.

        ![add log
        mediator](attachments/119129721/119129722.png "add log mediator"){width="500"}

    2.  Change the **Log Level** to FULL in the Log Mediator, and click
        **Update** .

        ![](attachments/119129721/119129726.png)

    3.  Add a drop mediator as shown below.

        ![](attachments/119129721/119129723.png){width="500"}

      

    ![create the sequence](attachments/119129721/119129728.png)

9.  Click **Save & Close** .

#### Executing the sample

In a new Terminal window, execute the following command to publish a
message using the Mosquitto publisher.

``` java
    mosquitto_pub -t esb.test  -m "<msg><a>Testing123</a></msg>"
```

#### Analyzing the output

You view the ESB profile receiving the messages in the logs of it in the
Terminal as shown below.

![](attachments/119129721/119129736.png)

### Using Broker Profile as the Message Broker

Following sections demonstrate how you can try this sample using the
Broker profile of WSO2 EI as the Message Broker.

#### Prerequisites

Follow the steps below before starting this MQTT sample configurations.

1.  Download and install WSO2 EI. For instructions, see [Installation
    Guide](https://docs.wso2.com/display/EI650/Installation+Guide) .
2.  Download [MQTT client
    library](http://repo.spring.io/plugins-release/org/eclipse/paho/mqtt-client/0.4.0/)
    (i.e. `          mqtt-client-0.4.0.jar         ` ) and add it to the
    `          <EI_HOME>/lib/         ` directory.

#### Building the sample

1.  Start the Broker profile of WSO2 EI. For instructions, see [Starting
    the product profiles - Broker
    Profile](https://docs.wso2.com/display/EI650/Running+the+Product) .

2.  Start the ESB profile of WSO2 EI. For instructions, see [Starting
    the product profiles - ESB
    Profile](https://docs.wso2.com/display/EI650/Running+the+Product) .

3.  Log in to the Management Console, and click **Main** → **Inbound
    Endpoints** → **Add inbound Endpoint** .

4.  Enter a name for the Inbound Endpoint (e.g.,
    `           SampleInbound          ` ), and click **Next** .  
    ![enter the inbound endpoint
    type](attachments/119129721/119129727.png)

5.  Enter the following details, and click **Save** .

        !!! tip
    
        When creating the below Inbound Endpoint, specify the value of
        **mqtt.server.port** as 1886 because the Broker profile starts with
        a [default port
        offset](https://docs.wso2.com/display/EI620/Default+ports+of+WSO2+EI#DefaultportsofWSO2EI-EI-Brokerruntimeports)
        of 3 in WSO2 EI.
    

    -   **Sequence** : `            TestIn           `
    -   **Error Sequence** : `            fault           `
    -   **coordination** : `            false           `
    -   **mqtt.connection.factory** :
        `            mqttConFactory           `
    -   **mqtt.server.host.name** : `            localhost           `
    -   **mqtt.server.port** : `            1883           `
    -   **mqtt.topic.name** : `            esb.test           `

    ![inbound endpoint of Broker profile
    method](attachments/119129721/119129724.png "inbound endpoint of Broker profile method")

6.  Click **Main** → **Sequences** → **Add Sequence** .

7.  Enter the name of the Sequence as `           TestIn          ` .

8.  Construct the following sequence.

    ![](images/icons/grey_arrow_down.png){.expand-control-image} Follow
    the steps below to add the mediators to the sequence.

    1.  Add a log mediator as shown below.

        ![add log
        mediator](attachments/119129721/119129722.png "add log mediator"){width="500"}

    2.  Change the **Log Level** to FULL in the Log Mediator, and click
        **Update** .

        ![](attachments/119129721/119129726.png)

    3.  Add a drop mediator as shown below.

        ![](attachments/119129721/119129723.png){width="500"}

      

    ![create the sequence](attachments/119129721/119129728.png)

9.  Click **Save & Close** .

#### Executing the sample

In a new Terminal window, execute the following command to publish a
message using the Mosquitto publisher.

``` java
    mosquitto_pub -t esb.test  -m "<msg><a>Testing123</a></msg>"
```

#### Analyzing the output

You view the ESB profile receiving the messages in the logs of it in the
Terminal as shown below.

![](attachments/119129721/119129736.png)
