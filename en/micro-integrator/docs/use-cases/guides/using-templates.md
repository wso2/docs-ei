# Using Templates

In this sample scenario, you will use a **[Sequence
Template](https://docs.wso2.com/display/EI650/Working+with+Inbound+Endpoints)**
and reuse it in multiple places of the medation flow. You can reuse the
mediation flow that was defined in the [Sending a Simple Message to a
Service](https://docs.wso2.com/display/EI650/Sending+a+Simple+Message+to+a+Service)
tutorial and then replace its sections with the sequence template . See
[Creating Templates](_Working_with_Templates_via_Tooling_) for details
on how to work with templates using WSO2 Integration Studio.

  

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
    <td><code>                /Library/WSO2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>                C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\               </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>                /usr/lib/wso2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>                /usr/lib64/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    </tbody>
    </table>

3.  Select and download the relevant WSO2 Integration Studio ZIP file
    based on your operating system from
    [here](https://wso2.com/integration/tooling/) and then extract the
    ZIP file.  
    The path to this folder will be referred to as
    `            <EI_TOOLING>           ` throughout this tutorial.

    !!! note

    Getting an error message? See the troubleshooting tips given under
    [Installing WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio#InstallingWSO2IntegrationStudio-Troubleshooting)
    .


4.  If you did not try the [Exposing Several Services as a Single
    Service](https://docs.wso2.com/display/EI650/Exposing+Several+Services+as+a+Single+Service)
    tutorial yet, open the WSO2 Integration Studio, click **File** , and
    then click **Import** . Next, select **Existing WSO2 Projects into
    workspace** under the **WSO2** category, click **Next** and upload
    the [pre-packaged
    project](https://github.com/wso2-docs/WSO2_EI/blob/master/Integration-Tutorial-Artifacts/ExposingSeveralServicesTutorial.zip)
    . This contains the configurations of the [Exposing Several Services
    as a Single
    Service](https://docs.wso2.com/display/EI650/Exposing+Several+Services+as+a+Single+Service)
    tutorial so that you do not have to repeat those steps.
5.  Download the MSF4J service from
    [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-2.0.0.jar)
    and copy the JAR file to
    `           <EI_HOME>/wso2/msf4j/deployment/microservices          `
    folder. The back-end service is now deployed in the MSF4J profile of
    WSO2 EI.

  
Let's get started!

-   [Configuring a sequence
    template](#UsingTemplates-Configuringasequencetemplate)
-   [Using the sequence template in the REST
    API](#UsingTemplates-UsingthesequencetemplateintheRESTAPI)
-   [Deploying the artifacts](#UsingTemplates-Deployingtheartifacts)
-   [Invoking the ESB sequence](#UsingTemplates-InvokingtheESBsequence)

### Configuring a sequence template

1.  Once you have exported the ESB project as described above, the
    project directory will appear with the artifacts as shown below.

    ![](attachments/119131612/119131619.png){width="250" height="413"}

2.  Right-click on **SampleServices** and navigate to **New -\>
    Template** . The **New Template Artifact** dialog will open.

3.  Select the **Create a New Template** and click **Next** .

4.  Enter the following details and click **Finish** .

    |               |                    |
    |---------------|--------------------|
    | Template Name | HospitalRoutingSeq |
    | Template Type | Sequence Template  |

    ![](https://lh6.googleusercontent.com/jlYENAKLNtl-psHJCLm1dG_ziOYkinAK755IrObL-xdK2KXYmCkMju76X957PeSMQ8Hn5Q5RfNRgv_Uq-wOjE6apTyTlU3-jhf0ZUylfuydaOTCp8EZPtWkQ4hS9_cLADSME168K){width="550"}  

5.  The template artifact will open in the canvas as shown below.

    ![](attachments/119131612/119131617.png){width="600" height="327"}  

6.  Open the **Properties** tab of the sequence template by clicking on
    the canvas (outside the sequence box).  

7.  Click the ![](attachments/119131612/119131614.png){width="20"} icon
    to start adding parameters .

    ![](attachments/119131612/119131615.png){width="500" height="444"}  

8.  In the **Template Parameter** dialog that opens, enter '
    sethospital' as the parameter name and click **Finish** .

9.  Add a **Log** mediator to the sequence template as shown below. This
    will print a message indicating to which hospital a requested
    message is routed.

    ![](attachments/119131612/119131613.png){width="550" height="330"}

10. Open the **Properties** tab of the log mediator and specify the
    following:

    |              |        |
    |--------------|--------|
    | Log Category | INFO   |
    | Log Level    | CUSTOM |

11. Click the ![](attachments/119131612/119131614.png){width="20"} icon
    to start defining a property. Then add the following details for the
    property:

    |                     |                                                    |
    |---------------------|----------------------------------------------------|
    | Name                | message                                            |
    | Type                | EXPRESSION                                         |
    | Property Expression | fn:concat('Routing to ', get-property('Hospital')) |

    We select EXPRESSION because the required properties for the log
    message must be extracted from the request, which we can do using an
    XPath expression.  

12. Add a **Property** mediator just after the **Log** mediator to store
    the value for uri.var.hospital.

    ![](https://lh6.googleusercontent.com/Q4ulnFRxB7JyfuTexWUkGz_BurFUn8K45OPKrKDy-fZ1dS9fbC3Y0CBTdCMcKanKdPD2Httx-7C6S746QEb96ixJ-y48On_IHgaxqG2FqnAQfM4JemIkN2EnSpoJgqvF2FGztgA-){width="502"
    height="309"}  

13. With the **Property** mediator selected, access the **Properties**
    tab and enter the information given below:

    |                    |                       |
    |--------------------|-----------------------|
    | Property Name      | Select New Property   |
    | New Property Name  | uri.var.hospital      |
    | URI Template       | Select set            |
    | Value Type         | Select EXPRESSION     |
    | Property Data Type | Select STRING         |
    | Value Expression   | $func:sethospital     |
    | Description        | Set Hospital Variable |

### Using the sequence template in the REST API

1.  Open the design view of the HealthcareAPI.xml and delete 'GrandOak'
    **Log** mediator by right clicking the mediator and selecting
    **Delete from Model** .  
    ![](https://lh4.googleusercontent.com/6Lsp6WbAQJwjSbmsyWlKnIMkP6Q3cB9hiwzvGRkyFUtbZKteu--ePnwvaA2LqfD-7AyLqy6U1Im42We67PsSP4JhL41vlZ8nwTC8S2KrF11Hpfu365bBXhCNOZX8cMlgdBgGX7e-){width="600"
    height="351"}

2.  Delete the 'Set Hospital Variable' **Property** mediator.

3.  Add a **Call Template** mediator to the sequence as shown below.  
    ![](https://lh6.googleusercontent.com/1SUm23IRnOH-vp_o0QWpSS3bhIawxhmsKN_1Y_PYyEaZ2k6_2tuDFeyyJtp18c8Q-p6mubMToqA6v4tyWDtBMgQcJdXet00Vtxa-IDnu7TTh5eSvqnEfSi9YEoxEhuE1yJO62w4B){width="600"
    height="312"}

4.  Open the **Properties** tab of the **Call Template** mediator and
    select ' HospitalRoutingSeq' from the list of available templates.

5.  Click the ![](attachments/119131612/119131614.png){width="20"} icon
    to start adding parameters. Enter the following parameter details
    and click **Finish** .

    |                  |             |
    |------------------|-------------|
    | Parameter Name   | sethospital |
    | Parameter Type   | value       |
    | Value/Expression | grandoaks   |

    ![](https://lh5.googleusercontent.com/Pu0UusC_42jmt_VQ2NEA1PdwOI8z2VyC7bDm7HSJ6iJgf9J8Jz4k87PZ2e9UAuT62FEHdpFXXGlXx5n78qtBBvQxEmQbDkMlg3lCfTIn5grDxKDaZW0XGItxqJ72XmC0_uE84gKO){width="624"
    height="316"}

6.  Repeat the above steps to add **Call Templates** for 'Clemency' and
    'Pine Valley' hospitals. Add **clemency** and **pinevalley** as the
    respective parameter values.

7.  S ave the configuration.

### Deploying the artifacts

1.  Open the `           pom.xml          ` file under the
    **SampleServicesCompositeApplication** project from the project
    explorer.

2.  See that the newly added ' HospitalRoutingSeq ' artifact is listed
    under **Dependencies** . Select this artifact to add it to the
    composite application project.

    ![Screen Shot 2016-10-14 at 10.38.42
    AM.png](https://lh6.googleusercontent.com/3S5ZsFZU51zy9bvUv1sqI4LT0nvUR6v1VtPlrPGZp5CqS_x4M23K8kmcQ08N-PizZP1ybis62VG7BbxZ7uoe6bJKV_7Pzo2vFCZUb13THQUB6PMQI_AwL-E90UpT2fAtajD6fj-Y){width="550"}

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

    ![](attachments/119131612/119131622.png){width="600" height="88"}

10. As the server starts, the **Console** tab appears. Note messages
    indicating that the composite app was successfully deployed.

### Invoking the ESB sequence

Send a request to the ESB as shown below and see that the response is
successfully received.

1.  Open a command line terminal and execute the following command from
    the location where the request.json file you created is saved:

    ``` java
        curl -v -X POST --data @request.json http://localhost:8280/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
    ```

2.  You will see the response in the command line terminal.
