# Deploying Streaming Applications

A Siddhi application ( `         .siddhi file        ` ) can be deployed
to run that on production.

There are two modes deployment

-   Standard deployment  
    Here we deploy Siddhi Apps in worker node (In Single node, Minimum
    HA Deployments)

<!-- -->

-   Fully distributed deployment  
    Here we deploy Siddhi Apps in manager node (Only in Fully
    Distributed Deployment)

Refer below sections to get to know how to deploy on the above
deployments:

-   [Standard
    Deployment](#DeployingStreamingApplications-StandardDeployment)
-   [Fully Distributed
    Deployment](#DeployingStreamingApplications-FullyDistributedDeployment)

### Standard Deployment

To start a worker node in single node mode, issue one of the following
commands:

-   For Windows: `          worker.bat         `
-   For Linux : `          ./worker.sh         `

Siddhi applications can be deployed to the worker node by using one of
the following methods:

-   **Using Stream Processor Studio**  
    This involves deploying the Siddhi applications via the Stream
    Processor studio once you create and save them. To do this, follow
    the substeps below.

        !!! tip
    
        This method allows you to deploy multiple Siddhi applications to
        mutiple servers at once.
    

      

    1.  Open the Stream Processor Studio. For detailed instructions, see
        [Stream Processor Studio
        Overview](https://docs.wso2.com/display/SP440/Stream+Processor+Studio+Overview)
        .
    2.  In the top menu bar, click **Deploy** and then click **Deploy to
        Server** .  
        ![](attachments/112391085/112391089.png){width="850"}
    3.  If you want to deploy all the Siddhi applications saved in the
        Stream Processor Studio, select the check box for the
        **Workspace** directory as shown in the example below. If not,
        select the check boxes of the required Siddhi applications.  
        ![](attachments/112391085/112391088.png){height="250"}
    4.  Add the servers to which you want to deploy the Siddhi
        applications as follows:  
        ![](attachments/112391085/112391087.png){width="771"}  
        1.  In the **Host** field, enter the host of the server.
        2.  In the **Port** field, enter the port of the server.
        3.  In the **User Name** field enter the user name to log in to
            the server.
        4.  In the **Password** field, enter the password to  log in to
            the server.
        5.  Click **Add** to add the server.

          
        Repeat the above substeps to specify all the servers to which
        the Siddhi applications you selected need to be deployed.  
          
    5.  Once you have added all the required servers, click **Deploy** .
        As a result, a log appears to indicate whether the selected
        Siddhi applications are successfully deployed as shown in the
        example below.  
        ![](attachments/112391085/112391086.png){width="771"}

-   **Deploying manually**  
    This involves dropping the `          .siddhi         ` file in to
    the
    `          <SP_HOME>/wso2/worker/deployment/siddhi-files/         `
    directory before or after starting the worker node.
-   **Via REST API**  
    This involves sending a "POST" request to http://:/siddhi-apps with
    the Siddhi App included in the body of the request. Refer Stream
    Processor REST API Guide for more information on using WSO2 Strean
    Processor APIs.

When a Siddhi application is successfully deployed, a message similar to
the following example appears in the startup logs.

![](attachments/112391085/112391090.png)

To configure a Minimum HA deployment refer [Minimum High Availability
Deployment](https://docs.wso2.com/display/SP440/Minimum+High+Availability+Deployment)
documentation.

### Fully Distributed Deployment

To successfully set up, configure and run Siddhi applications in a fully
distributed environment refer [Fully Distributed
Deployment](https://docs.wso2.com/display/SP440/Fully+Distributed+Deployment)
documentation.

!!! info

If you need to deploy streaming applications in a Docker environment,
they need to be exported as Docker artifacts. For mo
![](attachments/112391085/112391090.png) re information, see [Exporting
Siddhi Files as Docker
Artifacts](https://docs.wso2.com/display/SP440/Exporting+Siddhi+Files+as+Docker+Artifacts)
.


  
  
  
