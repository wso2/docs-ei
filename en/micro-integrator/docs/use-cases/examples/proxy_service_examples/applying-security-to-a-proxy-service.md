# Applying Security to a Proxy Service

The steps below demonstrate how you can apply security to a proxy
service via WSO2 Integration Studio .

-   [Prerequisites](#ApplyingSecuritytoaProxyService-Prerequisites)
-   [Step 1: Creating a registry resource
    project](#ApplyingSecuritytoaProxyService-Step1:Creatingaregistryresourceproject)
-   [Step 2: Creating the security policy
    file](#ApplyingSecuritytoaProxyService-Step2:Creatingthesecuritypolicyfile)
-   [Step 3: Add a proxy
    service](#ApplyingSecuritytoaProxyService-Step3:Addaproxyservice)
-   [Step 4: Add the security policy to the proxy
    service](#ApplyingSecuritytoaProxyService-Step4:Addthesecuritypolicytotheproxyservice)
-   [Step 5: Deploying the artifacts in the ESB
    server](#ApplyingSecuritytoaProxyService-Step5:DeployingtheartifactsintheESBserver)
-   [Testing the
    service](#ApplyingSecuritytoaProxyService-Testingtheservice)

### Prerequisites

-   [Install WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
    .
-   Click [this link](attachments/119130870/119130892.xml) to download
    the sample proxy service ( **StockQuoteProxy.xml** ). We will use
    this proxy service to apply security.

### Step 1: Creating a registry resource project

First, create a registry resource project. We will use this project to
store the security policy (which is a registry resource).

1.  Open **WSO2 Integration Studio** and click **Miscelleneous → Create
    New Registry **Project**** in the **Getting Started** tab as shown
    below.

    ![](attachments/119130870/119133596.png){width="800" height="396"}

2.  Enter a name for the project and click **Next** .
3.  Enter the Maven information about the project and click **Finish** .
4.  The new project will be listed in the project explorer.

### Step 2: Creating the security policy file

Follow the instructions given below to create a **WS-Policy** resource
in your registry project. This will be your security policy file.

1.  Right-click the registry resource project in the left navigation
    panel, click **New** , and then click **Registry Resource** . This
    will open the **New Registry Resource** window.  
    ![](attachments/119130870/119130887.png){width="350" height="458"}
2.  Select the **From existing template** option as shown below and
    click **Next** .  
    ![](attachments/119130870/119130886.png){width="500" height="535"}
3.  Enter a resource name and select the **WS-Policy** template along
    with the preferred registry path.  
    ![](attachments/119130870/119130885.png){width="400" height="519"}  
    ![](attachments/119130870/119130884.png){width="500" height="535"}
4.  Click **Finish** . The policy file is now listed in the project
    explorer as shown below  
    ![](attachments/119130870/119130883.png){width="1166"
    height="250"}  
      
5.  Double-click the policy file to open the file. Note that you get a
    **Design View** and **Source View** of the policy.

6.  Let's use the **Design View** to enable the required security
    scenario. For example, enable the **Sign and Encyrpt** security
    scenario as shown below.

        !!! tip
    
        Click the icon next to the scenario to get details of the scenario.
    

      
    ![](attachments/119130870/119130882.png){width="600" height="433"}

7.  You can provide also provide encryption properties, signature
    properties, and advanced rampart configurations as shown below.

    -   [**Encryption/Signature
        Properties**](#ac024c573da14fbf845e2ff2fe98c231)
    -   [**Rampart Properties**](#52a3ebf19d5c4b9aa6a8b63c7b228b17)

    ![](attachments/119130870/119130890.png){width="320" height="374"}

    ![](attachments/119130870/119130889.png){width="530" height="400"}

#### Specifying role-based access?

For certain scenarios, you can specify user roles. After you select the
scenario, scroll to the right to see the **User Roles** button.

![](attachments/119130870/119130874.png){width="600" height="289"}

Either define the user roles inline or retrieve the user roles from the
server.

-   [**Define Inline**](#b899b74fbe2f4c608f6bbc0b492a201c)
-   [**Get from the server**](#e976e501bf9b4b6e946762cc5ff68ed5)

![](attachments/119130870/119130872.png){width="500" height="362"}

![](attachments/119130870/119130871.png){width="500" height="363"}

!!! info

By default, the role names are not case sensitive. If you want to make
them case sensitive, add the following property under the
`         <AuthorizationManager>        ` configuration in the
`         user-mgt.xml        ` file:

  

``` java
    <Property name= "CaseSensitiveAuthorizationRules"> true </Property>
```
    

### Step 3: Add a proxy service

You can either create a new proxy service, or import an already created
proxy service to your workspace.

Follow the steps given below.

1.  Right-click the **ESB Solution project** in the navigator and go to
    **New → Proxy Service** to open the **New Proxy Service** dialog.
2.  Let's import the proxy service you downloaded previously. Click
    **Import Poxy Service** and **Next** . Enter values for the
    following fields:

        !!! tip
    
        Alternatively, you can [create a new proxy
        service](Creating-a-Proxy-Service_119130913.html#CreatingaProxyService-create)
        .
    

    <table>
    <tbody>
    <tr class="odd">
    <td>Proxy Service Configuration File</td>
    <td>Browse for the proxy service file that you downloaded <a href="#ApplyingSecuritytoaProxyService-Prerequisites">previously</a> .</td>
    </tr>
    <tr class="even">
    <td>Save in</td>
    <td><p>The file you import should be saved in an ESB project in your Tooling workspace. To create a new ESB project:</p>
    <ol>
    <li>Click <strong>Create New ESB Project</strong> .</li>
    <li>Select <strong>New ESB Config Project</strong> .</li>
    <li>Enter a project name. For example, enter 'ESB_Project' as the project name.</li>
    <li>Click <strong>Finish</strong> . The new ESB_Project is added to the <strong>Save in</strong> field.</li>
    </ol></td>
    </tr>
    </tbody>
    </table>

3.  Click **Finish** . You will now see a new ESB\_Project folder (with
    the proxy service) in your project explorer.  
    ![](attachments/119130870/119130880.png?effects=drop-shadow){width="800"
    height="288"}  
    ![](attachments/119130870/119130878.png?effects=drop-shadow){width="800"}

### Step 4: Add the security policy to the proxy service

You can now apply the security policy to the proxy service. Follow the
steps given below.

1.  Double-click the proxy service on the project explorer to open the
    file and click on the service on design view.
2.  In the **Properties** tab shown below and tick on **Security
    Enabled** property.  
    ![](attachments/119130870/119130879.png){width="900" height="197"}
3.  Select the **Browse** icon for the **Service Policies** field. In
    the dialog box that opens, create a new record and click the
    **Browse** icon to open the **Resource Key** dialog as shown
    below.  
    ![](attachments/119130870/119130877.png){width="500" height="300"}
4.  Click **workspace** , to add the security policy from the current
    workspace. You can select the path to the
    `          sample_policy.         ` xml file that you created in the
    previous steps.  
    ![](attachments/119130870/119130876.png){width="800" height="343"}
5.  Save the proxy service file.

### Step 5: Deploying the artifacts in the ESB server

Once you have added the security policy to your proxy service as
explained in the previous topics, you need to create a **Composite
Application** project with a CAR file. You can then deploy the CAR file
in the ESB server:

1.  Right-click the **Project Explorer** and click **New \> Project** .
2.  From the window that opens, click **Composite Application Project**
    .
3.  Give a name to the **Composite Application** project and select the
    projects that you need to group into your C-App from the list of
    available projects. You need to select the **ESB** project and the
    **registry resource** project, which contains the proxy service and
    security policy file respectively.  
    ![](attachments/119130870/119130875.png){width="500" height="346"}

4.  Next, [deploy the CAR file in the ESB
    server](https://docs.wso2.com/display/ADMIN44x/Deploying+Composite+Applications+in+the+Server)
    .

### Testing the service

Secured proxy services (not including user name token) cannot be tested
using the management console. For this, we need to create a Soap UI
project with the relevant security settings and then send the request to
the hosted service.
