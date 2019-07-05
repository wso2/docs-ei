# Encrypting Passwords in Configuration files

The instructions on this page explain how plain text passwords in configuration files can be encrypted using the secure vault implementation that is built into WSO2 products. 

Note that you can customize the default secure vault configurations in the product by implementing a new secret repository, call back handler etc.

## Before you begin
If you are using Windows, you need to have [Ant](http://ant.apache.org/) installed before using the Cipher Tool.

## Encrypting passwords

1. Open the ei.toml file and add the `[secrets]` configuration section as shown below. Give an alias for the password type followed by the actual password. The following example lists the most common passwords in configuration files.

    ```
    [secrets]
    db_password = "password_1"
    admin_password = "password_2"
    keystore_password = "password_3"
    key_password = "password_4"
    truststrore_password = "password_5"
    log4j.appender.LOGEVENT.password = "password_6"
    ```

    See the complete list of [configuration parameters](../../references/ei_config_catalog.md).

2. Open a terminal, navigate to the MI_HOME/bin/ directory, and execute the following command (You must first enable the Cipher tool for the product by executing the `-Dconfigure` command with the cipher tool script as shown below).
    * On Linux: `./ciphertool.sh -Dconfigure`
    * On Windows: `./ciphertool.bat -Dconfigure`

3. Go back to the ei.toml file and see that the alias passwords are encrypted.
    ```
    [secrets]
    db_password = "encrypted_pass_1"
    admin_password = "encrypted_pass_2"
    keystore_password = "encrypted_pass_3"
    key_password = "encrypted_pass_4"
    truststrore_password = "encrypted_pass_5"
    ```

    See the complete list of [configuration parameters](../../references/ei_config_catalog.md).

## Using encrypted Passwords
When you have [encrypted passwords](#encrypting-passwords), you can refer them from the relevant configuration files: The ei.toml file or LOG4j properties.

### Passwords in ei.toml

You can add the encrypted password to the relevant sections in the ei.toml file by using a place holder: `$ref{alias}`. 

> Note that you can also replace your passwords by refering values passed by environment variables and system properties. See [Set Passwords using Environment Variables/System Properties](../../setup/security/replace_passwords_env_variables_sys_properties.md)

```
[database.shared_db]
password = "$ref{db_password}"

[super_admin]
username="admin"
password="$ref{admin_password}"

[keystore.tls]
password = "$ref{keystore_password}" 
alias = "$ref{keystore_password}" 
key_password = "$ref{key_password }"  

[truststore]                  
password = "$ref{keystore_password}" 
```

See the complete list of [configuration parameters](../../references/ei_config_catalog.md).

### Passwords in LOG4j properties
For example, consider the 'log4j.appender.LOGEVENT.password' in the log4j.properties file. You can refer the [encrypted password](#encrypting-passwords) form the log4j.properties file as shown below.

```
log4j.appender.LOGEVENT.password=secretAlias:log4j.appender.LOGEVENT.password
```

## Changing encrypted passwords

To change any password which we have encrypted already, follow the below steps:

1. Be sure to shut down the server.
2. Open a command prompt and go to the MI_HOME/bin/ directory, where the cipher tool scripts (for Windows and Linux) are stored.
3. Execute the following command for your OS:
    * On Linux: `./ciphertool.sh -Dconfigure`
    * On Windows: `./ciphertool.bat -Dconfigure`
4. It will prompt for the primary keystore password. Enter the keystore password (which is "wso2carbon" for the default keystore).
5. The alias values of all the passwords that you encrypted will now be shown in a numbered list.
6. The system will then prompt you to select the alias of the password which you want to change. Enter the list number of the password alias.
7. The system will then prompt you (twice) to enter the new password. Enter your new password.

## Resolving encrypted passwords

There are two ways to resolve encrypted passwords:

> **Note**: If you have secured the plain text passwords in configuration files using Secure Vault, the keystore password and private key password of the product's [primary keystore](../../setup/security/configuring_keystores.md) will serve as the root passwords for Secure Vault. This is because the keystore passwords are needed to initialise the values encrypted by the **Secret Manager** in the **Secret Repository**. Therefore, the **Secret Callback handler** is used to resolve these passwords. The default secret CallbackHandler provides the two options given below. Read more about [Secure Vault concepts](../../references/security/customizing-secure-vault.md).

### Enter password in command-line
1. Start the server by running the product start up script from the MI_HOME/bin/ directory as shown below.
   ```
   ./micro-integrator.sh
   ```
2. When you run the startup script, the following message will be prompted before starting the server: `[Enter KeyStore and Private Key Password:]`. This is because, in order to connect to the default user store, the encrypted passwords should be decrypted. The administrator starting the server must provide the private key and keystore passwords using the command-line. Note that passwords are hidden from the terminal and log files.

### Start server as a background job

If you start the Micro Integrator as a background job, you will not be able to provide password values on the command line. Therefore, you must start the server in "daemon" mode as explained below.

1. Create a new file in the MI_HOME directory. The file should be named according to your OS as explained below.

    * For Linux: The file name should be `password-tmp`.
    * For Windows: The file name should be `password-tmp.txt`.

2. Add the primary keystore password (which is "wso2carbon" by default) to the new file and save. By default, the password provider assumes that both private key and keystore passwords are the same. If not, the private key password must be entered in the second line of the file.

3. Now, start the server as a background process by running the following command.
   ```
   ./micro-integrator.sh start
   ```
4. Start the server by running the product start-up script from the MI_HOME/bin directory by executing the following command:
   ```
   daemon. sh micro-integrator.sh -start
   ```
