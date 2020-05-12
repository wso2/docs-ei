---
title: Unit Testing with Integration Studio - Tutorial
commitHash: 4829c9c3330fa8011650d52066c6fda1f8a7953e
NOTE: This is an auto-generated file do not edit this, Edit content in source repository
---

This tutorial demonstrates the process of creating unit tests to validate the behaviour of the integration solutions developed via the Integration Studio.

### Assumption

This document describes the details of the example within the context of WSO2 Integration Studio, WSO2 EI’s graphical user interface (GUI). This document assumes that you are familiar with WSO2 EI and the [Integration Studio interface](https://ei.docs.wso2.com/en/latest/micro-integrator/overview/quick-start-guide/). To increase your familiarity with Integration Studio, consider completing one or more [WSO2 EI Tutorials](https://ei.docs.wso2.com/en/latest/micro-integrator/use-cases/integration-use-cases/).

This example performs the following actions:

1. Get the value of queryParam `url_key` and will be assigned it to the `payload` property.

2. API will call the secondaryFlow sequence to assign a value for `flowValue` property.

3. In the secondaryFlow sequence, it will check the `payload` property value and based on that value it will route the mediation flow through either firstSubFlow or secondSubFlow sequence.

3. firstSubFlow or secondSubFlow sequence will assign a value for `flowValue` property.

4. Based on the value of `flowValue` property, API will set the payload to the message context and respond it. 


### Set Up the Example

Follow the steps in this procedure to create and run this example in your own instance of Integration Studio. 

1. Start WSO2 Integration Studio ([Installing WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/installing-WSO2-Integration-Studio/)).

2. Create the following scenario on it.

    - api/business-logic.xml<br>
    <img width="60%" src="../../../assets/img/migration-examples/unittest-short-tutorial-main.png">


    - sequence/secondaryFlow.xml<br>
    <img width="30%" src="../../../assets/img/migration-examples/unittest-short-tutorial-secondary.png">

    - sequence/firstSubFlow.xml<br>
    <img width="25%" src="../../../assets/img/migration-examples/unittest-short-tutorial-subFlow.png">

    - sequence/secondSubFlow.xml<br>
    <img width="25%" src="../../../assets/img/migration-examples/unittest-short-tutorial-subFlow.png">

    - Integration solution source
    ```xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">

    <api context="/custom" name="business-logic">
        <resource methods="GET">
            <inSequence>
                <property expression="get-property('query.param.url_key')" name="payload" scope="default" type="STRING"/>
                <sequence key="secondaryFlow"/>
                <switch source="get-property('flowValue')">
                    <case regex="flowValue_1">
                        <payloadFactory description="Set Response Payload" media-type="text">
                            <format>responsePayload_1</format>
                            <args/>
                        </payloadFactory>
                    </case>
                    <default>
                        <payloadFactory description="Set Response Payload" media-type="text">
                            <format>responsePayload_2</format>
                            <args/>
                        </payloadFactory>
                    </default>
                </switch>
                <respond description="respond"/>
            </inSequence>
            <outSequence/>
            <faultSequence>
                <sequence key="ErrorHandling"/>
            </faultSequence>
        </resource>
    </api>
    
    <sequence name="secondaryFlow" trace="disable">
        <switch source="get-property('payload')">
            <case regex="payload_1">
                <sequence key="firstSubFlow"/>
            </case>
            <default>
                <sequence key="secondSubFlow"/>
            </default>
        </switch>
    </sequence>
    
    <sequence name="firstSubFlow" trace="disable">
        <property name="flowValue" scope="default" type="STRING" value="flowValue_1"/>
    </sequence>
    
    <sequence name="secondSubFlow" trace="disable">
        <property name="flowValue" scope="default" type="STRING" value="flowValue_2"/>
    </sequence>

    </definitions>
    ```

### Creating Unit Tests
Once you have developed an integration solution, WSO2 Integration Studio allows you to build unit tests for the following:

- Test mediation sequences, proxy services, and REST apis with multiple test cases
- Test the artifacts with registry resources.
- Test the artifacts with Connectors.

Here we have created unit test suites for each and every units such as API and sequences. You can follow the [Creating a Unit Test Suite](https://ei.docs.wso2.com/en/next/micro-integrator/develop/creating-unit-test-suite/) documentation to getting started with unit test suites for your integration solutions.

The unit test suite code is located in the `test` directory located in the Integration project.

- Unit Test Suite for whole scenario.
    ```xml
    <unit-test>
        <artifacts>
            <test-artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/api/business-logic.xml</artifact>
            </test-artifact>
            <supportive-artifacts>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/secondSubFlow.xml</artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/firstSubFlow.xml</artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/secondaryFlow.xml</artifact>
            </supportive-artifacts>
            <registry-resources/>
            <connector-resources/>
        </artifacts>
        <test-cases>
            <test-case name="TestCaseForTrueResponseCase">
                <input>
                    <request-path>/?url_key=payload_1</request-path>
                    <request-method>GET</request-method>
                </input>
                <assertions>
                    <assertEquals>
                        <actual>$body</actual>
                        <expected><![CDATA[responsePayload_1]]></expected>
                        <message>Respond from the API does not equal to the expected response</message>
                    </assertEquals>
                </assertions>
            </test-case>
            <test-case name="TestCaseForFalseResponseCase">
                <input>
                    <request-path>/?url_key=payload_other</request-path>
                    <request-method>GET</request-method>
                </input>
                <assertions>
                    <assertEquals>
                        <actual>$body</actual>
                        <expected><![CDATA[responsePayload_2]]></expected>
                        <message>Respond from the API does not equal to the expected response</message>
                    </assertEquals>
                </assertions>
            </test-case>
        </test-cases>
        <mock-services/>
    </unit-test>
    ```

- Unit test suite for secondaryFlow sequence.
    ```xml
    <unit-test>
        <artifacts>
            <test-artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/secondaryFlow.xml</artifact>
            </test-artifact>
            <supportive-artifacts>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/firstSubFlow.xml</artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/secondSubFlow.xml</artifact>
            </supportive-artifacts>
            <registry-resources/>
            <connector-resources/>
        </artifacts>
        <test-cases>
            <test-case name="TestCaseForFirstSubFlow">
                <input>
                    <properties>
                        <property name="payload" value="payload_1"/>
                    </properties>
                </input>
                <assertions>
                    <assertEquals>
                        <actual>$ctx:flowValue</actual>
                        <expected><![CDATA[flowValue_1]]></expected>
                        <message>flowValue property value does not equal to the expected value for the payload_1 query param</message>
                    </assertEquals>
                </assertions>
            </test-case>
            <test-case name="TestCaseForSecondSubFlow">
                <input>
                    <properties>
                        <property name="payload" value="payload_2"/>
                    </properties>
                </input>
                <assertions>
                    <assertEquals>
                        <actual>$ctx:flowValue</actual>
                        <expected><![CDATA[flowValue_2]]></expected>
                        <message>flowValue property value does not equal to the expected value for the different query param</message>
                    </assertEquals>
                </assertions>
            </test-case>
        </test-cases>
        <mock-services/>
    </unit-test>
    ```

- Unit test suite for first sub flow sequence
    ```xml
    <unit-test>
        <artifacts>
            <test-artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/firstSubFlow.xml</artifact>
            </test-artifact>
            <supportive-artifacts/>
            <registry-resources/>
            <connector-resources/>
        </artifacts>
        <test-cases>
            <test-case name="firstSubFlow">
                <input>
                    <properties>
                        <property name="flowValue" value="flowValue_1"/>
                    </properties>
                </input>
                <assertions>
                    <assertEquals>
                        <actual>$ctx:flowValue</actual>
                        <expected><![CDATA[flowValue_1]]></expected>
                        <message>flowValue property value does not equal to the expected value</message>
                    </assertEquals>
                </assertions>
            </test-case>
        </test-cases>
        <mock-services/>
    </unit-test>
    ```

- Unit test suite for second sub flow sequence.
    ```xml
    <unit-test>
        <artifacts>
            <test-artifact>
                <artifact>/UnittestSampleProject/src/main/synapse-config/sequences/secondSubFlow.xml</artifact>
            </test-artifact>
            <supportive-artifacts/>
            <registry-resources/>
            <connector-resources/>
        </artifacts>
        <test-cases>
            <test-case name="secondSubFlow">
                <input>
                    <properties>
                        <property name="flowValue" value="flowValue_2"/>
                    </properties>
                </input>
                <assertions>
                    <assertEquals>
                        <actual>$ctx:flowValue</actual>
                        <expected><![CDATA[flowValue_2]]></expected>
                        <message>flowValue property value does not equal to the expected value</message>
                    </assertEquals>
                </assertions>
            </test-case>
        </test-cases>
        <mock-services/>
    </unit-test>
    ```

### Run Unit Test Suites

You can run the created Unit Test Suites using the unit testing server that is included in the embedded Micro Integrator of WSO2 Integration Studio. Right-click the test directory and click Run Unit Test to run all the unit test suites at once, or right-click the particular unit test suite and click Run Unit Test to run a selected unit test suite. Please refer the [Run Unit Test Suites](https://ei.docs.wso2.com/en/next/micro-integrator/develop/creating-unit-test-suite/#run-unit-test-suites) section to get more details.

Once you run the unit test suites, it will start the unit testing server in the console and prints the summary report for the given unit test suite(s) using the response from the unit testing server.

<img width="50%" src="../../../assets/img/migration-examples/unittest-short-tutorial-run-tests.png">

### Download The Project

You can download the ZIP file and extract the contents to get the project code.

<a href="../../../assets/zip/mule-migration/unittest-short-tutorial.zip" download>
    <img src="../../../assets/img/migration-examples/common/download-zip.png" width="200" alt="Download ZIP">
</a>

### Go Further

* Learn more about configuring an [REST API](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/rest-api-properties/) in Studio.
* Learn more about creating [Unit Test Suites](https://ei.docs.wso2.com/en/next/micro-integrator/develop/creating-unit-test-suite/) in Studio.
* Learn more about creating [Mock Services for Unit Tests](https://ei.docs.wso2.com/en/next/micro-integrator/develop/creating-unit-test-suite/#create-mock-service) in Studio.
