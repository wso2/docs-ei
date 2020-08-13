# About this Release

WSO2 Enterprise Integrator (EI) version 7.1.0 is the latest release of the WSO2 EI 7.x family, which provides a powerful solution for integrating systems.

## What's new in this release?

### New in the Micro Integrator

The following features and improvements are available for the <b>Micro Integrator</b> of EI 7.1.0:

-   [Task coordination](../../setup/deployment/deploying_wso2_ei/#cluster-coordination) for distributed deployments.
-   Capability of viewing and configuring logs without accessing the file system.
-   User management with [external user stores](../../setup/install_and_setup_overview/#data-stores).
-   [Readiness probe](../../setup/deployment/health_check) for health checking.
-   Enhanced experience for [handling security](../../setup/security/encrypting_plain_text) (Ciphertool, Securevault, Kubernetes Secrets).
-   [Hot deployment](../../develop/hot-deployment) capability.
-   Schema-based XML to JSON transformations with the [JSON Transform mediator](../../references/mediators/json-Transform-Mediator).
-   [Swagger generation for data services](../../develop/advanced-development/using-swagger-for-apis).
-   [Transaction counter](../../setup/deployment/deployment_checklist/#monitoring-transaction-counts) for services.
-   [Environment/System variable support](../../setup/dynamic_server_configurations) for Kubernetes environments.
-   [Enhanced Micro Integrator CLI](../../administer-and-observe/using-the-command-line-interface).

### New in WSO2 Integration Studio**

The following features and improvements are available for <b>WSO2 Integration Studio</b> EI 7.1.0:

Revamped [<b>Connectors</b>](../../references/connectors/connectors-overview) functionality with improved user experience:

-   Redesigned connector configuration view.
-   New externalizable connection configuration view for connectors.

Improved capabilities to support CI/CD with WSO2 Micro Integrator:

!!! Note
    For more information, read about the [<b>integration project</b>](../../develop/create-integration-project) and the [<b>Docker project</b>](../../develop/create-docker-project).

-   Capability to create the integration solution as a **Maven Multi Module** project.
-   Embedded maven support to easily build and push projects.
-   Maven profiles allowing you to build individual integration modules.
-   Capability to import/export maven project (integration project).
-   Capability to import/export projects with/without copying to the workspace.
-   Capability to import existing modules into maven multi module project (integration project).
-   Capability to build and push Docker exporter using maven.
-   Capability to push images to a remote repository via the Docker exporter.
-   Capability to encrypt secrets in Docker/Kubernetes modules of the integration project by using the **Cipher Tool**.

Improvements to the [<b>embedded Micro Integrator (MI)</b>](../../develop/using-embedded-micro-integrator):

-	WSO2 MI 1.2.0 as the embedded server runtime.
-	Hot deployment capability for the embedded MI.
-   Capability to configure the embedded Micro Integrator via the tool.
-   Capability to Run and Debug synapse configurations with MI 1.2.0.
-   Capability to view deployed services using the **Runtime Services** view.
-   Capability to view and monitor deployed artifacts using the **embedded** WSO2 EI Monitoring Dashboard.

Improvements to the [Synapse Unit testing framework](../../develop/creating-unit-test-suite):

-   Capability to run detailed analysis of unit test failures.
-   Capability to test synapse template.
-   Capability to use HTTP status code and HTTP version assertion.
-   Capability use JSON and text payload assertion for sequence testing.

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

## Known issues

-   [Micro Integrator - Known Issues](https://github.com/wso2/micro-integrator/issues)
-   [WSO2 Integration Studio - Known Issues](https://github.com/wso2/devstudio-tooling-ei/issues)

-   [Streaming Integrator - Known Issues](https://github.com/wso2/streaming-integrator/issues)
-   [Streaming Integrator Tooling - Known Issues](https://github.com/wso2/streaming-integrator/issues?q=is%3Aissue+is%3Aclosed)
