# About this Release

WSO2 Enterprise Integrator (EI) version 7.1.0 is the latest release of the WSO2 EI, which provides a powerful solution for integrating systems.

!!! Tip
	If you are already using a previous version of WSO2 EI and you want to migrate to this release, see the instructions in the [migration guide](../../setup/install_and_setup_overview/#migrate).

## What's new in this release?

WSO2 Enterprise Integrator is continuously improving in every new release. Listed below are the new features and improvements that are introduced with the EI 7.1.0 release.

### New in the Micro Integrator

The following features and improvements are available for the <b>Micro Integrator</b> runtime of EI 7.1.0:

-   [Task coordination](../../setup/deployment/deploying_wso2_ei/#cluster-coordination) for distributed deployments.
-   Capability of viewing and configuring logs (without accessing the file system) via the Micro Integrator's [CLI](../../administer-and-observe/using-the-command-line-interface) or the [management API](../../administer-and-observe/working-with-management-api).
-   User management with [external user stores](../../setup/install_and_setup_overview/#data-stores).
-   [Readiness probe](../../setup/deployment/health_check) for health checking.
-   Enhanced experience for [handling security](../../setup/security/encrypting_plain_text) (Ciphertool, Securevault, Kubernetes Secrets).
-   [Hot deployment](../../develop/hot-deployment) capability.
-   Schema-based XML to JSON transformations with the [JSON Transform mediator](../../references/mediators/json-Transform-Mediator).
-   [Swagger generation for data services](../../develop/advanced-development/using-swagger-for-apis).
-   [Transaction counter](../../setup/deployment/deployment_checklist/#monitoring-transaction-counts) for services.
-   [Environment/System variable support](../../setup/dynamic_server_configurations) for Kubernetes environments.
-   [Enhanced Micro Integrator CLI](../../administer-and-observe/using-the-command-line-interface).

### New in WSO2 Integration Studio

The following features and improvements are available for <b>WSO2 Integration Studio</b> EI 7.1.0:

Revamped [<b>Connectors</b>](../../references/connectors/connectors-overview) functionality with improved user experience:

-   Redesigned connector configuration view.
-   New externalizable connection configuration view for connectors.

Improved capabilities to support CI/CD with WSO2 Micro Integrator:

!!! Note
    For more information, read about the [<b>integration project</b>](../../develop/create-integration-project) and the [<b>Docker project</b>](../../develop/create-docker-project).

-   Create the integration solution as a **Maven Multi Module** project.
-   Embedded maven support to easily build and push projects.
-   Maven profiles allowing you to build individual integration modules.
-   Import/export maven project (integration project).
-   Import/export projects with/without copying to the workspace.
-   Import existing modules into maven multi module project (integration project).
-   Build and push Docker exporter using maven.
-   Push images to a remote repository via the Docker exporter.
-   Encrypt secrets in Docker/Kubernetes modules of the integration project by using the **Cipher Tool**.

Improvements to the [<b>embedded Micro Integrator (MI)</b>](../../develop/using-embedded-micro-integrator):

-	WSO2 MI 1.2.0 as the embedded server runtime.
-	Hot deployment capability for the embedded MI.
-   Configure the embedded Micro Integrator via WSO2 Integration Studio.
-   Run and Debug synapse configurations with MI 1.2.0.
-   View deployed services using the **Runtime Services** view.
-   View and monitor deployed artifacts using the **embedded** WSO2 EI Monitoring Dashboard.

Improvements to the [Synapse Unit testing framework](../../develop/creating-unit-test-suite):

-   Run detailed analysis of unit test failures.
-   Test synapse templates.
-   Use HTTP status code and HTTP version assertion.
-   Use JSON and text payload assertion for sequence testing.

Improvements to <b>artifact development</b>:

-   Improved [data mapper mediator](../../references/mediators/data-Mapper-Mediator)
-   Improved [data services editor](../../develop/creating-artifacts/data-services/creating-data-services)

WSO2 Integration Studio customizations (RPC):

-   New WSO2 Integration Studio theme.
-   New 'Getting Started' experience.
-   New update notification functionality.

### New in the Streaming Integrator

The following features and improvements are available for the <b>Streaming Integrator</b> of EI 7.1.0:

-   [Extension Installer](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/connectors/downloading-and-Installing-Siddhi-Extensions/#downloading-and-installing-siddhi-extensions-via-the-command-line) CLI tool to install Siddhi extensions via the command line.
-   Error Store in which the events with errors can be browsed. This also allows events with certain types of errors to be replayed. For more information, see [Error Handling](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/guides/fault-Handling/).
-   ETL wizard that allows you to design ETL applications without writing code. For more information, see [Creating an ETL Application via SI Tooling](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/examples/creating-etl-application-via-tooling/).
-   New monitoring dashboards to analyze ETL flows. For more information, see [Monitoring Statistics via Grafana Dashboards](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/admin/setting-up-grafana-dashboards/).
-   Enhanced support for CDC, File Streaming and Cloud storages.
-   Improvements to the Design View of Streaming Integrator Tooling.
-   Extensive support for CDC and File connectors with the ability to generate Cron expressions via the UI. For more information, see [Siddhi Query Guide - File](https://siddhi-io.github.io/siddhi-io-file/) and [Siddhi Query Guide - CDC](https://siddhi-io.github.io/siddhi-io-cdc/).

<!--
## Fixed issues
-   [Streaming Integrator - Fixed Issues](https://github.com/wso2/streaming-integrator-tooling/issues)
-   [Streaming Integrator Tooling - Fixed Issues](https://github.com/wso2/streaming-integrator-tooling/issues)
-->

## Compare this release with previous ESBs

Given below is a comparison of the Micro Integrator of EI 7.1 and the previous WSO2 ESB runtimes. Note that the Micro Integrator of EI 7.1 is the latest, most improved version of the WSO2 ESB runtime.

If you are migrating from a previous ESB, this comparison will help you understand the important changes in the Micro Integrator that will impact your migration.

!!! Note
	The following comparison is applicable to the following versions of the WSO2 ESB runtime:
	
	-	[WSO2 ESB 5.0](https://docs.wso2.com/display/ESB500/WSO2+Enterprise+Service+Bus+Documentation)
	-	[ESB profile of WSO2 EI 6.x.x series](https://docs.wso2.com/display/EI660/WSO2+Enterprise+Integrator+Documentation)

### Feature comparison

The following table explains the availability of the most critical features in the ESB runtime and Micro Integrator runtime of EI 7.1.0. In addition to the following list, the Micro Integrator runtime also contains [new and improved features](#whats-new-in-this-release?) listed above.

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

###	Features removed

The following features, which are available in ESB runtimes, are removed from the Micro Integrator of EI 7.1 because they are not frequently used.

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

## Known issues

-   [Micro Integrator - Known Issues](https://github.com/wso2/micro-integrator/issues)
-   [WSO2 Integration Studio - Known Issues](https://github.com/wso2/devstudio-tooling-ei/issues)

-   [Streaming Integrator - Known Issues](https://github.com/wso2/streaming-integrator/issues)
-   [Streaming Integrator Tooling - Known Issues](https://github.com/wso2/streaming-integrator/issues?q=is%3Aissue+label%3Atooling+is%3Aopen)
