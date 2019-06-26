# Working with Passwords in the ESB profile

All WSO2 products are shipped with a **Secure Vault** implementation
that allows you to store encrypted passwords that are mapped to aliases.
This approach allows you to use the aliases instead of the actual
passwords in your configurations for better security. For example, some
configurations require the admin username and password. If the admin
user's password is "admin", you could use
`         UserManager.AdminUser.Password        ` as the password alias.
You will then map that alias to the actual "admin" password using Secure
Vault. The WSO2 product will then look up this alias in Secure Vault
during runtime, decrypt and use its password.

!!! note

Go to the WSO2 administration guide for more information about the
[Secure Vault implementation in WSO2
products](https://docs.wso2.com/display/ADMIN44x/Carbon+Secure+Vault+Implementation)
.


In all WSO2 products, Secure Vault is commonly used for encrypting
passwords and other sensitive information in configuration files. When
you use the ESB profile of WSO2 EI, you can encrypt sensitive
information contained in synapse configurations in addition to the
information in configuration files. See the following topics:

-   [Encrypting passwords in configuration
    files](#WorkingwithPasswordsintheESBprofile-Encryptingpasswordsinconfigurationfiles)
-   [Encrypting passwords for synapse
    configurations](#WorkingwithPasswordsintheESBprofile-Encryptingpasswordsforsynapseconfigurations)
    -   [Using encrypted passwords in synapse
        configurations](#WorkingwithPasswordsintheESBprofile-Usingencryptedpasswordsinsynapseconfigurations)
-   [Updating the password
    validation](#WorkingwithPasswordsintheESBprofile-Updatingthepasswordvalidation)

### Encrypting passwords in configuration files

To encrypt passwords in configuration files, you simply have to update
the `         cipher-text.properties        ` and
`         cipher-tool.properties        ` files that are stored in the
`         <EI_HOME>/conf/security/        ` directory and then run the
Cipher tool that is shipped with the product. Go to the links given
below to see instructions in the WSO2 administration guide:

-   [Encrypting passwords using the automated
    process](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool#EncryptingPasswordswithCipherTool-automated)
    .
-   [Encrypting passwords using the manual
    process](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool#EncryptingPasswordswithCipherTool-manual_process)
    . This is relevant when the location of the configuration files
    (that contain the elements to be encrypted) cannot be specified
    using an xpath in the `          cipher         ` -
    `          tool.properties         ` file.
-   [Changing already encrypted
    passwords](https://docs.wso2.com/display/ADMIN44x/Encrypting+Passwords+with+Cipher+Tool#EncryptingPasswordswithCipherTool-changing_encrypted_passwords)
    .
-   [Resolving already encrypted
    passwords](https://docs.wso2.com/display/ADMIN44x/Resolving+Encrypted+Passwords)
    .

### Encrypting passwords for synapse configurations

The ESB profile of WSO2 EI provides a UI that can be used for encrypting
passwords and other sensitive information in synapse configurations.

!!! tip

**Before you begin** , be sure that your [registry
database](https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry)
has write-access enabled. Open the `         registry.xml        ` file
(stored in the `         <EI_HOME>/conf/        ` directory) and ensure
that the `         <readOnly>        ` element is set to
`         false        ` as shown below.

``` java
    <currentDBConfig>wso2registry</currentDBConfig>
    <readOnly>false</readOnly>
    <enableCache>true</enableCache>
    <registryRoot>/</registryRoot>
```
    
    This is necessary because the passwords you encrypt using the management
    console of the ESB profile are written to the registry DB. If the
    registry does not have write-access enabled, the required functions on
    the management console will be disabled.
    

Follow the steps given below if you are using the ESB profile.

1.  If you are using the Cipher tool for the first time in your
    environment, you must first enable the Cipher tool by executing
    the -Dconfigure command with the cipher tool script:

    1.  Open a terminal and navigate to the
        `            <EI_HOME>/bin           ` directory.
    2.  Execute one of the following commands:
        -   On Linux:
            `               ./ciphertool.sh -Dconfigure              `

        -   On Windows:
            `               ./ciphertool.bat -Dconfigure              `

2.  Start the ESB profile of WSO2 EI and sign in to the management
    console:
    1.  Open a terminal and navigate to the
        `            <EI_HOME>/bin           ` directory.
    2.  Execute one of the following scripts:
        -   On Windows:
            `              integrator.bat --run             `
        -   On Linux/Mac OS:
            `              sh integrator.sh             `
    3.  Sign in to the management console.
3.  Go to **Manage** -\> **Secure Vault Tool** and then click **Manage
    Passwords** on the **Main** tab of the management console. The
    **Secure Vault Password Management** screen appears.
4.  Click **Add New Password to encrypt and store,** and then specify
    values for the given fields as shown below. This creates a new
    password entry in the
    [registry](https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry)
    , which is encrypted with the alias (Vault Key) that you specify.
    -   **Vault Key:** The alias for the password.
    -   **Password:** The actual password.
    -   **Re-enter password:** The password that you specified as the
        actual password.

    ![](attachments/119130247/119130248.png){width="669" height="309"}

#### Using encrypted passwords in synapse configurations

To use the alias of an encrypted password in a synapse configuration,
you need to add the `         {wso2:vault-lookup('alias')}        `
custom path expression when you define the synapse configuration. For
example, instead of hard coding the admin user's password as
`         <Password>admin</Password>        ` , you can encrypt and
store the password using the `         AdminUser.Password        ` alias
as follows:
`         <Password>{wso2:vault-lookup('AdminUser.Password')}</Password>.        `

This password in the synapse configuration can now be retrieved by
using the `         {wso2:vault-lookup('alias')}        ` custom path
expression to logically reference the password mapping.

### Updating the password validation

The default expression used for password validation is
`         ^[\\S]{5,30}$        ` . This allows the password to have 5 to
30 characters.

If you want to change the expression that is used to validate the
password, you need to add the
`         org.wso2.SecureVaultPasswordRegEx        ` system property
to the `         <EI_HOME>/conf/carbon.properties        ` file.  
Example:

``` java
    org.wso2.SecureVaultPasswordRegEx=^[\\S]{5,60}$
```
