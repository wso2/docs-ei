# Applying Security to a Data Service

WSO2 supports WS-Security, WS-Policy, and WS-Security Policy
specifications. These specifications define a behavioral model for Web
services. To enable a security policy for a data service, you need to
first create a security policy file, and then add it to the data
service.

## Prerequisites

Be sure to [configure a user store](../../../../setup/user_stores/setting_up_ro_ldap) for the Micro Integrator and add the required users and roles.

## Step 1: Creating a registry project

Registry artifacts (such as security policy files) should be stored in a
**Registry Resource** project. Follow the steps given below to create a
project:

1.  Open **WSO2 Integration Studio** and click **Miscelleneous → Create
    New Registry Project** in the **Getting Started** tab as shown
    below.

    ![](../../../assets/img/tutorials/data_services/119130577/119135181.png)

2.  Enter a name for the project and click **Next** .
3.  Enter the Maven information about the project and click **Finish** .
4.  The new project will be listed in the project explorer.

## Step 2: Creating a security policy as a registry resource

1.  Right-click the registry resource project in the left navigation
    panel, click **New** , and then click **Registry Resource** . This
    will open the **New Registry Resource** window.
2.  Select the **From existing template** option as shown below and
    click **Next** .  
    ![](../../../assets/img/tutorials/data_services/119130577/119130583.png)
3.  Enter the following details:

    | Property      |    Value       |
    |---------------|----------------|
    | Resource Name | Sample_Policy  |
    | Artifact Name | Sample_Policy  |
    | Template      | WS-Policy      |
    | Registry      | gov            |
    | Registry path | ws-policy/     |

4.  Click **Finish** and the policy file will be listed in the
    navigator.
    1.  Let's use the **Design View** to enable the required security
        scenario. For example, enable the **Sign and Encyrpt** security
        scenario.

        !!! Tip
            Click the icon next to the scenario to get details of the scenario.
          
        ![](../../../assets/img/tutorials/data_services/119130577/119130596.png)

    2.  You can also provide encryption properties, signature
        properties, and advanced rampart configurations.

        !!! Info
            **Using role-based permissions?**
        
            For certain scenarios, you can specify user roles. After you select the scenario, scroll to the right to see the **User Roles** button.  

            ![](attachments/119130577/119130622.png)
        
            Either define the user roles inline or retrieve the user roles from the server.
                
        !!! Info
            Switch to source view of the policy file and make sure the tokenStoreClass in the policy file is 'org.wso2.micro.integrator.security.extensions.SecurityTokenStore'
        
5.  Save the policy file.

## Step 2: Adding the security policy to the data service

Once you have configured the policy file, you can add the security
policy to the data service as explained below.

1.  If you have already created a data service using WSO2 Integration
    Studio, select the file from the Project Explorer.

    !!! Tip
        Be sure to update your database credentials in the dataservice file.
    
2.  Once you have select the data service file, click the **browse**
    icon for the **Policy** field.

    ![](../../../assets/img/tutorials/data_services/119130577/119130582.png)

3.  Click **workspace**, to add the security policy from the current
    workspace. You can select the path to the
    `          sample_policy.         ` xml file that you created in the
    previous steps.  
    ![](../../../assets/img/tutorials/data_services/119130577/119130581.png)
4.  Click **OK**, and the security policy will be added to the data
    service.  
    ![](../../../assets/img/tutorials/data_services/119130577/119130579.png)
5.  Save the data service.

## Step 3: Package the artifacts

See the instructions on [packaging the artifacts](../../../../develop/packaging-artifacts) into a composite application project.

## Step 4: Build and run the artifacts

See the instructions [deploying the artifacts](../../../../develop/deploy-and-run).

## Step 5: Testing the service

Create a Soap UI project with the relevant security settings and then send the request to the hosted service.

## Using an encrypted datasource password

When you create a data service for an RDBMS datasource, you have the
option of encrypting the datasource connection password. This ensures
that the password is encrypted in the configuration file (.dbs file) of
the data service.

See the instructions on [encrypting plain-text passwords](../../../../setup/security/encrypting_plain_text)

Once you have encrypted the datasource password, you can update the data
service as explained below.

1.  Select the data service from the [data service project](../../../../develop/creating-projects/#data-services-project), right click and
    go to **-> Open With -> Text Editor** . This will open the data
    service in text form.    
2.  Update the datasource configuration by adding the secret alias of
    the password. See the example given below.

    ```xml
    <config id="Datasource">
            <property name="org.wso2.ws.dataservice.user">root</property>
            <property name="org.wso2.ws.dataservice.password" svns:secretAlias="DB_Password_Alias"></property>
            <property name="org.wso2.ws.dataservice.protocol">jdbc:mysql://localhost:3306/Employees</property>
            <property name="org.wso2.ws.dataservice.driver">com.mysql.jdbc.Driver</property>
            <property name="org.wso2.ws.dataservice.minpoolsize"/>
            <property name="org.wso2.ws.dataservice.maxpoolsize"/>
            <property name="org.wso2.ws.dataservice.validation_query"/>
    </config>
    ```

3.  Add the "http://org.wso2.securevault/configuration" namespace
    configuration to the datasource as follows:

    ```xml
    <data enableBatchRequests="true" name="RDBMSDataService" serviceNamespace="http://ws.wso2.org/dataservice/samples/rdbms_sample" xmlns:svns="http://org.wso2.securevault/configuration">
    ```
4.  Save the data service file.