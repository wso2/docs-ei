# Migrating from WSO2 EI 6.x to WSO2 EI 7.0

This guide provides an overview of the recommended migration strategy for migrating from WSO2 EI 6.x to WSO2 EI 7.0. Note that these guidelines are only applicable when you are migrating the ESB profile of EI 6.x to either the Micro Integrator or the Ballerina Integrator in EI 7.0.

## Before you begin

See the following topics to understand the benefits of moving to EI 7.0 from EI 6.x:

-   [Comparison: EI 6.x vs EI 7.0](../../../references/comparisong-mi7-ei6xx/#comparison-wso2-ei-6xx-vs-wso2-ei-700)
-   [Advantages of using the Micro Integrator in EI 7.0](../../../references/comparisong-mi7-ei6xx/#advantages-of-using-the-micro-integrator-in-ei-70)
-   [Comparison: ESB profile of EI 6.x vs Micro Integrator of EI 7.0](../../../references/comparisong-mi7-ei6xx/#comparison-esb-profile-of-ei-6x-vs-micro-integrator-of-ei-70)
-   [Features removed from the Micro Integrator of EI 7.0](../../../references/comparisong-mi7-ei6xx/#features-removed-from-the-micro-integrator-of-ei-70)

Note that EI 7 is a **WUM-only release**, which means that manual patches are not allowed. You can use [WSO2 Update Manager(WUM)](https://docs.wso2.com/display/updates/WSO2+Updates) to get the latest fixes or updates for this release.

## Why migrate to EI 7.0?

If you are an EI 6.x user, migration is recommended for the following requirements:
 
-   You need to switch to a microservices architecture from the conventional centralized architecture.
-   You prefer the code-driven integration approach.
-   You need a more lightweight, container-friendly runtime.
-   You need native support for Kubernetes.
   
The decision on migration to the new platform needs to be taken by considering several factors including the preferred architectural style (centralized vs microservices), deployment environment in your organization, and the effort it takes to migrate existing integration configurations.

## Migrating to the Micro Integrator 
 
Both the ESB profile of EI 6.x and the Micro Integrator of EI 7.0 uses the same ESB runtime and the same developer tool ([WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio)) for developing integrations. Most of the mediation(ESB) and data integration features available in the ESB profile of EI 6.x are available in the Micro Integrator as well. Some of the features are [removed from WSO2 Micro Integrator](../../../references/comparisong-mi7-ei6xx/#features-removed-from-the-micro-integrator-of-ei-70) as they are not needed for microservice deployments or they are not frequently used.

In summary, all the integration capabilities that you used in the ESB can be used in the Micro Integrator with minimal changes. However, EI 7.0 introduces a [Toml-based configuration strategy](../../../references/config-catalog) to replace XML configurations, which simplifies your product configurations.
 
See the [detailed comparison of EI 6.5 and EI 7.0](../../../references/comparisong-mi7-ei6xx) to understand what has changed between the ESB profile of EI 6.5 and the Micro Integrator of EI 7.0.

Follow the instructions below to start the migration!

### Set up the migration

-	Make a backup of the existing database used by the current EI 6.5.0 deployment. This backup is necessary in case the migration causes any issues in the existing database.
-	Download and install EI 7.0 in your environment. The home directory of your installation will be referred to as `<MI_HOME>` from hereon.
	-	[On a VM](../../../setup/installation/install_in_vm)
	-	[On Docker](../../../setup/installation/run_in_docker)
	-	[On Kubernetes](../../../setup/installation/run_in_kubernetes)
-	You can [use WSO2 Update Manager](https://docs.wso2.com/display/updates/) to get the latest available updates.

	!!! Info
		Note that you need a valid [WSO2 subscription](https://wso2.com/subscription) to use updates in a production environment.

### Migrating databases
If you are already using a JDBC user store with EI 6.5.0, you can connect the same database to the Micro Integrator of EI 7 by simply updating the user store configurations in the Micro Integrator.

!!! Note
	You cannot manage users and roles when you use a JDBC user store with the Micro Integrator. Therefore, be sure that your database is already up-to-date before connecting it to the Micro Integrator. Alternatively, you can shift to an LDAP user store. Read more about [user stores in the Micro Integrator](../../../setup/user_stores/setting_up_ro_ldap).

### Migrating the registry

The Micro Integrator uses a [file based registry](../file_based_registry) instead of a database (which is used in EI 6.5.0). Note the following when migrating the registry:

-	If the artifacts in EI 6.5.0 are added in carbon applications developed using WSO2 Integration Studio, you can directly migrate the artifacts to the Micro Integrator of EI 7.
-	If the artifacts are added through the management console in EI 6.5.0, first download the artifacts from the management console, and then add to the `<MI_HOME>/registry/` folder by maintaining the same resource structure.

### Migrating artifacts
Copy the contents inside `<EI_6.5.0_HOME>/repository/deployment` to the `<MI_HOME>/repository/deployment` folder.

### Migrating custom components
Copy custom OSGI components in the `<EI_6.5.0_HOME>/dropins` folder to the `<MI_HOME>/dropins` folder. If you have custom JARs in the `<OLD_EI_HOME>/lib` directory, copy those components to the `<MI_HOME>/lib` directory.

### Migrating keystores
Copy the JKS files in the `<EI_6.5.0_HOME>/repository/resources/security` directory to the `<MI_HOME>/repository/resources/security` directory.

### Migrating configurations
WSO2 EI 6.x.x versions supported multiple configuration files such as carbon.xml, synapse.properties, and axis2.xml. With the new configuration model in the Micro Integrator of EI 7, product configurations are primarily handled by a single configuration file named `deployment.toml` (stored in the `<MI_HOME>/conf` directory). The log4j2 configurations are handled in the `log4j2.properties`.

**Migrating to TOML configurations**

See the [product configuration catalog](../../../references/config-catalog) for the complete list of configurations that are available for the Micro Integrator.

!!! Tip
	If you have a [WSO2 subscription](https://wso2.com/subscription), it is highly recommended to reach WSO2 Support before attempting to proceed with the configuration migration.

**Migrating Log4j configurations**

From EI 7 onwards, the `carbon.logging.jar` file is not packed and the `pax-logging-api` is used instead. With this upgrade, the log4j version is also updated to log4j2.

-	If custom log appenders are used in your EI 6.5.0 deployment, add that as an appender in the `<MI_HOME>/conf/log4j2.properties` file. For example:

	```xml
	appender.SQ_PROXY_APPENDER.type = RollingFile
	appender.SQ_PROXY_APPENDER.name = SQ_PROXY_APPENDER
	appender.SQ_PROXY_APPENDER.fileName = ${sys:carbon.home}/repository/logs/stock-quote-proxy-service.log
	appender.SQ_PROXY_APPENDER.filePattern = ${sys:carbon.home}/repository/logs/stock-quote-proxy-service-%d{MM-dd-yyyy}.log
	appender.SQ_PROXY_APPENDER.layout.type = PatternLayout
	appender.SQ_PROXY_APPENDER.layout.pattern = TID: [%d] %5p {%c} [%logger] - %m%ex%n
	appender.SQ_PROXY_APPENDER.policies.type = Policies
	appender.SQ_PROXY_APPENDER.policies.time.type = TimeBasedTriggeringPolicy
	appender.SQ_PROXY_APPENDER.policies.time.interval = 1
	appender.SQ_PROXY_APPENDER.policies.time.modulate = true
	appender.SQ_PROXY_APPENDER.policies.size.type = SizeBasedTriggeringPolicy
	appender.SQ_PROXY_APPENDER.policies.size.size=10MB
	appender.SQ_PROXY_APPENDER.strategy.type = DefaultRolloverStrategy
	appender.SQ_PROXY_APPENDER.strategy.max = 20
	appender.SQ_PROXY_APPENDER.filter.threshold.type = ThresholdFilter
	appender.SQ_PROXY_APPENDER.filter.threshold.level = DEBUG
	```

	Then, add the appender to the appenders list. For example:

	```xml
	appenders = CARBON_CONSOLE, CARBON_LOGFILE, AUDIT_LOGFILE, SQ_PROXY_APPENDER, … 
	```

-	If the log level is changed/added for a component in your EI 6.5.0 deployment, add a logger in the `<MI_HOME>/conf/log4j2.properties` file as shown below.

	```xml
	logger.<Logger_Name>.name = <Component_name> 
	logger.<Logger_Name>.type = INFO
	```

	Then, add the logger name to the comma-separated list of logger. For example:

	```xml
	logger.synapse-transport-http-wire.name=org.apache.synapse.transport.http.wire
	logger.synapse-transport-http-wire.level=DEBUG
	logger.synapse-transport-http-wire.appenderRef.CARBON_LOGFILE.ref = CARBON_LOGFILE
	loggers = AUDIT_LOG, API_LOG, synapse-transport-http-wire, 
	```

## Migrating to the Ballerina Integrator
 
Integration development in EI 6.x is based on the configuration-driven/graphical tooling experience provided through [WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio). Integration development in the EI 7.0 Ballerina Integrator is based on a code-driven approach. 
 
Even though there are no tools to convert your integration configurations (developed using [WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio)) to Ballerina code, the intuitiveness and built-in integration concepts of the Ballerina language helps to build integration solutions very quickly. 
 
All the integration capabilities that you used in the ESB can be used in the Ballerina Integrator through the code-driven approach. See the documentation on Ballerina Integrator's [templates and tutorials](https://ei.docs.wso2.com/en/latest/ballerina-integrator/learn/use-cases/) to help you with this process.
