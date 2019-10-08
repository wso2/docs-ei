# Developing Integration Solutions

The contents on this page will walk you through the topics related to developing integration solutions using WSO2 Micro 
Integrator.

## Development workflow

Integration developers will follow the workflow illustrated by the following diagram.

![developer workflow](../../assets/img/development_workflow.png)

<table>
	<tr>
		<td><b>Step 1: Set up the workspace</b></td>
		<td>
			To start developing integration solutions, you need to first set up your workspace.</br>
			<a href="../../develop/installing-WSO2-Integration-Studio">Install WSO2 Integration Studio</a>, which you will use to develop, build, and test your integration artifacts.
		</td>
	</tr>
	<tr>
		<td><b>Step 2: Develop the artifacts</b></td>
		<td>
			Before you start developing your artifacts, design the synapse configurations that suite your requirement. Use the following resources:
			<ul>
				<li>
					See the <a href="../../use-cases/integration-use-cases">Use cases</a> documentation,                 which contains:
					 <ul>
					    <li>
						    <b>Tutorials</b> that will walk you through the process of developing the most                             common integration use cases. 
				        </li>
				        <li>
						<b>Examples</b> providing a quick demo that will help you understand the                                synapse configurations for implementing specific functions.
				        </li>
				     </ul>
				</li>
				<li>
				    See the documentation on <a href="../../references/best-Practices">developing 
				    common integration patterns</a> to get an understanding on the best practices that should be 
				    followed while developing integration solutions for the Micro Integrator.
				</li>
				<li>
					See the <a href="../../develop/WSO2-Integration-Studio">WSO2 Integration Studio documentation</a> for in-depth information on the development tool.
				</li>
				<li>
				    See the documentation on <a href="../../develop/creating-unit-test-suite">creating unit tests</a> for the integration solution you developed.
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td><b>Step 3: Build and Run the integration</b></td>
		<td>
			Once you have developed your integration solution,
			<ol>
				<li>
					<a href="../../develop/packaging-artifacts">Package the integration artifacts</a>
				</li>
				<li>
					<a href="../../develop/deploy-and-run">Build and run</a> the integration artifacts in the Micro Integrator that is embedded in WSO2 Integration Studio.
				</li>
			</ol>
		</td>
	</tr>
	<tr>
		<td><b>Step 4: Iterate and improve</b></td>
		<td>
			You can easily test your integration flow either in a container environment, or a VM.
			<ul>
				<li>
					Use the <a href="../../develop/creating-unit-test-suite/#run-unit-test-suites">Integration Test Suite</a> in WSO2 
					Integration Studio to run unit testing on your integration artifacts.
				</li>
				<li>
					To test the integration flow in <b>Docker</b>, <a href="../../develop/create-docker-project">create a Docker project</a> and push it to your Docker environment.
				</li>
				<li>
					To test the integration flow in <b>Kubernetes</b>, <a href="../../develop/create-kubernetes-project">create a Kubernetes project</a> and push it to your Kubernetes environment.
				</li>
				<li>
					To test the integration flow as a <b>VM deployment</b>, you can instantly <a href="../../develop/deploy-and-run">build and run</a> the integration artifacts in the Micro Integrator that is embedded in WSO2 Integration Studio.
				</li>
			</ul>
			As you build and run the integration flow, you may identify errors that need to be fixed, and changes that need to be done to the synapse artifacts.
			<ol>
				<li>
					<a href="../../develop/debugging-mediation">Debug the mediation flow</a> to find potential errors.
				</li>
				<li>
					Redeploy the integration artifacts after applying changes.</br></br>
					<b>Note</b>: If you are testing on a VM, the artifacts will be instantly deployed when you <a href="../../develop/deploy-and-run">Redeploy the synapse artifacts</a>. If you are testing on containers, you need to rebuild the <a href="../../develop/create-docker-project">Docker images</a> or <a href="../../develop/create-kubernetes-project">Kubernetes artifacts</a>.
				</li>
			</ol>
		</td>
	</tr>
	<tr>
		<td><b>Step 5: Deploy in production</b></td>
		<td>
			It is recommended to use a <b>CICD pipeline</b> to deploy your tested integration solutions in the production environment.</br></br>
			WSO2 Integration Studio handles the <b>continous integration</b> of your solutions by generating the 
			artifacts that need to be pushed to a remote artifact repository for the continous deployment process: 
			<ul>
				<li>
					<b>Docker images</b> in the <a href="../../develop/create-docker-project">Docker project</a> and <b>Kubernetes artifacts</b> in the <a href="../../develop/create-kubernetes-project">Kubernetes project</a> can be directly pushed to the remote artifact repository.
				</li>
				<li>
					If you are using a <b>VM deployment</b>, <a href="../../develop/packaging-artifacts">package the integration artifacts</a> and deploy the CAR file in the remote repository.
				</li>
			</ul>
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
			Develop your <b><a href="../../develop/integration-development-kickstart">first integration solution</a></b>.</br></br>
			You can try the development workflow end-to-end by running a simple use case.
		</td>
		<!--
		<td>
			<b><a href="../../develop/using-cicd-pipeline">Using a CI/CD Pipeline</a></b></br></br>
			Publish your tested integration solution into production using a CI/CD pipeline.
		</td>
	-->
		<td>
			<b><a href="../../develop/WSO2-Integration-Studio">Using WSO2 Integration Studio</a></b></br></br>
			Get familiar with the developer tool for creating your integration solutions.
		</td>
	</tr>
</table>
