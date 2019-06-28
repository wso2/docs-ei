# Securing Datasource Password

All WSO2 products, by default, come with a [secure vault
implementation](https://docs.wso2.com/display/Carbon440/Secure+Vault+Implementation)
, which is a modified version of synapse secure vault. It provides
capability to securely store-sensitive data such as plain-text passwords
in configuration files of the WSO2 Carbon platform, such as
`         user-mgt.xml        ` , `         Carbon.xml        ` ,
`         Axis2.xml        ` , `         registry.xml        ` etc. All
WSO2 Carbon-based products inherit the secure vault implementation from
the core Carbon platform. For more information, go to [Securing
Passwords in Configuration
Files](https://docs.wso2.com/display/ADMIN44x/Securing+Passwords+in+Configuration+Files)
.

However, when securing passwords of more product-specific configuration
files such as data service configurations, the steps may vary.
Therefore, WSO2 DSS provides the feature to securely store sensitive
data in data service configuration files, using the secure vault
functionality. Users can encrypt their passwords using tokens instead of
the actual password inside the data service configuration file. See the
instructions given below.

!!! note

You need to manually secure the password if you are creating the
datasource inline when creating the data service. However, If you create
the datasource via the **Configure** → **Datasources** → **Add
Datasource** option in the Management Console, it stores the datasource
in the Registry, and thereby, t he password you enter will be encrypted
automatically .


1.  Run ciphertool script from `          <PRODUCT_HOME>/bin         `
    directory.  
    -   Linux: sh ciphertool.sh -Dconfigure
    -   Windows: ciphertool.bat -Dconfigure
2.  To encrypt the plain text using ciphertool, run the ciphertool
    script again without `          -Dconfigure         ` option. It
    asks for the KeyStore Password of the running Carbon instance. The
    default is `          wso2carbon         ` .
3.  Then provide the plain text value that needs to be encrypted and the
    tool returns the encrypted text value.
4.  Update the
    `          <PRODUCT_HOME>/repository/conf/security/cipher-text.properties         `
    file by adding a new alias (any name of your preference) and the
    encrypted value.
5.  Log in to the Management Console and click **Main** .
6.  Click **List** under the **Services** menu and click on the service
    whose password you want to secure.
7.  Click **Edit Data Service (Wizard)** , click **Next** , and then
    click **Edit Datasource** .
8.  In the **Edit datasource** window of the management console select
    **Use as Secret Alias** option. I n the **Password** field, provide
    the alias name instead of the actual password.  
    ![](attachments/119130668/119130671.png){width="500"}
9.  The namespace and alias will be added to the .dbs file as follows:
    ![](attachments/119130668/119130669.png){width="850"}
