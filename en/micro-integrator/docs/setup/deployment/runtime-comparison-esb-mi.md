# Compare the EI 7.1 Micro Integrator with the ESB
The Micro Integrator of EI 7.1 is the latest most improved version of the WSO2 ESB. Therefore, you can easily migrate to EI 7.1 Micro Integrator from any previous ESB runtime.

Given below is a comparison of the Micro Integrator of EI 7.1 and the previous WSO2 ESB runtimes. This will help you understand the important changes in the Micro Integrator that will impact your migration.

## Feature comparison

The following table explains the availability of the most critical features in each runtime.

<table>
	<tr>
		<th></th>
		<th>WSO2 ESB Runtime</th>
		<th>Micro Integrator Runtime</th>
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
			<a href="../../../references/config-catalog">TOML-based configurations</a>
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
			<a href="../../../develop/WSO2-Integration-Studio">WSO2 Integration Studio</a>
		</td>
		<td>
			<a href="../../../develop/WSO2-Integration-Studio">WSO2 Integration Studio</a>
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
			<a href="../../../administer-and-observe/working-with-monitoring-dashboard">Micro Integrator Dashboard</a></br>
			<a href="../../../administer-and-observe/using-the-command-line-interface">Micro Integrator CLI</a>
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
			<a href="../../../setup/deployment/file_based_registry">File system based Registry</a>
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

## Features removed

The following features are removed from the Micro Integrator of EI 7.1 because they are not frequently used.

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
			<a href="../../../develop/WSO2-Integration-Studio">WSO2 Integration Studio</a> is the recommended tool for developing integration solutions. The monitoring capabilities available in the management console (of the ESB profile) are available through the new <a href="../../../administer-and-observe/working-with-monitoring-dashboard">Micro Integrator dashboard</a>.
		</td>
		<td>
			<a href="../../../administer-and-observe/working-with-monitoring-dashboard">Micro Integrator Dashboard</a>
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
			RDBMS-based Registry
		</td>
		<td>
			-
		</td>
		<td>
			<a href="../../../setup/deployment/file_based_registry">File system based Registry</a>
		</td>
	</tr>
</table>