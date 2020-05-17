# Encrypting Secrets in Server Configurations

WSO2 Micro Integrator can use secrets with static origins as well as dynamic origins.

## Static Secrets

Static secrets in server configurations, such as database passwords and keystore passwords, are defined directly in the `deployment.toml` file. The secret can be a plain text value or the alias of an encrypted value.

You can encrypt static secrets by using the secure vault implementation that is built in. These secrets may include passwords and other sensitive data that you would otherwise specify as plain text values.

!!! Note
    This approach of encrypting static secrets in server configurations can only be applied to VM deployments.

Note that you can customize the default secure vault configurations in the product by implementing a new secret repository, call back handler, etc.

### Encrypting static secrets

!!! Tip
    If you are using **Windows**, you need to have [Ant](http://ant.apache.org/) installed before using the Cipher Tool.

1. Open the `deployment.toml` file located in `<MI_HOME>/conf/` directory and add the `[secrets]` configuration section as shown below. Give an alias for the password type followed by the actual password. The following example lists the most common passwords in configuration files.

    ```toml
    [secrets]
    keystore_password = "[password_3]"
    key_password = "[password_4]"
    truststrore_password = "[password_5]"
    ```

    See the complete list of [configuration parameters](../../references/config-catalog.md#secret-passwords).

2. Open a terminal, navigate to the `<MI_HOME>/bin/` directory.
3. Enable the Cipher tool by executing the `-Dconfigure` command with the cipher tool script as shown below.

    ```bash tab='On Linux'
    ./ciphertool.sh -Dconfigure
    ```

    ```bash tab='On Windows'
    ./ciphertool.bat -Dconfigure
    ```

3. Go back to the `deployment.toml` file and see that the alias passwords are encrypted.

    ```toml
    [secrets]
    keystore_password = "encrypted_pass_3"
    key_password = "encrypted_pass_4"
    truststrore_password = "encrypted_pass_5"
    ```

    See the complete list of [configuration parameters](../../references/config-catalog.md#secret-passwords).

### Using the encrypted secrets
When you have [encrypted secrets](#encrypting-passwords) using secure vault, you can refer them from the `deployment.toml` file by using the alias of the encrypted value instead of the plain-text password. The  `$secret` place holder is used for holding the value as shown below.

```toml
[keystore.tls]
password = "$secret{keystore_password}"
alias = "$secret{keystore_password}"
key_password = "$secret{key_password }"  

[truststore]                  
password = "$secret{keystore_password}"
```

See the complete list of [configuration parameters](../../references/config-catalog.md).

!!! Tip
    **Using server secrets in synapse configurations**
    A static secret defined in the `delpoyment.toml` file can also be used within your synapse configurations when creating your integration scenarios.

## Dynamic Secrets

If you want to dynamically populate secrets (as an environment variable, system property, Docker secret, or Kubernetes secret), you have to encrypt the secrets separately. WSO2 Micro Integrator CLI tool provides out of the box support for secret encryption.

A place holder specified in the `deployment.toml` file is then used to fetch the secret during runtime.

Dynamic secrets are useful when you want to manage your product configurations in multiple environments.

### Generating encrypted secrets

1.  [Download](https://wso2.com/integration/micro-integrator/tooling/) and setup the Micro Integrator CLI tool.
2.  Initialize the CLI tool from your command line:

    ```bash
    ./mi
    ```
3.  Initialize the secret creation process in the tool:

    ```bash
    mi secret init
    ```

    You will be prompted to provide details of the keystore that will be used for encryption.

    - Keystore location
    - keystore type
    - keystore alias
    - Keystore password

4.  Execute one of the following commands to generate the secret:

    ```bash
    # To encrypt secret and get output to console
    mi secret create

    # To encrypt secret and get output to file
    mi secret create file

    # To encrypt secret and get output as a .yaml file
    mi secret create k8

    # To bulk encrypt secrets defined in a properties file
    mi secret create -f=</file_path>
    ```

### Using the generated secrets

Update the secret values in the `deployment.toml` file with the relevant place holder as shown below.

```toml tab='Environment Variable'
[keystore.primary]
password = "$env{ENV_VAR}"
alias = "$env{ENV_VAR}"
key_password = "$$env{ENV_VAR}"  

[truststore]                  
password = "$env{ENV_VAR}"
```

```toml tab='System Property'
[keystore.primary]
password = "$sys{system.property}"
alias = "$sys{system.property}"
key_password = "$sys{system.property}"  

[truststore]                  
password = "$sys{system.property}"
```
