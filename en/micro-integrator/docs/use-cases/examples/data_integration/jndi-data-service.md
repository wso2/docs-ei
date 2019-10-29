# Exposing a JNDI Datasource

Java Naming and Directory Interface (JNDI) is a Java API (application
programming interface) providing naming and directory functionality for
Java software clients to discover and look up data and objects via a
name. WSO2 Micro Integrator (WSO2 MI) supports the JNDI
InitialContext implementation by inheriting the JNDI implementation of
Tomcat (
<http://tomcat.apache.org/tomcat-7.0-doc/jndi-resources-howto.html> ).
JNDI helps decouple object creation from the object look-up . When you
have registered a datasource with JNDI, others can discover it through a
JNDI lookup and use it.

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

```xml
<data name="jndi_dataservice" serviceNamespace="" serviceGroup="">
    <description/>
    <config id="jndi">
        <property name="jndi_context_class">com.sun.jndi.rmi.registry.RegistryContextFactory</property>
        <property name="jndi_provider_url">rmi://localhost:2199</property>
        <property name="jndi_resource_name">DataServiceSample</property>
        <property name="jndi_password">password</property>
    </config>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project)
3. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.
4. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator.