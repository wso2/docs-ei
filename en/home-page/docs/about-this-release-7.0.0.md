WSO2 Enterprise Integrator (EI) 7.0.0 is the only open-source, API-centric, and cloud-native integration platform to connect APIs, data, streams, SaaS, and legacy apps using microservices or ESB style architecture.  

WSO2 Enterprise Integrator 7.0.0 consists of three integrators providing a comprehensive set of functionalities and features required to fulfill any enterprise-grade integration requirement. WSO2 Enterprise Integrator 7.0.0 also offers sophisticated tooling for each integrator providing a rich developer experience. Following are the key features of each runtime and tooling.

## Ballerina Integrator 1.0.0

WSO2 EI 7.0 is also the first solution to use Ballerina, a revolutionary new code-driven language for programming network-distributed applications. This is based on Ballerina 1.0.1, which consists of improvements to the language syntax and semantics based on the stable language specification version 2019R3

### What’s new in Ballerina Integrator?

-   Connectors for protocols like HTTP, File, FTP, Samba, gRPC, NATS and Kafka.
-   EI Connectors for well known SaaS applications like Salesforce, Amazon SQS, Amazon S3, Google Sheets and Gmail. You can pull them from Ballerina Central.
-   Improved support for EIP’s (Enterprise Integration Patterns) with tailor-made EI connectors and template support for tooling.
-   Java interoperability (allows you to call Java code from Ballerina)
-   Project Templates for widely used integration use cases. With this, we expect to save time spent on bootstrapping a new integration project. You can pull them from Ballerina Central.
-   VS Code based tooling support to discover and create projects using templates.
 
## Micro Integrator 1.1.0

This is the successor of Micro Integrator 1.0.0, which is the cloud-native version of the WSO2 Enterprise Integrator 6.5.0 integration profile containing all its key capabilities. The new Micro Integrator is also supported by the new WSO2 Integration Studio, which provides enhanced tooling capabilities for developing and testing integration solutions.
 
### What’s new in Micro Integrator?

-   Built-in unit testing framework for writing unit tests for synapse configuration artifacts
-   Support for JDK 11
-   All new Micro Integrator Dashboard to monitor the synapse runtime artifacts
-   Management API with improved functionalities such as JWT based authentication
-   Improved Micro Integrator CLI 
-   ODATA support for Data Services
-   JDBC User store support
-   Seamless integration with WSO2 Streaming Integrator through gRPC
-   Single file (TOML based) configuration approach that makes runtime configuration much simpler and intuitive    
-   System variable support for all environment-dependent parameters of synapse configurations
 
### What’s new in WSO2 Integration Studio 7.0.0?

-   Synapse Unit Testing Framework
-   Test Synapse integration scenarios
-   Create mock services
-   New Docker/Kubernetes Project type
-   Generate Docker images with custom configurations
-   Publish Docker images to any Docker repository
-   Support for k8s-ei-operator, which allows deploying integration into Kubernetes environments
-   WSO2 Micro Integrator 1.1.0 as the internal server runtime
Run and debug Synapse configurations
 
## Streaming Integrator 1.0.0

WSO2 Streaming integrator is powered by siddhi.io and inherits all its features and characteristics. It’s a successor of WSO2 Stream Processor 4.4.0 and includes all key features of WSO2 SP 4.4.0 except dashboards. 

### What’s new in Streaming Integrator?

-   Amazon S3 and Google cloud storage connector, introducing cloud data integration. 
-   Siddhi K8s operator support for Streaming integrator enabling easy deployment of SI in a Kubernetes cluster.
-   Support to export siddhi applications as Docker images or K8s artifacts via tooling.
-   GRPC connector for low latency RPCs.
-   Seamless integration with the micro integrator.
-   Enhanced file connector for efficient real-time ETL with large files.
-   JDK11 support.
 
### What’s new in Streaming Integrator Tooling 1.0.0?

-   On-demand query to run store queries against windows and databases.
-   An interactive tour which guides you through Streaming Integrator capabilities.
-   An operator finder that enables to quickly insert various extensions to the integration flow.
-   Deploys created Siddhi applications to a remote server.
-   Exports Siddhi applications and configurations as Docker and Kubernetes Artifacts.
-   JDK11 support.
 

## Fixed Issues

Ballerina Integrator fixed issues:

-   [Tasks/Bug Fixes and Improvements](https://github.com/wso2/ballerina-integrator/milestone/13?closed=1)

Micro Integrator fixed issues:

-   [Tasks/bug fixes and improvements](https://github.com/wso2/micro-integrator/issues?utf8=%E2%9C%93&q=is%3Aissue+closed%3A2019-09-11..2019-09-27+is%3Aclosed+)

WSO2 Integration Studio fixed issues:

-   [7.0.0-RC1](https://github.com/wso2/devstudio-tooling-ei/milestone/7?closed=1)

Streaming Integrator fixed issues:

-   [Tasks/Bug Fixes and Improvements](https://github.com/wso2/ballerina-integrator/milestone/12?closed=1)