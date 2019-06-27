# Configuring Business Rule Deployment

  

WSO2 Business Rules Manager uses ruleTemplates within templateGroups, to
derive business rules from. Each ruleTemplate will have a UUID - that is
used to uniquely identify itself. When a ruleTemplate is specified under
a worker node; SiddhiApps derived in business rules created out of that
ruleTemplate will be deployed in the specified worker node.

1.  Open the `          deployment.yaml         ` file in the
    `          <WSO2SP_HOME>/conf/dashboard         ` directory.
2.  Deployment configurations for the Business Rules Manager are
    specified under `           wso2.business.rules.manager          ` .
    Provide the URL(s) of worker node(s) that is/are available to deploy
    SiddhiApps, under `           deployment_configs          ` in the
    `           <HOST_NAME>:<PORT>          ` format.

    ``` powershell
        deployment_configs:
            - <NODE1_HOST_NAME>:<NODE1_PORT>
              <NODE2_HOST_NAME>:<NODE2_PORT>
    ```

    Eg:

    ``` powershell
            deployment_configs:
                - localhost:9090
                  10.100.4.140:9090
    ```

3.  List down the UUIDs of required rule templates under each node. This
    results in Siddhi applications created out of the business rules
    derived from those templates being deployed in the required nodes.

    ``` powershell
            deployment_configs:
                - <NODE1_HOST_NAME>:<NODE1_PORT>:
                    - ruleTemplate1_UUID
                    - ruleTemplate2_UUID
                    - ruleTemplate3_UUID
    ```

    e.g.,

    ``` powershell
            deployment_configs:
                - localhost:9090:
                   - sweet-production-kpi-analysis
                   - stock-exchange-input
                   - stock-exchange-output
    ```

4.  If required, you can enter a specific rule template under multiple
    nodes as shown below.

        !!! info
    
        Before entering a specific rule template under multiple node, make
        sure that you have selected **Many** for the **Instance Count**
        field of the template. For more information, see [Creating a
        Business Rule
        Template](https://docs.wso2.com/display/SP440/Creating+a+Business+Rule+Template#CreatingaBusinessRuleTemplate-InstanceCount)
        .
    

    ``` powershell
        deployment_configs:
            - <NODE1_HOST_NAME>:<NODE1_PORT>:
                - ruleTemplate1_UUID
                - ruleTemplate2_UUID
                - ruleTemplate3_UUID
              <NODE2_HOST_NAME>:<NODE2_PORT>:
                - ruleTemplate1_UUID
                - ruleTemplate3_UUID
              <NODE3_HOST_NAME>:<NODE3_PORT>:
                - ruleTemplate2_UUID
                - ruleTemplate3_UUID
                - ruleTemplate4_UUID
    ```

    e.g.,

    ``` powershell
            deployment_configs:
                - localhost:9090:
                    - sweet-production-kpi-analysis
                    - stock-exchange-input
                    - stock-exchange-output
                  10.100.40.169:9090:
                    - identifying-continuous-production-decrease
                    - sweet-production-kpi-analysis
    ```

    Note that the rule template with the
    `           sweet-production-kpi-analysis          ` UUID has been
    configured under two worker nodes. As indicated by this, if a
    business rule is derived from
    `           sweet-production-kpi-analysis          ` , Siddhi
    Applications created from it are deployed in both the nodes.

5.  Specify the username and password that are common for all the worker
    nodes.

    ``` powershell
            username: admin
            password: admin
    ```

      
    The complete deployment configuration for Business Rules looks as
    follows.

    ``` powershell
            wso2.business.rules.manager:
                datasource: BUSINESS_RULES_DB
                deployment_configs:
                    - localhost:9090:
                        - stock-data-analysis
                        - stock-exchange-input
                        - stock-exchange-output
                        - identifying-continuous-production-decrease
                        - sweet-production-kpi-analysis
                username: admin
                password: admin
    ```
