# Install and Setup Overview

## Installation Guide

The installation guide of the Micro Integrator explains how to set up the Micro Integrator on a single server node or container.

<table>
	<tr>
		<td>
			<a href="../../setup/installation/install_prerequisites">Installation Prerequisites</a>
		</td>
		<td>
			See the system requirements for setting up and using the Micro Integrator of WSO2 EI 7.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/installation/install_in_vm_installer">Install via the Installer</a>
		</td>
		<td>
			Follow the instructions on downloading the Micro Integrator and installing the server on a VM by using the installer.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/installation/install_in_vm_binary">Install via the Binary</a>
		</td>
		<td>
			Follow the instructions on downloading the Micro Integrator and installing the server on a VM by using the binary distribution.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/installation/run_in_containers">Run the Micro Integrator on Containers</a>
		</td>
		<td>
			Use the Docker images of the Micro Integrator from <b>Docker Hub</b> and deploy an integration solution in <b>Docker</b> or <b>Kubernetes</b>.
		</td>
	</tr>
</table>

## Deployment Options

You can set up a Micro Integrator deployment either in a VM environment or a Kubernetes environment.

### Kubernetes Deployment

Follow the topics in this section if you are setting up a Micro Integrator deployment in a Kubernetes environment.

<table>
	<tr>
		<td>
			<a href="../../setup/deployment/kubernetes_deployment_patterns">Kubernetes Deployment Patterns</a>
		</td>
		<td>
			Understand the various deployment patterns that you can use. You can select the appropriate pattern for your requirement.
		</td>
	</tr>
	<!--
	<tr>
		<td>
			<a href="">Setting up a Kubernetes Cluster</a>
		</td>
		<td>
			Follow the step-by-step instructions on how to set up a Kubernetes cluster for deployment pattern you choose.
		</td>
	</tr>
	-->
	<tr>
		<td>
			Building a CICD Pipeline
		</td>
		<td>
			Follow the step-by-step instructions on how to build a pipeline to manage the continuous integration and continuous deployment process.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/dynamic_server_configurations">Managing Configurations across Environments</a>
		</td>
		<td>
			When you have multiple environments such as DEV, QA, UAT, and PROD, you can follow these instructions to remotely manage the configurations in each environment without modifying the artifacts or configuration file.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/deployment/deployment_checklist">Product Deployment Checklist</a>
		</td>
		<td>
			Once you have set up your development verify with this checklist to ensure that you have followed all the required standards.
		</td>
	</tr>
</table>

### VM Deployment

Follow the topics in this section if you are setting up a Micro Integrator deployment in a VM environment.

<table>
	<tr>
		<td>
			<a href="../../setup/deployment/deploying_wso2_ei">Clustered Deployment</a>
		</td>
		<td>
			Follow the instructions on how to set up a clustered deployment in a VM environment.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/deployment/deployment_synchronization">Deployment Synchronization</a>
		</td>
		<td>
			Follow the instructions on configuring deployment synchronization for the nodes in your cluster.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/deployment/setting_up_lb">Load Balancing</a>
		</td>
		<td>
			Follow the instructions on configuring an external load balancer to front the nodes in a cluster.
		</td>
	</tr>
	<tr>
		<td>
			<b>Databases</b>
		</td>
		<td>
			This section explains how to set up a database and connect it to the nodes in your cluster in your Micro Integrator. The following database types are supported: <a href="../../setup/deployment/databases/setting-up-MySQL">MySQL</a>, <a href="../../setup/deployment/databases/setting-up-MSSQL">MSSQL</a>, <a href="../../setup/deployment/databases/setting-up-PostgreSQL">Postgre</a>, <a href="../../setup/deployment/databases/setting-up-Oracle">Oracle</a>, <a href="../../setup/deployment/databases/setting-up-IBM-DB2">IBM</a>.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/deployment/troubleshooting_deployment">Troubleshooting a Production Deployment</a>
		</td>
		<td>
			This section explains how you can troubleshoot your cluster in a VM deployment.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/deployment/backup_recovery">Backup and Recovery</a>
		</td>
		<td>
			The section explains how to configure backup and recovery for your deployment.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/dynamic_server_configurations">Managing Configurations across Environments</a>
		</td>
		<td>
			When you have multiple environments such as DEV, QA, UAT, and PROD, you can follow these instructions to remotely manage the configurations in each environment without modifying the artifacts or configuration file.
		</td>
	</tr>
	<tr>
		<td>
			<a href="../../setup/deployment/deployment_checklist">Product Deployment Checklist</a>
		</td>
		<td>
			Once you have set up your development verify with this checklist to ensure that you have followed all the required standards.
		</td>
	</tr>
</table>

## Migration Guide

Follow the topics in this section to migrate to the latest version of WSO2 EI 7 from a previous version.

-	[Migrating from WSO2 EI 6.x Family](../../setup/deployment/migrating-from-ei-6.x.x)

## Security Hardening

Follow the topics in this section to configure security for your Micro Integrator instance.

<table>
	<tr>
		<td>
		   <a href="../../setup/security/creating_keystores">Creating New Keystores</a>
		</td>
		<td>
			Create new kestores to replace the default keystores in your Micro Integrator.
		</td>
	</tr>
	<tr>
		<td>
		   <a href="../../setup/security/renewing_ca_signed_certificate_in_keystore">Renew a CA-signed Certificate</a>
		</td>
		<td>
			Renew a CA certificate in the keystore of your Micro Integrator.
		</td>
	</tr>
	<tr>
		<td>
		   <a href="../../setup/security/configuring_keystores">Configuring Keystores</a>
		</td>
		<td>
			Update the keystore configurations by pointing to the new keystores.
		</td>
	</tr>
	<tr>
		<td>
			 <a href="../../setup/security/gdpr_ei">GDPR for the Micro Integrator</a>
		</td>
		<td>
			 Learn about GDPR compliance in the Micro Integrator.
		</td>
	</tr>
	<tr>
		<td>
			 <a href="../../setup/security/about_forgetme_tool">Identity Anonymization Tool</a>
		</td>
		<td>
			 Find out more about the ForgetMe tool, which is used for anonymizing personal data saved in the Micro Integrator.
		</td>
	</tr>
	<tr>
		<td>
			 <a href="../../setup/security/encrypting_plain_text">Encrypting Secrets</a>
		</td>
		<td>
			 Encrypt sensitive data in your configuration files and synapse configurations.
		</td>
	</tr>
	<tr>
		<td>
			 <a href="../../setup/security/single_key_encryption">Using Symmetric Encryption</a>
		</td>
		<td>
			 As an alternative to asymmetric encryption using keystores, use single key encryption.
		</td>
	</tr>
	<tr>
		<td>
			 <a href="../../setup/security/securing_management_api">Securing the Management API</a>
		</td>
		<td>
			 The management API of the Micro Integrator powers the Micro Integrator <b>CLI tool</b> as well as the <b>Dashboard</b>. This section explains how the management API can be secured.
		</td>
	</tr>
</table>

## Performance Tuning

Follow the topics in this section to optimize the performance of your Micro Integrator instance.

-	<a href='../../setup/performance_tuning/tuning_jvm_performance'>Tuning JVM Performance</a>
-	<a href='../../setup/performance_tuning/network_os_performance'>Tuning Network and OS Performance</a>
-	<a href='../../setup/performance_tuning/jdbc_tuning'>Tuning JDBC Performance</a>
-	<a href='../../setup/performance_tuning/http_transport_tuning'>Tuning the HTTP Transport</a>
-	<a href='../../setup/performance_tuning/jms_transport_tuning'>Tuning the JMS Transport</a>
-	<a href='../../setup/performance_tuning/tuning-the-VFS-Transport'>Tuning the VFS Transport</a>
-	<a href='../../setup/performance_tuning/rabbitmq_transport_tuning'>Tuning the RabbitMQ Transport</a>
-	<a href='../../setup/performance_tuning/tuning-inbound-endpoints'>Tuning the Inbound Endpoints</a>

## Configuring Brokers

The following topics explain how to connect the Micro Integrator with an external message broker.

-	<a href='../../setup/brokers/configure-with-ActiveMQ'>Connecting to ActiveMQ</a>
-	<a href='../../setup/brokers/configure-with-Apache-Artemis'>Connecting to Apache Artemis</a>
-	<a href='../../setup/brokers/configure-with-HornetQ'>Connecting to HornetQ</a>
-	<a href='../../setup/brokers/configure-with-IBM-websphere-app-server'>Connecting to IBM Websphere App Server</a>
-	<a href='../../setup/brokers/configure-with-IBM-websphereMQ'>Connecting to IBM Websphere MQ</a>
-	<a href='../../setup/brokers/configure-with-JBossMQ'>Connecting to JBoss MQ</a>
-	<a href='../../setup/brokers/configure-with-MSMQ'>Connecting to MSMQ</a>
-	<a href='../../setup/brokers/configure-with-rabbitMQ'>Connecting to RabbitMQ</a>
-	<a href='../../setup/brokers/configure-with-SwiftMQ'>Connecting to SwiftMQ</a>
-	<a href='../../setup/brokers/configure-with-Tibco-EMS'>Connecting to TIBCO EMS</a>
-	<a href='../../setup/brokers/configure-with-WebLogic'>Connecting to Weblogic</a>
-	<a href='../../setup/brokers/configure-with-WSO2-MB'>Connecting to WSO2 MB</a>
-	<a href='../../setup/brokers/configure-with-multiple-brokers'>Connecting to Multiple Brokers</a>

## Feature-Specific Configurations

The follow topics explain configurations specific to various scenarios and features of the Micro Integrator.

-	<a href='../../setup/feature_configs/configuring_timestamp_conversion_for_rdbms'>Configuring Time Stamp Conversion for RDBMS</a>
-	<a href='../../setup/message_builders_formatters/message-builders-and-formatters'>Configuring Message Builders and Formatters</a>
-	<a href='../../setup/message_builders_formatters/message-relay'>Configuring Message Relay</a>
-	<a href='../../setup/transport_configurations/configuring-transports'>Configuring Transports</a>
-	<a href='../../setup/transport_configurations/multi-https-transport'>Configuring Multi-HTTPS</a>
-	<a href='../../setup/feature_configs/configuring-kafka'>Configuring Kafka</a>

## Additional Configurations

The follow topics explain additional configurations of the Micro Integrator.

-	<a href='../../setup/adding_a_custom_proxy_path'>Adding a Custom Proxy Path</a>
-	<a href='../../setup/configuring_proxy_servers'>Configuring a Proxy Server</a>
-	<a href='../../setup/changing_default_ports'>Changing Default Ports</a>
-	<a href='../../setup/customizing_error_pages'>Customizing Error Messages</a>
-	<a href='../../setup/govern_ext_refs_across_env'>Governing External References Across Environments</a>
-	<a href='../../setup/enabling_SSL_tunneling_thru_proxy_server'>Enable SSL Tunneling through Proxy Server</a>
