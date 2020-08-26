# Connector Developer Guidelines

WSO2 Enterprise Integrator (EI) Connectors are extensions to WSO2 EI runtime (compatible with both 6.x and 7.x) that enable developers to interact with SaaS applications on the cloud, databases, and popular B2B protocols.

Connectors are hosted in a connector store and can be added to integration flows in WSO2 Integration Studio, which is the tooling component of WSO2 EI. Once added, the operations of the connector can be dragged onto your canvas and added to your sequences and proxy services.

Each connector provides a set of operations, which you can call from your proxy services, sequences, and APIs to interact with the specific third-party service.

This document is an in-depth guide for developers to follow when developing a new connector from scratch. It aims to cover the initial steps to be followed, best practices, and details the means of implementing the UI schema for Integration Studio support.

## Connector Architecture

A connector is a collection or a set of operations that can be used in WSO2 Enterprise Integrator integration flow to access a specific service or a functionality. These operations are invoked from proxy services, sequences and APIs to interact.

* A connector operation is made using [sequence templates](../synapse-properties/template-properties/) in WSO2 EI. 
* The integration logic inside a connector operation is constructed using WSO2 EI mediators. 
* The integration logic inside a connector operation needs some custom functionality not provided by the WSO2 EI mediators, a java implementation can be attached to the associated sequence template. This is using the Custom Class Mediator approach. 
* If the third party service provider provides a Java SDK to interact with the service, connector operation can use them extending the java implementation. 


<img src="../../../assets/img/connectors/dev-connectors.png" title="Developing Connectors" width="800" alt="Developing Connectors"/>

### Connector Types

There are two types of connectors.

* Application / SaaS connectors - which connect to cloud applications. These are implemented purely using WSO2 EI mediators and constructs. E.g., Amazon S3, Salesforce
* Technology connectors - which implement different B2B protocols. Logic for these are implemented using mainly Java. E.g., JMS, NATS, Email

### Connector Structure

The typical folder structure of a connector is as follows.

```
├── pom.xml
├── repository
├── src
│   ├── main
│   │   ├── assembly
│   │   │   ├── assemble-connector.xml
│   │   │   └── filter.properties
│   │   ├── java
│   │   │   └── org
│   │   │       └── wso2
│   │   │           └── carbon
│   │   │               └── connector
│   │   │                   └── sampleConnector.java
│   │   └── resources
│   │       ├── config
│   │       │   ├── component.xml
│   │       │   └── init.xml
│   │       ├── connector.xml
│   │       └── sample
│   │           ├── component.xml
│   │           └── operation1.xml
│   └── test
```

* **pom.xml** - Defines the build information for maven.
* **repository** - When running Integration tests, the EI pack should be placed here.
* **src/main/assembly** - Instructions on packaging the connector.
* **src/main/java/org/wso2/carbon/connector** - Java code which is being used to implement connector logic
* **src/main/resources** - contains sequence templates for each connector operation.
* **src/main/resources/config** - contains the connector initialization logic.
* **src/main/resources/connector.xml** - Contains the connector information.
* **src/test** - Contains the test cases

### About the connector.xml file

All the operations exposed by the connector should be registered in this file. The syntax is as follows.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<connector>
   <component name="sample" package="org.wso2.carbon.connector">
      <dependency component="config" />
      <dependency component="sample" />
      <description>WSO2 sample connector library</description>
   </component>
   <icon>icon/icon-small.gif</icon>
</connector>
```

<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>name</td>
        <td>The ‘name’ attribute of the ‘component’ element in the connector.xml file defines the name of the connector. When operations are being invoked, this is the name appended to the operation.</td>
    </tr>
    <tr>
        <td>Dependancy</td>
        <td>Defines the sub directories which contain the operations. E.g., According to the sample above, it contains two subdirectories named ‘config’ and ‘sample’ inside /resources.
            └── resources
                  ├── config
                  │   ├── component.xml
                  │   └── init.xml
                  ├── connector.xml
                  └── sample
                         ├── component.xml
                         └── operation1.xml
        </td>
    </tr>
    <tr>
        <td>icon</td>
        <td>Path to the icon file of the connector.</td>
    </tr>
</table>    

### Subdirectory containing operations

Resources folder is used to group the operations in the connector in a more organised manner. 

It may contain subdirectories which contain operations. Each of those subdirectories should contain a component.xml file as below defining each template which represents an operation. Ultimately, all component.xml files in sub-directories should be referred to by the main component.xml file of the connector.

Below is the component.xml in ‘sample’ subdirectory.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<component name="sample" type="synapse/template">
   <subComponents>
      <component name="operation1">
         <file>operation1.xml</file>
         <description>sample wso2 connector method</description>
      </component>
   </subComponents>
</component>
```

<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>name</td>
        <td>The name of the subdirectory. This is the name to be used as the ‘component’ attribute of the ‘dependency’ element in the connector.xml file. For example:
            └── resources
                  ├── config
                  │   ├── component.xml
                  │   └── init.xml
                  ├── connector.xml
                  └── sample
                         ├── component.xml
                         └── operation1.xml
        The following is a sample available in the component.xml file.
            <code>
                <component name="sample" type="synapse/template">
            </code>
        </td>
    </tr>
    <tr>
        <td>subComponents</td>
        <td>Defines the template files.</td>
    </tr>
    <tr>
        <td>component (under subComponent)</td>
        <td>Defines an operation. ‘name’ attribute defines the name of the operation. The following is an example of what you can find in the component.xml file.
            <code>
                <subComponents>
                    <component name="operation1">
                        <file>operation1.xml</file>
                        <description>sample wso2 connector method</description>
                    </component>
                </subComponents>
            </code>
        </td>
    </tr>
    <tr>
        <td>file</td>
        <td>Name of the file containing the operation.</td>
    </tr>
    <tr>
        <td>description</td>
        <td>Description of the operation</td>
    </tr>
</table> 

### Operation

An operation of a WSO2 EI connector is implemented using a [synapse template](https://docs.wso2.com/display/EI611/Sequence+Template) as mentioned before.
A typical template configuration for an operation would look like below.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<template xmlns="http://ws.apache.org/ns/synapse" name="operation1">
   <parameter name="hostName" />
   <sequence>
      <log level="full">
     		<property name="*******host name********" expression="$func:hostName" />
      </log>
   </sequence>
</template>
```

<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>name</td>
        <td>The name of the operation. This should correspond to the name defined in the subcomponent in the component.xml. </td>
    </tr>
    <tr>
        <td>parameter</td>
        <td>The parameters required for the operation are defined as parameters.</td>
    </tr>
    <tr>
        <td>sequence</td>
        <td>The mediation logic is implemented here.</td>
    </tr>
</table>

The following is a sample of the code in component.xml.

```xml
    <subComponents>
        <component name="operation1">
            <file>operation1.xml</file>
            <description>sample wso2 connector method</description>
        </component>
    </subComponents>
```

The following is a sample code extracted from operation1.xml
```xml
    <template xmlns="http://ws.apache.org/ns/synapse" name="operation1">
```

### Invoking an operation

When invoking an operation from the main integration flow, the connector name defined in the connector.xml would be appended to the respective operation. Invoking the operation would look similar to the following.

```xml
<sample.operation1>
	<hostName>localhost</hostName>
</sample.operation1>
```

## Writing Your First Connector

### Prerequisites

* Download and install Apache Maven.

### Step 1: Create Maven project template


We will use the [Maven archetype](https://github.com/wso2-extensions/archetypes/tree/master/esb-connector-archetype) to generate the Maven project template and sample connector code.

1. Open a terminal and navigate to the directory where you want the connector code to be created and run the following command.

    ```bash
    mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate -DarchetypeGroupId=org.wso2.carbon.extension.archetype -DarchetypeArtifactId=org.wso2.carbon.extension.esb.connector-archetype -DarchetypeVersion=2.0.4 -DgroupId=org.wso2.carbon.esb.connector -DartifactId=org.wso2.carbon.esb.connector.googlebooks -Dversion=1.0.0 -DarchetypeRepository=http://maven.wso2.org/nexus/content/repositories/wso2-public/
    ```

2. Enter the name of the connector and press enter.

3. Next, press ‘Y’ and enter to confirm configuration properties.

    You will observe the following in the logs, if the connector was successfully created.

    ```bash
    [INFO] ----------------------------------------------------------------------------
    [INFO] Using following parameters for creating project from Archetype: org.wso2.carbon.extension.esb.connector-archetype:2.0.4
    [INFO] ----------------------------------------------------------------------------
    [INFO] Parameter: groupId, Value: org.wso2.carbon.esb.connector
    [INFO] Parameter: artifactId, Value: org.wso2.carbon.esb.connector.googlebooks
    [INFO] Parameter: version, Value: 1.0.0
    [INFO] Parameter: package, Value: org.wso2.carbon.esb.connector
    [INFO] Parameter: packageInPathFormat, Value: org/wso2/carbon/esb/connector
    [INFO] Parameter: package, Value: org.wso2.carbon.esb.connector
    [INFO] Parameter: groupId, Value: org.wso2.carbon.esb.connector
    [INFO] Parameter: artifactId, Value: org.wso2.carbon.esb.connector.googlebooks
    [INFO] Parameter: connector_name, Value: Synergy
    [INFO] Parameter: version, Value: 1.0.0
    [WARNING] CP Don't override file /home/user/org.wso2.carbon.esb.connector.googlebooks/src/test/resources/testng.xml
    [WARNING] CP Don't override file /home/user/org.wso2.carbon.esb.connector.googlebooks/src/test/resources/artifacts/ESB/connector/config/Synergy.properties
    [INFO] project created from Archetype in dir: /home/user/org.wso2.carbon.esb.connector.googlebooks
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:15 h
    [INFO] Finished at: 2020-08-10T11:59:34+05:30
    [INFO] ------------------------------------------------------------------------

    ```

    You may observe that the following directory structure is created.

### Step 2: Adding the new connector resources

Now, let's configure files in the org.wso2.carbon.esb.connector.sample/src/main/resources directory:

1. Create a directory named googlebooks_volume in the /src/main/resources directory.

2. Create a file named listVolume.xml with the following content in the googlebooks_volume directory:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <template xmlns="http://ws.apache.org/ns/synapse" name="listVolume">
        <parameter name="searchQuery" description="Full-text search query string." />
        <sequence>
            <property name="uri.var.searchQuery" expression="$func:searchQuery" />
            <call>
                <endpoint>
                    <http method="get" uri-template="https://www.googleapis.com/books/v1/volumes?q={uri.var.searchQuery}" />
                </endpoint>
            </call>
        </sequence>
    </template>
    ```

3. Create a file named component.xml in the googlebooks_volume directory and add the following content.
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <component name="googlebooks_volume" type="synapse/template">
        <subComponents>
            <component name="listVolume">
                <file>listVolume.xml</file>
                <description>Lists volumes that satisfy the given query.</description>
            </component>
        </subComponents>
    </component>
    ```

4. Edit the connector.xml file in the src/main/resources directory and replace the contents with the following dependency:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <connector>
        <component name="sample" package="org.wso2.carbon.connector">
            <dependency component="googlebooks_volume" />
            <description>wso2 sample connector library</description>
        </component>
    </connector>
    ```

5. Create a folder named icon in the /src/main/resources directory and add two icons.(You can download icons from the following location: http://svn.wso2.org/repos/wso2/scratch/connectors/icons/)


### Step 3: Building the connector

Open a terminal, navigate to the org.wso2.carbon.esb.connector.sample directory and execute the following maven command:

```bash
mvn clean install
```

This builds the connector and generates a ZIP file named sample-connector-1.0.0.zip in the target directory.

### Step 4: Testing the connector

1. Open Integration Studio and [create an Integration Project](../../develop/create-integration-project.md) by clicking **New Integration Project**.

2. In the window that appears, make sure you select **Connector Exporter Project"* as a module of the project.

    <img src="../../../assets/img/connectors/connector-project.png" title="Connector Exporter Project" width="600" alt="Connector Exporter Project"/>

3. In the newly created project, navigate to SampleConnector/SampleConnectorConfigs/src/main/synapse-config/api in Integration Studio. Right click and select **New** -> **Rest API**.

4. Select **Create A New API Artifact** and provide below details.
    * Name - sampleAPI
    * Context - /sample

5. Right click on the SampleConnectorConfigs project, and select **Add or Remove Connector**. In the window that appears, select **Add from File System** and select the file path to the `<sample_connector_folder>/target/sample-connector-1.0.0.zip` file. You may observe the sample-connector added in the pallette as shown below.

    <img src="../../../assets/img/connectors/connector-explorer.png" title="Connector Expolorer" width="300" alt="Connector Explorer"/>

6. Switch to source view and update the configuration as below.
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <api context="/sample" name="sampleAPI" xmlns="http://ws.apache.org/ns/synapse">
        <resource methods="POST" uri-template="/listVolume">
            <inSequence>
                <sample.listVolume>
                    <searchQuery>{json-eval($.searchQuery)}</searchQuery>
                </sample.listVolume>
                <respond/>
            </inSequence>
            <outSequence/>
            <faultSequence/>
        </resource>
    </api>
    ```

    <img src="../../../assets/img/connectors/studio-sequence.png" title="Integration Studio Sequence" width="400" alt="Integration Studio Sequence"/>

7. Right click on the SampleConnectorConnectorExporter project -> **New** -> **Add or Remove Connectors** -> **Select ‘workspace’**. Select the connector from the below window and click **OK**. Click **Finish**.

    <img src="../../../assets/img/connectors/workspace-connector.png" title="Connector Workspace" width="400" alt="Connector Workspace"/>

8. To run the project, right click on the project and select **Run As** -> **Run on Micro Integrator**.

9. Select the artifacts to be exported and click **Finish**.

    <img src="../../../assets/img/connectors/select-artifacts.png" title="Select Artifacts" width="300" alt="Select Artifacts"/>

10. Send a POST call to http://localhost:8290/sample/listVolume with the below request payload.
    ```json
    {
	    "searchQuery": "rabbit"
    }
    ```

11. A JSON response containing book information will be returned.

    <img src="../../../assets/img/connectors/json-response.png" title="JSON response" width="800" alt="JSON Response"/>


## Extending Connector Capabilities with Java

In cases where you need to provide custom capabilities that cannot be fulfilled using mediators, we are able to implement this logic in Java within the connector itself and invoking them using the Class Mediator. This capability is useful when creating Technology connectors.

These Java classes should reside inside /src/main/java/org.wso2.carbon.connector/ directory.


### Sample

This sample is an extension to the ‘Writing your first connector’ section. Let us improve the connector with a Java implementation. 

In the same project, you may observe the sampleConnector class created under /src/main/java/org.wso2.carbon.connector/ directory.

<img src="../../../assets/img/connectors/sampleConnector-class.png" title="sampleConnector class" width="200" alt="sampleConnector class"/>

The class would look similar to the following.

```java
public class sampleConnector extends AbstractConnector {

   @Override
   public void connect(MessageContext messageContext) throws ConnectException {
       Object templateParam = getParameter(messageContext, "generated_param");
       try {
           log.info("sample sample connector received message :" + templateParam);
           /**Add your connector code here
           **/
       } catch (Exception e) {
      	throw new ConnectException(e);
       }
   }
}
```

This class is being invoked by `/src/main/resources/sample/sample_template.xml` with the below code segment.

```xml
<class name="org.wso2.carbon.connector.sampleConnector" />
```

Now, let’s add the component containing the sample_template.xml to the connector by adding the below line to connector.xml.

```xml
<dependency component="sample" />
```

After adding this line, the connector.xml should be as below.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<connector>
   <component name="sample" package="org.wso2.carbon.connector">
      <dependency component="googlebooks_volume" />
      <dependency component="sample" />
      <description>wso2 sample connector library</description>
   </component>
</connector>
```

In the sample, when the connect method is invoked, it should log message “sample sample connector received message : <template_param_passed>”.

