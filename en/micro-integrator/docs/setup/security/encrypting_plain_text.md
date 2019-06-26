# Encrypting Passwords in Configuration files

The instructions on this page explain how plain text passwords in configuration files can be encrypted using the secure vault implementation that is built into WSO2 products. Note that you can customize the default secure vault configurations in the product by implementing a new secret repository, call back handler etc. Read more about the Secure Vault implementation in WSO2 products.

## Before you begin
If you are using Windows, you need to have Ant (http://ant.apache.org/) installed before using the Cipher Tool.

## Encrypting passwords

1. Open the ei.toml file and add the following configuration section as shown below. Give an alias for the information type that you want to encrypt followed by the actual value.
```
[secrets]
foo = "[R!#etoqe]"
bar = "[T@h3rerqr]"
```
2. Open a terminal, navigate to the <EI_HOME>/bin/ directory, and execute the following command (You must first enable the Cipher tool for the product by executing the -Dconfigure command with the cipher tool script as shown below).
    * On Linux: ./ciphertool.sh -Dconfigure
    * On Windows: ./ciphertool.bat -Dconfigure

3. Go back to the ei.toml file and see that the alias passwords are encrypted.
```
[secrets]
foo = "qf24n82l48gl=3o42nfmakr83b5y3"
bar = "02jehlqnghfbat54oqhwcxeqorm2o"
```

## Using encrypting Passwords
Once you have encrypted password values, you can refer them from the relevant configuration sections in the ei.toml file. For example, if you have encrypted the admin password of your server, update the code `[super_admin]` config section in the ei.toml file as shown below.

```
[super_admin]
username="admin"
password="$ref{alias}"
```

## Changing encrypted passwords

To change any password which we have encrypted already, follow the below steps:

1. Be sure to shut down the server.
2. Open a command prompt and go to the <PRODUCT_HOME>/bin directory, where the cipher tool scripts (for Windows and Linux) are stored.
3. Execute the following command for your OS:
    * On Linux: ./ciphertool.sh -Dconfigure
    * On Windows: ./ciphertool.bat -Dconfigure
4. It will prompt for the primary keystore password. Enter the keystore password (which is "wso2carbon" for the default keystore).
5. The alias values of all the passwords that you encrypted will now be shown in a numbered list.
6. The system will then prompt you to select the alias of the password which you want to change. Enter the list number of the password alias.
7. The system will then prompt you (twice) to enter the new password. Enter your new password.

## Resolving encrypted passwords

If you have secured the plain text passwords in configuration files using Secure Vault, the keystore password and private key password of the product's primary keystore will serve as the root passwords for Secure Vault. This is because the keystore passwords are needed to initialise the values encrypted by the Secret Manager in the Secret Repository. Therefore, the Secret Callback handler is used to resolve these passwords. Read about the Secure Vault implementation in WSO2 products. Also, see how passwords in configuration files are encrypted using Secure Vault.

The default secret CallbackHandler in a WSO2 product provides two options for reading these encrypted passwords when you start the server:

### Enter password in command-line
1. Start the server by running the product start up script from the <PRODUCT_HOME>/bin/ directory as shown below.
   ```
   ./wso2server.sh
   ```
2. When you run the startup script, the following message will be prompted before starting the server: "[Enter KeyStore and Private Key Password :]". This is because, in order to connect to the default user store, the encrypted passwords should be decrypted. The administrator starting the server must provide the private key and keystore passwords using the command-line. Note that passwords are hidden from the terminal and log files.

### Start server as a background job

If you start the WSO2 Carbon server as a background job, you will not be able to provide password values on the command line. Therefore, you must start the server in "daemon" mode as explained below.

1. Create a new file in the <PRODUCT_HOME> directory. The file should be named according to your OS as explained below.

    * For Linux: The file name should be password-tmp.
    * For Windows: The file name should be password-tmp.txt.

2. Add "wso2carbon" (the primary keystore password) to the new file and save. By default, the password provider assumes that both private key and keystore passwords are the same. If not, the private key password must be entered in the second line of the file.

3. Now, start the server as a background process by running the following command.
   ```
   ./wso2server.sh start
   ```
4. Start the server by running the product start-up script from the <PRODUCT_HOME>/bin directory by executing the following command:
   ```
   daemon. sh wso2server.sh -start
   ```
