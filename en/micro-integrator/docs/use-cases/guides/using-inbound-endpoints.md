# Using Inbound Endpoints

In this sample scenario, you will use an **[Inbound
Endpoint](_Working_with_Inbound_Endpoints_)** to expose an already
defined REST API through a different port. You can reuse the REST API
that was defined in the [Sending a Simple Message to a
Service](https://docs.wso2.com/display/EI650/Sending+a+Simple+Message+to+a+Service)
tutorial. See [Creating an Inbound
Endpoint](_Creating_an_Inbound_Endpoint_) for details on how to work
with inbound endpoints using WSO2 Integration Studio.

!!! tip

**Before you begin** ,

1.  Install Oracle Java SE Development Kit (JDK) version 1.8.\* and set
    the JAVA\_HOME environment variable.
2.  Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>               /Library/WSO2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>               C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\              </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>               /usr/lib/wso2/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>               /usr/lib64/EnterpriseIntegrator/6.5.0              </code></td>
    </tr>
    </tbody>
    </table>

3.  Select and download the relevant WSO2 Integration Studio ZIP file
    based on your operating system from
    [here](https://wso2.com/integration/tooling/) and then extract the
    ZIP file.  
    The path to this folder will be referred to as
    `           <EI_TOOLING>          ` throughout this tutorial.

    !!! info

    Getting an error message? See the troubleshooting tips given under
    [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
    .


4.  If you did not try the [Sending a Simple Message to a
    Service](https://docs.wso2.com/display/EI650/Sending+a+Simple+Message+to+a+Service)
    tutorial yet, open the WSO2 Integration Studio, click **File** , and
    click **Import** .  
    Next, expand the **WSO2** category and select **Existing WSO2
    Projects into workspace** , click **Next** and upload the
    [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/SimpleMessageToServiceTutorial.zip)
    .  
    This contains the configurations of the [Sending a Simple Message to
    a
    Service](https://docs.wso2.com/display/EI650/Sending+a+Simple+Message+to+a+Service)
    tutorial so that you do not have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to the
    `          <EI_HOME>/wso2/msf4j/deployment/microservices         `
    directory. The back-end service is now deployed in the MSF4J profile
    of WSO2 EI.


  
Let's get started!

-   [Configuring an Inbound
    Endpoint](#UsingInboundEndpoints-ConfiguringanInboundEndpoint)
-   [Deploying the Inbound
    Endpoint](#UsingInboundEndpoints-DeployingtheInboundEndpoint)
-   [Invoking the Inbound
    Endpoint](#UsingInboundEndpoints-InvokingtheInboundEndpoint)

### Configuring an Inbound Endpoint

1.  Once you have exported the ESB project as described in above, the
    project directory will appear with the artifacts as shown below.
    Note the 'HealthcareAPI' that is already included.

    ![](attachments/119130512/119130517.png){width="250" height="382"}  

2.  Right-click on **SampleServices** and navigate to **New -\> Inbound
    Endpoint** . Select **Create A New Inbound Endpoint** and click
    **Next** .

3.  Enter the following details and click **Finish** .

    |                                |                            |
    |--------------------------------|----------------------------|
    | Inbound Endpoint Name          | QueryDoctorInboundEndpoint |
    | Inbound Endpoint Creation Type | HTTP                       |

    ![](https://lh3.googleusercontent.com/CYLJoSvCMhZfVYSZc73iRyAHhzgVWwjCqfkNgjPDlVs2qAs6QhsbDKt8mbIzEk8ojpONkEl2nemszzeNLPSAW3ogSs0eHqbGQMmw7WSlhx3b3Nbvfp0xGJ2Xbwl-Qbi0NxMGrSJB){width="550"}

4.  Go to the **Properties** tab in the **Design** view and enter the
    following:

    |                         |                             |
    |-------------------------|-----------------------------|
    | Inbound HTTP port       | 8285                        |
    | Dispatch Filter Pattern | /healthcare/querydoctor/.\* |

    ![](https://lh5.googleusercontent.com/KUBGiYXSzSVkZo_mDb0y9yzGFp6Fts7FdUrvrH_QlvpGDtaTiwnivjvCsMBpzhGDyRrJvCeBysYQNBFL3ndpEXwUB-U5TDsBbNS2actK3_8ie2RWULV6-g1LY3Q9XWaWOHsZNc7O){width="550"
    height="619"}

The endpoint will now get mapped to any URL that matches the above
pattern provided. You will be exposing the health care API on a new port
through this inbound endpoint.

### Deploying the Inbound Endpoint

1.  Open the `           pom.xml          ` file under the
    **SampleServicesCompositeApplication** project from the project
    explorer.

2.  See that the newly added ' QueryDoctorInboundEndpoint' artifact is
    listed under **Dependencies** . Select this artifact to add it to
    the composite application project.

    ![](attachments/119130512/119130515.png){width="700" height="123"}  

3.  Save your changes.

4.  Open the **Getting Started** view and click **Miscellaneous →Add New
    Server** to open the **New Server** dialog.

5.  In the **New Server** dialog, expand the WSO2 folder and select the
    version of your server.
6.  Click **Next** . In the CARBON\_HOME field, provide the path to your
    product's home directory, and then click **Next** again.
7.  Review the default port details for your server and click **Next** .

8.  To deploy the CApp project to your server, select the
    **SampleServicesCompositeApplication** project from the list, click
    **Add** to move it into the configured list, and then click
    **Finish** .

9.  On the **Servers** tab, note that the server is currently stopped.
    Click the 'play' icon on the tool bar. If prompted to save changes
    to any of the artifact files you created earlier, click **Yes** .

    ![](attachments/119130512/119130514.png){width="600" height="88"}

10. As the server starts, the **Console** tab appears. Note messages
    indicating that the composite app was successfully deployed.

### Invoking the Inbound Endpoint

Let's send a message to the **healthcare** REST API on the 8285 port.

1.  Open a command line terminal and execute the following command:

    ``` java
        curl -v http://localhost:8285/healthcare/querydoctor/surgery
    ```

2.  You will get the response shown below. The inbound endpoint has
    successfully invoked the REST API, and further, the response
    received by the REST API has been routed back to client through the
    inbound endpoint.

    ``` java
            [{"name":"thomas collins","hospital":"grand oak community 
               hospital","category":"surgery","availability":"9.00 a.m - 11.00 a.m","fee":7000.0},
             {"name":"anne clement","hospital":"clemency medical center","category":"surgery","availability":"8.00 a.m - 10.00 A.m","fee":12000.0},
             {"name":"seth mears","hospital":"pine valley community hospital","category":"surgery","availability":"3.00 p.m - 5.00 p.m","fee":8000.0}]
    ```
