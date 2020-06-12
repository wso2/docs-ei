# Encrypting Secrets

WSO2 Micro Integrator can use secrets with static origins as well as dynamic origins in configurations. This applies to secrets in server-level configurations as well as configurations within integration solutions (synapse configurations).

## Static Secrets

Static secrets are sensitive data that are specified directly in configurations. The secret can be a plain-text value or the alias of an encrypted value.

Because it is not recommended to use plain-text values for your sensitive data, you can encrypt the plain-text secrets by using the secure vault implementation that is built in to the Micro Integrator. The Micro Integrator is shipped with the **Cipher Tool**, which uses the secure vault implementation and encrypts the secrets you specify.

Note that you can customize the default secure vault configurations in the product by implementing a new secret repository, call back handler, etc.

### Encrypting static secrets

!!! Tip
    - If you are using **Windows**, you need to have [Ant](http://ant.apache.org/) installed before using the Cipher Tool.

You must first list the plain-text secrets in the `deployment.toml` file under the `secrets` header. When you run the Cipher Tool, these plain-text secrets will be encrypted. You can then refer the encrypted secrets from anywhere in your server configurations (also specified in the `deployment.toml` file) or synapse configurations (such as **proxy services** and **rest API** artifacts).

Let's generate some encrypted values to represent the most common secrets in server configurations.

1. Open the `deployment.toml` file located in the `<MI_HOME>/conf/` directory and add the `[secrets]` configuration section as shown below.

    Note that we have specified an alias for the secret type followed by the actual plain-text secret.

    ```toml
    [secrets]
    keystore_password = "[password_3]"
    key_password = "[password_4]"
    truststore_password = "[password_5]"
    ```

2. Open a terminal, navigate to the `<MI_HOME>/bin/` directory.
3. Enable the Cipher tool by executing the `-Dconfigure` command with the cipher tool script as shown below.

    ```bash tab='On Linux'
    ./ciphertool.sh -Dconfigure
    ```

    ```bash tab='On Windows'
    ./ciphertool.bat -Dconfigure
    ```

3. Now, go back to the `deployment.toml` file and see that the secrets are encrypted.

    ```toml
    [secrets]
    keystore_password = "encrypted_pass_3"
    key_password = "encrypted_pass_4"
    truststore_password = "encrypted_pass_5"
    ```

### Using encrypted (static) secrets
To use encrypted secrets (static) in server configurations, add the alias of the encrypted secret to the relevant configuration in the `deployment.toml` (instead of the plain-text secret). The  `$secret` place holder is used for holding the value as shown below.

```toml
[keystore.tls]
password = "$secret{keystore_password}"
alias = "$secret{keystore_password}"
key_password = "$secret{key_password }"  

[truststore]                  
password = "$secret{truststore_password}"
```

To use static encrypted secrets in synapse configurations, see [using encrypted synapse secrets](../../../develop/creating-artifacts/encrypting-synapse-passwords).

## Dynamic Secrets

If you want to dynamically populate secrets (as an environment variable, system property, Docker secret, or Kubernetes secret), you have to encrypt the secrets separately using the WSO2 Micro Integrator CLI tool.

A place holder specified in the `deployment.toml` file is then used to fetch the secret during runtime.

### Generating encrypted secrets

To encrypt secrets using the CLI tool:

1.  [Download](https://wso2.com/integration/micro-integrator/tooling/) and setup the Micro Integrator CLI tool.

2.  Go to the Micro Integrator CLI tool Home folder and Navigate to the bin folder <MI-CLI>/bin

3.  Initialize the CLI tool from your command line:

    ```bash
    mi
    ```
4.  Initialize the secret creation process in the tool:

    ```bash
    mi secret init
    ```

    You will be prompted to provide details of the keystore that will be used for encryption.

    - Keystore location
    - keystore type
    - keystore alias
    - Keystore password

5.  Execute one of the following commands to generate the secret:

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
6. Populate the secret into the relevant environment as required. For an instance, in the case of environment variables, you can populate them with the export command as follows.
    ```bash
    export env_carbon_sec=<ENCRYPTED_VALUE>
    ```
7. Open the `deployment.toml` file located in the `<MI_HOME>/conf/` directory and add the `[secrets]` configuration section as shown below.

    ```toml
   [secrets]
   carbon_secret = "$env{env_carbon_sec}"
    ```
    
8. Run the Cipher tool as explained in the previous section to enable security.

### Using encrypted (dynamic) secrets

Dynamic secrets can be used in server configurations the same way as described in the `Using encrypted (static) secrets` section with the `$secret` placeholder. 

```toml tab='Environment Variable'
[keystore.tls]
file_name = "wso2carbon.jks"
password = "$secret{carbon_secret}"
alias = "$secret{carbon_secret}"
key_password = "$secret{carbon_secret}"

[secrets]
carbon_secret = "$env{env_carbon_sec}"
```

Given below is an instance of populating the encrypted value as a system variable in the environment and referring it in the `deployment.toml` file.

```toml tab='System Property'
[keystore.tls]
file_name = "wso2carbon.jks"
password = "$secret{carbon_secret}"
alias = "$secret{carbon_secret}"
key_password = "$secret{carbon_secret}"

[secrets]
carbon_secret = "$sys{sys_carbon_sec}"
```

Also note that each time a new secret is added to the [secrets] section of deployment.toml file
you should run the Cipher Tool. 

Read more about [using dynamic server configurations](../../../setup/dynamic_server_configurations).
