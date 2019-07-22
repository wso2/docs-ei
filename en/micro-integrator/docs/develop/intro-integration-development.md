# Developing Integration Solutions

The contents on this page will walk you through the topics related to developing integration solutions for the WSO2 Micro Integrator.

## Development workflow

Integration developers will follow the workflow illustrated by the following diagram.

![developer workflow](../assets/img/development_workflow.png)

<table>
	<tr>
		<td><b>Step 1: Set up the workspace</b></td>
		<td>
			To start developing integration solutions, you need to first set up your workspace:
			<ul>
				<li> <a href="../../develop/installing-WSO2-Integration-Studio">Install WSO2 Integration Studio</a>, which you will use to develop, build, and test your integration artifacts.</li>
				<li><a href="https://www.docker.com/">Install Docker</a> if you want to test your solution in a containerized environment.</li>
				<li><a href="https://curl.haxx.se/">Install CURL</a> to test the integration solution by triggering the integration flow.</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b>Step 2: Develop the artifacts</b></td>
		<td>
			Before you start developing your artifacts, design the synapse configurations that suite your requirement. Use the following resources:
			<ul>
				<li>
					<a href="../../use-cases/guides/using-templates">Guides</a> will walk you through the process of developing the most common integration use cases.
				</li>
				<li>
					<a href="../../use-cases/examples/configuring-endpoints-using-apis">Examples</a> will provide a quick demo that will help you understand the synapse configurations for implementing specific functions.
				</li>
				<li>
					<a href="../../use-cases/tasks/working-with-mediators">Tasks</a> will provide indepth information on developing all the integration artifacts and configurations.
				</li>
				<li>
					See the <a href="../../develop/working-with-WSO2-Integration-Studio">WSO2 Integration Studio documentation</a> for indepth information on the development tool.
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b>Step 3: Build and Run the integration</b></td>
		<td>
			You can easily test your integration flow, either in a container environment, or a VM.</br></br>
			<b>Using a VM</b>: If you want to test the integration flow as a VM deployment, you can instantly deploy the synapse artifacts in the Micro Integrator that is embedded in WSO2 Integration Studio. </br>
			See <a href="../../develop/working-with-WSO2-Integration-Studio/#testing-build-and-run-the-integration"></a>testing the integration solution for instructions.</br></br>
			<b>Using Docker</b>: If you want to test the integration flow in a container environment:
			<ol>
				<li>Be sure that Docker is installed in your machine.</li>
				<li><a href="../../develop/working-with-WSO2-Integration-Studio/#generating-docker-images">Export a Docker Image</a> of your integration artifacts.</li>
				<li>Build and run the Docker image.</li>
			</ol>
		</td>
	</tr>
	<tr>
		<td><b>Step 4: Iterate and improve</b></td>
		<td>
			As you build and run the integration flow, you may identify errors that need to be fixed, and changes that need to be done to the synapse artifacts.
			<ul>
				<li>
					<a href="../../use-cases/tasks/sequences/debugging-mediation">Debug the mediation flow</a> to find potential errors.
				</li>
				<li>
					<a href="../../develop/working-with-WSO2-Integration-Studio/#addredeploy-synapse-artifacts">Redeploy the synapse artifacts</a> after applying changes. </br></br>
					<b>Note</b>: If you are testing on a VM, the artifacts will be instantly deployed. If you are running in a container, you need to <a href="../../develop/working-with-WSO2-Integration-Studio/#generating-docker-images">rebuild the Docker image</a> and deploy it in Docker.
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b>Step 5: Deploy in production</b></td>
		<td>
			To deploy your tested integration solution in your production environment, you can use a <a href="../../develop/using-cicd-pipeline">CICD pipeline</a>. Alternatively, you can <a href="../../develop/working-with-WSO2-Integration-Studio/#packaging-synapse-artifacts">package the integration artifacts</a> and <a href="../../develop/working-with-WSO2-Integration-Studio/#deploying-in-wso2-micro-integrator">deploy the CAR file</a> in the Micro Integrator that is running in production.
		</td>
	</tr>
</table>

<!--

### Step 1: Set up the workspace
To start developing integration solutions, you need to first set up your workspace:

* Install WSO2 Integration Studio, which you will use to develop, build, and test your integration artifacts.
* Install Docker if you want to test your solution in a containerized environment.
* Install the CURL client to test the integration solution by triggering the integration flow.

### Step 2: Develop the artifacts

Before you start developing your artifacts, design the synapse configurations that suite your requirement. Use the following resources:

* [Guides](use-cases/guides/using-templates.md) will walk you through the process of developing the most common integration use cases.
* [Examples](../../use-cases/guides/configuring-endpoints-using-apis.md) will provide a quick demo that will help you understand the synapse configurations for implementing specific functions.
* [Tasks](../../use-cases/tasks/configuring-endpoints-using-apis.md) will provide indepth information on developing all the integration artifacts and configurations. 
* See [Using WSO2 Integration Studio](develop/working-with-WSO2-Integration-Studio.md) for information on the tool that you use for development.

### Step 3: Build and Run the integration
You can easily test your integration flow, either in a container environment, or a VM.

#### Using a VM
If you want to test the integration flow as a VM deployment, you can instantly deploy the synapse artifacts in the Micro INtegrator that is embedded in WSO2 Integration Studio.

See [testing the integration](develop/working-with-WSO2-Integration-Studio#testing-build-and-run-the-integration)

#### Using Docker
If you want to test the integration flow in a container environment:

1. Be sure that Docker is installed in your machine.
2. Export a Docker Image of your integration artifacts. 
3. Build and run the Docker image.

### Step 4: Iterate and improve
As you build and run the integration flow, you may identify errors that need to be fixed, and changes that need to be done to the synapse artifacts.

1. Debug mediation to find potential errors.
2. Redeploy the synapse artifacts after applying changes. 
   > If you are testing on a VM, the artifacts will be instantly deployed. If you are running in a container, you need to rebuild the Docker image and deploy it in Docker.

### Step 5: Deploy in production

To deploy your tested integration solution in your production environment, you can use a CI/CD pipeline. Alternatively, you can package the integration artifacts and deploy the CAR file in the Micro Integrator that is running in production.

-->

## Related topics

<table>
	<tr>
		<td>
			<b><a href="../../develop/integration-development-kickstart">Developer Kick Start</a></b></br></br>
			You can try the development workflow end-to-end by running a simple use case.
		</td>
		<td>
			<b><a href="../../develop/using-cicd-pipeline">Using a CI/CD Pipeline</a></b></br></br>
			Publish your tested integration solution into production using a CI/CD pipeline.
		</td>
		<td>
			<b><a href="../../develop/working-with-WSO2-Integration-Studio">Using WSO2 Integration Studio</a></b></br></br>
			Get familiar with the developer tool for creating your integration solutions.
		</td>
	</tr>
</table>