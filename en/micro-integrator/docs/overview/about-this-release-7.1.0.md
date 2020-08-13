# About this Release

WSO2 Enterprise Integrator (EI) version 7.1.0 is the latest release of the WSO2 EI 7.x family, which provides a powerful solution for integrating systems.

## What's new in this release?

#### New in the Micro Integrator

The following features and improvements are available for the <b>Micro Integrator</b> of EI 7.1.0:

-   [Task coordination](../../setup/deployment/deploying_wso2_ei/#cluster-coordination) for distributed deployments
-   Capability of viewing and configuring logs without accessing the file system
-   User management with [external user stores](../../setup/install_and_setup_overview/#data-stores)
-   [Readiness probe](../../setup/deployment/health_check) for health checking
-   Enhanced experience for [handling security](../../setup/security/encrypting_plain_text) (Ciphertool, Securevault, Kubernetes Secrets)
-   [Hot deployment](../../develop/hot-deployment) capability
-   Schema-based XML to JSON transformations with the [JSON Transform mediator](../../references/mediators/json-Transform-Mediator)
-   [Swagger generation for data services](../../develop/advanced-development/using-swagger-for-apis)
-   [Transaction counter](../../setup/deployment/deployment_checklist/#monitoring-transaction-counts) for services
-   [Environment/System variable support](../../setup/dynamic_server_configurations) for Kubernetes environments
-   [Enhanced Micro Integrator CLI](../../administer-and-observe/using-the-command-line-interface)

#### New in WSO2 Integration Studio

The following features and improvements are available for <b>WSO2 Integration Studio</b> EI 7.1.0:

Improvements to [<b>Connectors</b>](../../references/connectors/connectors-overview):

-   Revamped connector story with improved user experience
-   Redesigned connector configuration view
-   New externalizable connection configuration view for connectors
<!--   Improved CI/CD story with WSO2 Micro Integrator-->

Improvements to [<b>integration project</b>](../../develop/create-integration-project):

-   Maven multi module support for the integration project
<!-- -   Embedded maven support -->
-   New maven profiles for project build  
-   Improved project import/export functionality
<!-- -   Capability to import existing projects into maven multi module project -->

Improvements to [<b>Docker project</b>](../../develop/create-docker-project):

-   Improved Docker project
-   Capability to push images to a remote repository via the Docker exporter

Improvements to the [<b>embedded Micro Integrator</b>](../../develop/using-embedded-micro-integrator):

-	WSO2 Micro Integrator 1.2.0 as the embedded server runtime
-	Hot deployment capability for the embedded Micro Integrator
-   Capability to configure the embedded Micro Integrator via the tool
-   Embedded WSO2 EI monitoring dashboard to list details of the deployed services

Improvements to <b>debugging and testing</b>:

-   Capability to [run and debug synapse configurations](../../develop/debugging-mediation)
<!-- -   Listing deployed services via the embedded CLI tool -->
-   Improved [Synapse Unit testing framework](../../develop/creating-unit-test-suite)
-   Test failure observability improvements
<!-- -   Synapse template testing support -->
<!-- -   Synapse sequence testing improvements -->

Improvements to <b>artifact development</b>:

-   Improved [data mapper mediator](../../references/mediators/data-Mapper-Mediator)
<!--  -   AI-based data mapper improvements -->
<!--  -   Data mapper functionality and UI improvements -->
-   Improved [data services editor](../../develop/creating-artifacts/data-services/creating-data-services)
<!--  -   Improved editor theme -->
<!-- -   RCP improvements -->

Other improvements:

-   New WSO2 Integration Studio theme
-   New 'Getting Started' experience
<!-- -   New update notification functionality -->

#### New in the Streaming Integrator

The following features and improvements are available for the <b>Streaming Integrator</b> of EI 7.1.0:

-   [Extension Installer](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/connectors/downloading-and-Installing-Siddhi-Extensions/#downloading-and-installing-siddhi-extensions-via-the-command-line) CLI tool to install Siddhi extensions via the command line.
-   Error Store in which the events with errors can be browsed. This also allows events with certain types of errors to be replayed. For more information, see [Error Handling](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/guides/fault-Handling/).
-   ETL wizard that allows you to design ETL applications without writing code. For more information, see [Creating an ETL Application via SI Tooling](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/examples/creating-etl-application-via-tooling/).
-   New monitoring dashboards to analyze ETL flows. For more information, see [Monitoring Statistics via Grafana Dashboards](https://ei.docs.wso2.com/en/7.1.0/streaming-integrator/admin/setting-up-grafana-dashboards/).
-   Enhanced support for CDC, File Streaming and Cloud storages. For more information, see [Siddhi Query Guide - File](https://siddhi-io.github.io/siddhi-io-file/) and [Siddhi Query Guide - CDC](https://siddhi-io.github.io/siddhi-io-cdc/).
-   Improvements to the Design View of Streaming Integrator Tooling.
-   Extensive support for CDC and File connectors with the ability to generate Cron expressions via the UI. 

<!--
## Fixed issues
-   [Streaming Integrator - Fixed Issues](https://github.com/wso2/streaming-integrator-tooling/issues)
-   [Streaming Integrator Tooling - Fixed Issues](https://github.com/wso2/streaming-integrator-tooling/issues)
-->

## Known issues

-   [Micro Integrator - Known Issues](https://github.com/wso2/micro-integrator/issues)
-   [WSO2 Integration Studio - Known Issues](https://github.com/wso2/devstudio-tooling-ei/issues)

-   [Streaming Integrator - Known Issues](https://github.com/wso2/streaming-integrator/issues)
-   [Streaming Integrator Tooling - Known Issues](https://github.com/wso2/streaming-integrator/issues?q=is%3Aissue+is%3Aclosed)
