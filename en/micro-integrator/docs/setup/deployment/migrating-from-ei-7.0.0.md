# Migrating from WSO2 EI 7.0.0 to WSO2 EI 7.1.0

### Set up the migration

-	Make a backup of the existing database used by the current EI 7.0.0 deployment. This backup is necessary in case the migration causes any issues in the existing database.
-	Download and install EI 7.1.0 in your environment. The home directory of your Micro Integrator will be referred to as `<MI_HOME>` from hereon.
	-	[Using the Installer](../../../setup/installation/install_in_vm_installer)
	-	[Using the binary distribution](../../../setup/installation/install_in_vm_binary)
-	You can [use WSO2 Update Manager](https://docs.wso2.com/display/updates/) to get the latest available updates.

	!!! Info
		Note that you need a valid [WSO2 subscription](https://wso2.com/subscription) to use updates in a production environment.

### Migrating databases
You can use the same databases that you used for the EI 7.0.0 of WSO2 with WSO2 EI 7.1.0. Simply copy the database configurations to the `MI-HOME/conf/deployment.toml` file as you did in WSO2 EI 7.0.0. 

### Migrating the registry
The Micro Integrator uses a [file based registry](../file_based_registry). You can directly migrate the artifacts to the EI 7.1.0 by adding the carbon applications to the `MI-HOME/repository/deployment/server/carbonapps` folder. 

### Migrating artifacts
Copy the contents inside `<MI_7.0.0_HOME>/repository/deployment` to the `<MI_7.1.0_HOME>/repository/deployment` folder.

### Migrating custom components
Copy the jars inside the `<MI_7.0.0_HOME>/dropins` folder to the same folder of EI 7.1.0 (`<MI_7.1.0_HOME>/dropins`). The custom JARs can be copied to the `<MI_7.1.0_HOME>/lib` directory.

### Migrating keystores
Copy the JKS files to the `<MI_7.1.0_HOME>/repository/resources/security` directory.

### Migrating configurations
Copy the configurations in the `deployment.toml` file of EI 7.1.0 (such as database, transport, datasource configurations, etc.) to the `<MI_7.1.0_HOME/conf/deployment.toml` file. 

### Migrating encrypted passwords
This guide provides an overview of the recommended migration strategy for migrating from WSO2 EI 7.0.0 to WSO2 EI 7.1.0. In version 7.0.0, secure-vault was used to store sensitive information used in synapse configurations and cipher-tool was used for sensitive server information. In WSO2 EI 7.1.0, all the sensitive information (server related and synapse configuration) can simply be encrypted and stored using cipher-tool without having to differentiate.

This tool allows you to decrypt the existing passwords in secure-vault as well as cipher-tool. In a case of migrations, you can use this migration tool to get the plain text values of your existing passwords. The obtained plain-text values can be added into the [secrets] section of the deployment.toml of WSO2 Micro-Integrator-1.2.0 and get re-encrypted by running the ciphertool.sh -Dconfigure command from the /bin directory.

1. Download the migration service jar from [here](https://github.com/wso2-docs/WSO2_EI/tree/master/migration-client).

2. WUM Update your existing MI-1.1.0 distribution. (if not already updated)

3. Copy the org.wso2.mi.migration-1.2.0-SNAPSHOT.jar into MI-HOME/dropins folder.

4. Start the server with the migrate.from.product.version system property set as follows. 
   a. Linux/Unix.
   - `sh micro-integrator.sh -Dmigrate.from.product.version=110`

    b. Windows.
   - `micro-integrator.bat -Dmigrate.from.product.version=110`

5. Upon successful execution the decrypted (plain-text) values of secure-vault.properties and cipher-text.properties will be written respectively to EI-HOME/migration/secure-vault-decrypted.properties and EI-HOM/migration/cipher-text-decrypted.properties. 
