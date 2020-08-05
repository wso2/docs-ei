# WSO2 Micro Integrator vs WSO2 EI 6.x Family

## Comparison: EI 6.x vs EI 7.0

[WSO2 Enterprise Integrator](https://wso2.com/platform) consists of two product families:

-	WSO2 EI 7.x.x family
-	WSO2 EI 6.x.x family
 
This [WSO2 EI 6.x product family](http://docs.wso2.com/enterprise-integrator) offers the conventional, centralized integration solution of the WSO2 middleware stack. WSO2 EI 6.x includes the ESB profile and other supporting profiles packaged in a single distribution.
 
The new WSO2 Enterprise Integrator (WSO2 EI 7.1.0) is a hybrid platform that enables API-centric integration supporting various integration architecture styles: microservices architecture, cloud-native architecture, or a centralized ESB architecture. This integration platform offers a graphical/configuration-driven approach to developing integrations for any of the architectural styles.
 
The following are the approaches to integration with EI 7.1.0.
 
-   **Low-code Integration** with **Micro Integrator**

    The [Micro Integrator](../../overview/introduction) is an intuitive, configuration-driven integrator with graphical/drag-and-drop integration designing based on the cloud-native variant of the battle-tested WSO2 EI/ESB runtime.
 
-   **Streaming Integration**

    The [Streaming Integrator](https://ei.docs.wso2.com/en/latest/streaming-integrator/overview/overview/) is a cloud-native, lightweight component that understands, captures, analyzes, processes, and acts upon streaming data and events in real-time. It utilizes the SQL-like query language ‘Siddhi’ to implement the solution.

As explained above, EI 7.1.0 addresses a wider audience that prefers different approaches to integration.

## Advantages of using the Micro Integrator in EI 7.0

Compared to the ESB profile of WSO2 Enterprise Integrator 6.x, the Micro Integrator encompasses the following key attributes that are essential for a microservice-ready integration solution.

-	Faster startup time (<5s).
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
			5s
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
			~150 MB
		</td>
	</tr>
	<tr>
		<td>
			Product configuration model
		</td>
		<td>
			XML-based configurations
		</td>
		<td>
			<a href="../../references/config-catalog">TOML-based configurations</a>
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
			Task Coordination 
		</td>
		<td>
			Hazelcast based
		</td>
		<td>
			RDBMS based 
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
			Hot deployment
		</td>
		<td>
			Available
		</td>
		<td>
			Available
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
			Database-based Registry
		</td>
		<td>
			-
		</td>
		<td>
			<a href="../../setup/deployment/file_based_registry">File system based Registry</a>
		</td>
	</tr>
</table>
