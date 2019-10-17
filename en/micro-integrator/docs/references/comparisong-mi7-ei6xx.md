# WSO2 Micro Integrator vs WSO2 EI 6.x Family

## Comparison: EI 6.x vs EI 7.0

[WSO2 Enterprise Integrator](https://wso2.com/integration/micro-integrator/) consists of two product families:

-	WSO2 EI 7.x.x family
-	WSO2 EI 6.x.x family
 
[WSO2 EI 6.5.0](https://docs.wso2.com/display/EI650/WSO2+Enterprise+Integrator+Documentation) is the latest release of the 6.x family, which includes the ESB profile and other supporting profiles. This WSO2 EI product family offers the conventional, centralized integration solution of the WSO2 middleware stack. The next release of the 6.x family would be EI 6.6.0 that includes support for JDK11.
 
The new [WSO2 Enterprise Integrator (WSO2 EI 7.0.0)](https://ei.docs.wso2.com/en/latest/) is a hybrid platform that enables API-centric integration supporting various integration architecture styles: MSAs, cloud-native architecture, or a centralized ESB architecture. This integration platform offers the choice between code-driven and graphical/configuration-driven approaches to integration development.
 
The following are the three approaches for doing integration with EI 7.0.0.
 
-   **Code-driven Integration** with **Ballerina Integrator**

    The [Ballerina Integrator](https://ei.docs.wso2.com/en/latest/ballerina-integrator/get-started/introduction/) provides a powerful, simple-to-learn, code-driven approach to programming integration solutions based on the Ballerina programming language. The Ballerina language offers a revolutionary approach to closing the integration gaps often left by traditional ESBs when supporting modern, cloud-native app development. The Ballerina Integrator simplifies integration by offering high-level abstractions that represent services, endpoints, and network data types, along with a sequence diagram-based visualization of interactions. 
 
-   **Low-code Integration** with **Micro Integrator**

    The [Micro Integrator](../../overview/introduction) is an intuitive, configuration-driven integrator with graphical/drag-and-drop integration designing based on the cloud-native variant of the battle-tested WSO2 EI/ESB runtime.
 
-   **Streaming Integration**

    The [Streaming Integrator](https://ei.docs.wso2.com/en/latest/streaming-integrator/overview/overview/) is a cloud-native, lightweight component that understands streaming SQL queries that capture, analyze, process, and act on events in real-time.

## Advantages of using the Micro Integrator in EI 7.0

Compared to the ESB profile of WSO2 Enterprise Integrator 6.x, the Micro Integrator encompasses the following key attributes that are essential for a microservice-ready integration solution.

-	Faster startup time (<3s).
-	Low memory footprint.
-	Stateless services.
-	Immutable services.
-	Native Kubernetes support with the integration operator. 
-	Lightweight, container-native, and supports distributed deployments. 

## Comparison: ESB profile of EI 6.x vs Micro Integrator of EI 7.0

Given below is a comparision between the Micro Integrator of EI 7.0 and the ESB profile of EI 6.x.

<table>
	<tr>
		<th></th>
		<th>ESB Profile</th>
		<th>Micro Integrator</th>
	</tr>
	<tr>
		<td>
			Startup Time
		</td>
		<td>
			40s
		</td>
		<td>
			4s
		</td>
	</tr>
	<tr>
		<td>
			Distribution Size
		</td>
		<td>
			~600 MB
		</td>
		<td>
			~100 MB
		</td>
	</tr>
	<tr>
		<td>
			Mediation (ESB) Features
		</td>
		<td>
			Available
		</td>
		<td>
			Available
		</td>
	</tr>
	<tr>
		<td>
			Data Integration Features
		</td>
		<td>
			Available
		</td>
		<td>
			Available
		</td>
	</tr>
	<tr>
		<td>
			Clustering
		</td>
		<td>
			Built-in
		</td>
		<td>
			Using container orchestration
		</td>
	</tr>
	<tr>
		<td>
			Tooling
		</td>
		<td>
			<a href="../../develop/WSO2-Integration-Studio">WSO2 Integration Studio</a>
		</td>
		<td>
			<a href="../../develop/WSO2-Integration-Studio">WSO2 Integration Studio</a>
		</td>
	</tr>
	<tr>
		<td>
			Runtime monitoring and management
		</td>
		<td>
			Managemement Console
		</td>
		<td>
			<a href="../../administer-and-observe/working-with-monitoring-dashboard">Micro Integrator Dashboard</a></br>
			<a href="../../administer-and-observe/using-the-command-line-interface">Micro Integrator CLI</a>
		</td>
	</tr>
	<tr>
		<td>
			CAR Deployment
		</td>
		<td>
			Available
		</td>
		<td>
			Available
		</td>
	</tr>
	<tr>
		<td>
			Registry
		</td>
		<td>
			RDBMS-based Registry
		</td>
		<td>
			<a href="../../setup/deployment/file_based_registry">File system based Registry</a>
		</td>
	</tr>
	<tr>
		<td>
			Artifact deployment
		</td>
		<td>
			Hot deployable,</br>
			Hot updatable
		</td>
		<td>
			Immutable artifacts
		</td>
	</tr>
</table>

## Features removed from the Micro Integrator of EI 7.0

The following features, which are not needed for MSA-based deployments or not used frequently are removed from WSO2 Micro Integrator.

<table>
	<tr>
		<th>
			Feature
		</th>
		<th>
			Description
		</th>
		<th>
			Alternative
		</th>
	</tr>
	<tr>
		<td>
			Management Console
		</td>
		<td>
			<a href="../../develop/WSO2-Integration-Studio">WSO2 Integration Studio</a> is the recommended tool for developing integration solutions. The monitoring capabilities available in the management console (of the ESB profile) are available through the new <a href="../../administer-and-observe/working-with-monitoring-dashboard">Micro Integrator dashboard</a>.
		</td>
		<td>
			<a href="../../administer-and-observe/working-with-monitoring-dashboard">Micro Integrator Dashboard</a>
		</td>
	</tr>
	<tr>
		<td>
			Multitenancy
		</td>
		<td>
			This is not a widely used feature in the ESB profile. Multitenancy does not suit the world of microservices or micro integrations.
		</td>
		<td>
			-
		</td>
	</tr>
	<tr>
		<td>
			Svn based Dep-sync
		</td>
		<td>
			This is not a widely used feature in the ESB profile, and is not recommended for use.
		</td>
		<td>
			Third-party offering like <b>rsync</b>
		</td>
	</tr>
	<tr>
		<td>
			Hot deployment/Hot update
		</td>
		<td>
			Services and artifacts are immutable.
		</td>
		<td>
			-
		</td>
	</tr>
	<tr>
		<td>
			Built-in Hazelcast-based clustering
		</td>
		<td>
			The microservices world typically handles clustering using a container orchestration systems like Kubernetes.</br></br>However, built-in clustering support will be added to the Micro Intgrator in a future version to support standard, centralized deployment architectures.
		</td>
		<td>
			-
		</td>
	</tr>
	<tr>
		<td>
			Built-in node coordination feature
		</td>
		<td>
			Coordination support for message processors, inbound endpoints, and tasks, which is built into the ESB profile, is removed from the Micro Integrator. This feature is not a requirement in the microservices world.</br></br> However, coordination support will be added to the Micro Integrator in a future version to support standard, centralized deployment architectures.
		</td>
		<td>
			-
		</td>
	</tr>
	<tr>
		<td>
			Database-based Registry
		</td>
		<td>
			-
		</td>
		<td>
			File system based Registry
		</td>
	</tr>
</table>
