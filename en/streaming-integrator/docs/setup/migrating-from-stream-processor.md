# Migrating from WSO2 Stream Processor

The Streaming Integrator performs all functions that are also performed by [WSO2 Stream Processor](https://docs.wso2.com/display/SP440/Stream+Processor+Documentation). It also has additional features to trigger integration flows in order to take action in response to results derived after analyzing data.

If you are currently using WSO2 Stream Processor to carry out any streaming integration/stream processing activities and want to carry them out in the Streaming Integrator, you can migrate your setup as follows:

## Preparing to upgrade

The following prerequisites should be completed before upgrading.

- Make a backup of the SP 4.4.0 database and copy the <SP_HOME> directory in order to backup the product configurations.
- Download the Streaming Integrator from the [Enterprise Integrator Home](https://wso2.com/integration/)

## Migrating Databases

To connect the Streaming Integrator to the same databases as WSO2 SP 4.4.0 so that the persisted data can be accessed, configure the data sources as follows:

- Configure the data sources in the `<SI_HOME>/conf/server/deployment.yaml` file the same way you have configured them in `<SP_HOME>/conf/wso2/worker/deployment.yaml` file.
- Configure the data sources in the `<SI__TOOLING_HOME>/conf/server/deployment.yaml` file the same way you have configured them in `<SP_HOME>/conf/wso2/editor/deployment.yaml` file.
- Check the data source configured for Business Rules  in the `<SP_HOME>/conf/wso2/dashboard/deployment.yaml` file, and configure that data source with the same parameter values in the `<SI__TOOLING_HOME>/conf/server/deployment.yaml` file.

    !!!info
        The Business Rules feature which was a part of the `Dashboard` profile of the Stream Processor is now shipped with Streaming Integrator Tooling. Therefore, configurations related to this feature are added in the `<SI__TOOLING_HOME>/conf/server/deployment.yaml` file.

For the complete list of data sources configured for the Streaming Integrator, see [Configuring Data sources](configuring-data-sources.md).

## Migrating Siddhi applications

To migrate the Siddhi applications that you have deployed in WSO2 SP 4.4.0, follow the procedure below:

1. Copy all the Siddhi applications in the `<SP_HOME/wso2/worker/deployment/siddhi-files` directory.

2. Place the Siddhi applications you copied in the `<SI_HOME/wso2/server/deployment/siddhi-files` directory.

## Migrating dashboards

WSO2 Streaming Integrator 1.1.0 provides you with preconfigured dashboards to be used to visualize ETL performance statistics. You can deploy these dashboards in Grafana. For more information, see [Monitoring Statistics via Grafana Dashboards](../admin/setting-up-grafana-dashboards.md).

If you are using the [Analytics Dashboard](https://docs.wso2.com/display/SP440/Visualizing+Data) with WSO2 Stream Processor and you want to continue using it, follow the procedure below.

1. Download the Analytics Dashboard from [WSO2 Analytics Dashboard Github Repository.](https://github.com/wso2/analytics-dashboard/releases).

2. Extract the file you downloaded to a location of your choice. From here onwards, the extracted location is referred to as `<AD_HOME>`.

3. Check the database configurations that you have added in the `<SP_HOME>/conf/dashboards/deployment.yaml` file for the `WSO2_DASHBOARD_DB` data source, and do a database dump.

4. Import the database dump according to the dashboard data source configuration that you have added in the `<AD_HOME>/conf/portal/deployment.yaml` file.

5. If you have created any custom widget in the WSO2 Stream Processor, copy the relevant subdirectory for that widget from the `<SP_HOME>/wso2/dashboard/deployment/web-ui-apps/portal/extensions/widgets` directory and place it in the `<AD_HOME>/wso2/portal/deployment/web-ui-apps/analytics-dashboard/extensions/widgets` directory.

6.To publish statistics from the WSO2 Streaming Integrator in the Analytics Dashboard, configure the two products to work together. For instructions, see [Monitoring the Streaming Integrator](../admin/monitoring-the-streaming-integrator.md).

## Testing the migration

Simulate a few events to the Siddhi applications deployed in the Streaming Integrator to test whether they are generating the expected results.