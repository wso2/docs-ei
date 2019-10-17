# Migrating from WSO2 EI 6.x to WSO2 EI 7.0

This guide provides an overview of the recommended migration strategy for migrating from WSO2 EI 6.x to WSO2 EI 7.0. Note that these guidelines are only applicable when you are migrating the ESB profile of EI 6.x to either the Micro Integrator or the Ballerina Integrator in EI 7.0.

## Before you begin

See the following topics to understand the benefits of moving to EI 7.0 from EI 6.x:

-   [Comparison: EI 6.x vs EI 7.0](/references/comparisong-mi7-ei6xx/#comparison-wso2-ei-6xx-vs-wso2-ei-700)
-   [Advantages of using the Micro Integrator in EI 7.0](/references/comparisong-mi7-ei6xx/#advantages-of-using-the-micro-integrator-in-ei-70)
-   [Comparison: ESB profile of EI 6.x vs Micro Integrator of EI 7.0](/references/comparisong-mi7-ei6xx/#comparison-esb-profile-of-ei-6x-vs-micro-integrator-of-ei-70)
-   [Features removed from the Micro Integrator of EI 7.0](/references/comparisong-mi7-ei6xx/#features-removed-from-the-micro-integrator-of-ei-70)

## When should you migrate to EI 7.0?

If you are an existing EI 6.x user, migration is recommended for the following requirements:
 
-   You need to switch to a microservices architecture from the conventional centralized architecture.
-   You prefer the code-driven integration approach.
-   You need a more lightweight, container-friendly runtime.
-   You need native support for Kubernetes.
   
The decision on migration to the new platform needs to be taken by considering several factors including the preferred architectural style (centralized vs microservices), deployment environment in your organization, and the effort it takes to migrate existing integration configurations.

## Migrating to the Micro Integrator 
 
Both the ESB profile of EI 6.x and the Micro Integrator of EI 7.0 uses the same ESB runtime and the same developer tool ([WSO2 Integration Studio](../../develop/WSO2-Integration-Studio)) for developing integrations. Most of the mediation(ESB) and data integration features available in the ESB profile of EI 6.x are available in the Micro Integrator as well. Some of the features are [removed from WSO2 Micro Integrator](/references/comparisong-mi7-ei6xx/#features-not-available-in-wso2-micro-integrator) as they are not needed for MSA-based deployments or they are not frequently used.

In summary, all the integration capabilities that you used in the ESB can be used in the Micro Integrator with minimal changes. However, EI 7.0 introduces a [Toml-based configuration strategy](../../../references/config-catalog) to replace XML configurations, which simplifies your product configurations.
 
See the [detailed comparison of EI 6.5 and EI 7.0](../../references/comparisong-mi7-ei6xx/) to understand what has changed between the ESB profile of EI 6.5 and the Micro Integrator of EI 7.0.

## Migrating to the Ballerina Integrator
 
Integration development in EI 6.x is based on the configuration-driven/graphical tooling experience provided through [WSO2 Integration Studio](../../develop/WSO2-Integration-Studio). Integration development in the EI 7.0 Ballerina Integrator is based on a code-driven approach. 
 
Even though there are no tools to convert your integration configurations (developed using [WSO2 Integration Studio](../../develop/WSO2-Integration-Studio)) to Ballerina code, the intuitiveness and built-in integration concepts of the Ballerina language helps to build integration solutions very quickly. 
 
All the integration capabilities that you used in the ESB can be used in the Ballerina Integrator through the code-driven approach. See the documentation on Ballerina Integrator's [templates and tutorials](https://ei.docs.wso2.com/en/latest/ballerina-integrator/learn/use-cases/) to help you with this process.
