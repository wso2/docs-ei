# Using Docker Secrets


WSO2 Micro Integrator comes with a built-in secret repository as a part of its secure vault implementation by default. In addition to this, the Micro Integrator also provides built-in support for Docker secrets and Kubernetes secrets for your containerized deployments.

Managing sensitive information in a Docker environment can be achieved with Docker secrets using two simple steps:

1.	Adding the secret to your Docker environment.
2.	Accessing the secret from within your synapse configurations.

There are two ways to add secrets to a Docker environment: Create the Docker secret directly inside the environment, or store the secrets in a flat file and add the file to environment.

## Creating a secret in the Docker environment

Follow the steps given below to directly add the required secrets to the Docker environment.

### Step 1: Creating Docker secrets

You can create a Docker secret in the Docker environment by using the following command:

!!! Tip
		Note that you have to provide this secret to the Docker service at time out service creation so that it will be copied into the container.

```bash
echo "dockersecret123456" | docker secret create testdockersecret -
```
This command will create a docker secret named `testdockersecret` in your Docker environment.

### Step 2: Using Docker secrets in Synapse configurations

Secret can be accessed from the integration artifacts by using the `wso2:vault-lookup` function in the following format.

```bash
wso2:vault-lookup('<alias>', '<type>', '<isEncrypted>')
```

Specify values for the following three parameters:

-	`<alias>`: Name of the secret.
- `<type>`: Set this to `DOCKER`
-	`<isEncrypted>`: Set this to `true` or `false` to specify whether the secret is encrypted.

Given below is a sample synapse configuration that accesses and prints the docker secret we declared in the previous step.

```xml
<property expression="wso2:vault-lookup('testdockersecret', 'DOCKER', 'false')" name="secret"/>
```

## Adding Docker secrets from a flat file

Instead of creating the Docker secret directly in the Docker environment, you can add Docker secrets to the environment by adding a flat file that contains the secrets.

### Step 1: Adding the flat file

Follow the steps given below.

1.	Create a flat file with the secrets.
2.	Add the flat file to the **Resources** folder in your Docker Exporter project. This adds the file to your Docker image.
3.	Add the following line to the Dockerfile in your Docker Exporter project. This copies the secret file to the `<MI_HOME>` directory.

	```bash
	COPY Resources/FLAT_FILE_NAME ${WSO2_SERVER_HOME}/
	```
	
4.  Build the Docker image from the Docker Exporter project.

### Step 2: Using Docker secrets in Synapse configurations

Secret can be accessed from the integration artifacts by using the `wso2:vault-lookup` function in the following format.


```bash
wso2:vault-lookup('<alias>', '<type>', '<isEncrypted>')
```


Specify values for the following three parameters:

-	`<alias>`: Name of the file.
- 	`<type>`: Set this to `FILE`
-	`<isEncrypted>`: Set this to `true` or `false` to specify whether the secret is encrypted.

Given below is a sample synapse configuration that accesses and prints the docker secret we declared in the previous step.

```xml
<property expression="wso2:vault-lookup('testsecret', 'FILE', 'false')" name="secret"/>
```

### Step 3: Enabling secrets in the environment

Once the secrets are added to the environment, you need to enable <b>secure vault</b> in the environment. In a <b>Docker environment</b> you don't need to manually run the Cipher tool. Follow the steps given below.

1. Open your Integration Project in WSO2 Integration Studio, which contains all the integration artifacts and the Docker Exporter.
2. Open the `pom.xml` of the Docker Exporter module and select the <b>Enable Cipher Tool</b> check box as show below.

    <img src="../../assets/img/enable-cipher-tool-in-docker.png">

3.  When you build the Docker image from your Docker exporter, the secrets will get enabled in the environment.

## Configuring the secrets' location
The Docker secrets exist in default directory locations in the container based on the operating system. Therefore, by default, the server will search for aliases in these directories. The default location for the file secret would be `<MI-HOME>/` directory.

However, if you are storing your secrets in a different location, you can configure the server to search the secrets in the custom directories you have used by using the following system properties.

-	Path to the custom directory storing the docker secret:

	```bash
	ei.secret.docker.root.dir
	```

-	Path to the custom directory storing the flat file secrets:

	```bash
	ei.secret.file.root.dir
	```
