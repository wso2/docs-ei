# Governing External References Across Environments

Some artifacts must change based on the environment where the
application is deployed. For example, when you deploy a WSO2 Enterprise
Integrator(WSO2 EI) application to Dev, QA, and Production environments,
the service endpoints are different in each of those environments, so
you must update the proxy services accordingly with the relevant
endpoint values.

This section provides information on how to manage and deploy artifacts
across multiple environments. It focuses specifically on the management
of endpoints in multiple environments. The endpoints are the environment
dependent artifacts and are used as external references from within the
proxy service (environment independent artifact) configuration. By doing
this, the proxy service configuration does not need to be edited each
time it is deployed in a different environment.

## Understanding the Users

Users interacting with artifacts in each environment often have
different roles and have access to different resources and tools. For
example:

-   **Developer** : uses WSO2 Enterprise Integrator(WSO2 EI) Tooling to
    create services and Composite Applications (Capps) and push project
    artifacts to a source code repository, such as GitHub. Typically,
    the developer has no access to QA or Production resources.
-   **DevOps or Operations team member** : uses scripts and the WSO2
    Management Console to pull the applications created by the
    developers from the source code repository and deploy them to the QA
    and Production environments. These users need to update the
    endpoints before they deploy in the different environments.
    Typically, they do not have WSO2 EI Tooling.

## Best Practices for Migration

The following are the best practices that allow you to easily migrate
applications across environments:

-   We recommend you create one Composite Application (CApp) for each
    deployment environment, namely HelloWorldDevResources and
    HelloWorldQAResources. This allows you to deploy and manage them
    separately. Additionally, you need to define a Composite Application
    for the application itself, i.e., the proxy service.

-   Whenever you create a proxy service use the endpoint as a reference
    name rather than defining it inline within the proxy service. This
    approach ensures that the proxy service can be deployed from one
    environment to another without having to do any environment specific
    configuration changes.

-   Ensure to use the same name for the endpoint across all
    environments.
-   Ensure the endpoint values are present and accurate in all
    environments prior to deploying an application using those
    endpoints. You can either manually edit the endpoint values prior to
    deploying the application, or make this an automatic part of your
    deployment process.

### Maven Users

Maven can be used to build and deploy your artifacts across
environments. When using Maven, you are also able to define the
endpoints as variables and pass the URL value at the time of building
the project.

Details on how you can assign endpoint values at the time of building is
available at
<http://susinda.blogspot.ae/2017/01/wso2-esb-how-to-assign-endpoints-at.html>
.

> In WSO2 EI Tooling, a Maven Multi Module(MMM) project is used to contain
all the project information. For more information on Maven Multi Module
projects, see
<http://www.sonatype.com/books/mvnex-book/reference/multimodule.html> .

