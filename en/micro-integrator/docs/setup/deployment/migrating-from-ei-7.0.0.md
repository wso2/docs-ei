# Migrating from WSO2 EI 7.0.0 to WSO2 EI 7.1.0

This guide explains the recommended strategy for migrating from the Micro Integrator of WSO2 EI 7.0.0 to the Micro Integrator of WSO2 EI 7.1.0.

### Set up the migration

-	Make a backup of the current EI 7.0.0 deployment. This backup is necessary in case the migration causes any issues.
-	Download and install EI 7.1.0 in your environment. The home directory of your Micro Integrator will be referred to as `<MI_HOME>` from hereon.
	-	Install the product [using the Installer](../../../setup/installation/install_in_vm_installer)
	-	Install the product [using the binary distribution](../../../setup/installation/install_in_vm_binary)
-	You can use [WSO2 Update Manager](https://docs.wso2.com/display/updates/) to get the latest available updates.

	!!! Info
		Note that you need a valid [WSO2 subscription](https://wso2.com/subscription) to use updates in a production environment.

### Migrating user stores
You can use the same user stores that you used for the Micro Integrator of EI 7.0.0 with EI 7.1.0. Simply copy the user store configurations in your WSO2 EI 7.0.0 instance to the `<MI_7.1.0_HOME>/conf/deployment.toml` file in EI 7.1.0. See the instructions on [configuring a user store](https://ei.docs.wso2.com/en/7.1.0/micro-integrator/setup/user_stores/setting_up_a_userstore/).

### Migrating the registry
The Micro Integrator uses a [file based registry](../file_based_registry). You can directly migrate the artifacts to the EI 7.1.0 by adding the carbon applications to the `MI-HOME/repository/deployment/server/carbonapps` folder. 

### Migrating artifacts
Copy the contents inside `<MI_7.0.0_HOME>/repository/deployment` to the `<MI_7.1.0_HOME>/repository/deployment` folder.

### Migrating custom components
Copy the jars inside the `<MI_7.0.0_HOME>/dropins` folder to the same folder of EI 7.1.0 (`<MI_7.1.0_HOME>/dropins`). The custom JARs can be copied to the `<MI_7.1.0_HOME>/lib` directory.

### Migrating keystores
Copy the JKS files from the `<MI_7.0.0_HOME>/repository/resources/security` folder to the `<MI_7.1.0_HOME>/repository/resources/security` folder.

### Migrating configurations
Copy the configurations in the `deployment.toml` file of EI 7.1.0 (such as database, transport, datasource configurations, etc.) to the `<MI_7.1.0_HOME/conf/deployment.toml` file. 

### Migrating encrypted passwords

In version 7.0.0, **secure vault** was used to store sensitive information used in synapse configurations and the **cipher tool** was used for encrypting sensitive server information. In EI 7.1.0, all the sensitive information (in server configurations as well as synapse configuration) can simply be encrypted and stored using the cipher tool, without having to differentiate.

We provide a migration tool, which allows you to decrypt the existing passwords in secure vault as well as cipher tool.The decrypted plain-text values can then be added to the [secrets] section of the `deployment.toml` file of the Micro Integrator of EI 7.1.0 and re-encrypted by running the cipher tool. Follow the instructions given below.

1. Download the [migration tool](https://github.com/wso2-docs/WSO2_EI/tree/master/migration-client).

2. Get the latest update for your existing EI 7.0.0 distribution by using [WSO2 Update Manager](https://docs.wso2.com/display/updates/).

	!!! Info
		Note that you need a valid [WSO2 subscription](https://wso2.com/subscription) to use updates in a production environment.

3. Copy the `org.wso2.mi.migration-1.2.0-SNAPSHOT.jar` into the `MI_7.0.0_HOME/dropins` folder.

4. Start the server using the `migrate.from.product.version` system property as follows:

	```bash tab='On Linux/Unix'
	sh micro-integrator.sh -Dmigrate.from.product.version=110
	```
	
	```bash tab='On Windows'
	micro-integrator.bat -Dmigrate.from.product.version=110
	```

5. Upon successful execution, the decrypted (plain-text) values in the `secure-vault.properties` and `cipher-text.properties` files will be written respectively to `MI_7.1.0_HOME/migration/secure-vault-decrypted.properties` file and the `MI_7.1.0_HOME/migration/cipher-text-decrypted.properties` file. 
