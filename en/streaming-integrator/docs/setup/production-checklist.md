# Production Checklist

Once you download and install WSO2 Streaming Integrator, you may need to update its default configurations based on your requirements.

The changes that you need to make include the following:

## Providing access

Multiple users with various roles in your organization can require access to your WSO2 Streaming Integrator installation to carry out different activities. In order to manage the level of access provided to each user based on their roles, you are required to configure users roles and permissions. For instructions, see [User Management](../admin/user-management.md).

## Securing the Streaming Integrator

WSO2 SI is an open-source product. Therefore, anyone who downloads it has access to the default users and passwords, default keystore settings, etc. Therefore, you are required to update the configurations related to security in order to ensure that your data is secure when you run WSO2 SI in a production environment. For more information, see the following topics:

- WSO2 uses key stores to store cryptographic keys and certificates that are used for various purposes. For more information on how to configure and manage them, see [Working witb Keystores](../admin/working-with-Keystores.md).
- To protect sensitive data, see [Protecting Sensitive Data via the Secure Vault](https://ei.docs.wso2.com/en/latest/streaming-integrator/admin/protecting-sensitive-data-via-the-secure-vault/).
- To understand how WSO2 Streaming Integrator complies with GDPR(General Data Protection Regulations) and how you can comply with the same when you are using WSO2 Streaming Integrator, see [General Data Protection Regulations](../admin/general-data-protection-regulations.md).

## Opening the required ports

This involves configuring the network firewall for opening the ports used by WSO2 Streaming Integrator. For more information about the required ports, see [Configuring Default Ports](../ref/configuring-default-ports.md).

## Setting up databases

If you are integrating data stores in your Streaming Integration flows, you need to set up databases. For information about supported database types and how to configure datasources for them, see [Configuring Datasources](setup/configuring-data-sources).

## Configuring Transports

In order to use certain transports to receive and send data, you are required to configure them with WSO2 Streaming Integrator. For more information, see [Supporting Different Transports](../admin/supporting-different-transports.md).

## Minimizing the impact of system failure

In order to minimize the loss of data that can result from a system failure, you can configure WSO2 Streaming Integrator to periodically save its state in a database. For more information, see [Configuring Database and File System State Persistence](admin/configuring-Database-and-File-System-State-Persistence).

## Creating Business Rules templates

If you want to allow business users with limited coding knowledge to write business rules, you can template Siddhi applications and queries. For more information, see [Working with Business Rules](../admin/creating-business-rules-templates.md).

## Monitoring the Streaming Integrator

To monitor the performance of your Streaming Integrator setup, configure the Status Dashboard as described in [Monitoring the Streaming Integrator](../admin/monitoring-the-streaming-integrator.md).




 