# Managing Business Rules

This section covers how to view, edit, deploy and delete business rules
that are created from a template.

-   [Prerequisites](#ManagingBusinessRules-Prerequisites)
-   [Viewing business
    rules](#ManagingBusinessRules-Viewingbusinessrules)
-   [Editing business
    rules](#ManagingBusinessRules-Editingbusinessrules)
-   [Deploying business
    rules](#ManagingBusinessRules-Deployingbusinessrules)
-   [Undeploying business
    rules](#ManagingBusinessRules-Undeployingbusinessrules)
-   [Viewing deployment
    information](#ManagingBusinessRules-ViewingdeploymentinformationDeploymentInformation)
-   [Deleting business
    rules](#ManagingBusinessRules-Deletingbusinessrules)

### Prerequisites

In order to manage business rules, the following prerequisites must be
completed:

-   One or more business rules must be already created. For instructions
    to create a rule, see [Creating Business
    Rules](_Creating_Business_Rules_) .
-   The Business Rules Manager should be started and accessed by
    following the procedure below.
    1.  Start the Dashboard Portal of WSO2 SP with one of the following
        commands:  
        -   On Windows: `              dashboard.bat --run             `
        -   On Linux/Mac OS:
            `               ./dashboard.sh              `

    2.  Start a worker node with one of the following commands:  
        -   On Windows: `              worker.bat --run             `
        -   On Linux/Mac OS: `               ./worker.sh              `

    3.  Access the Business Rule Manager via one of the following URLs.

        | Protocol | URL Format                                                               | Example                                                                                                    |
        |----------|--------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
        | HTTP     | `                 http://<SP_HOST>:<HTTP_PORT>/portal                `   | `                                   http://0.0.0.0:9443/business-rules/                                 `  |
        | HTTPS    | `                 https://<SP_HOST>:<HTTPS_PORT>/portal                ` | `                                   https://0.0.0.0:9443/business-rules/                                 ` |

### Viewing business rules

Once you start and access the Business Rules Manager, the available
business rules are displayed as shown in the example below.

![](attachments/112390751/112390758.png){width="900"}

To view a business rule, click the icon for viewing (marked in the above
image) for the relevant row. This opens the rule as shown in the example
below.

![](attachments/112390751/112390754.png){width="675" height="557"}

### Editing business rules

Once you start and access the Business Rules Manager, the available
business rules are displayed as shown in the example below.

![](attachments/112390751/112390757.png){width="900"}

To edit a business rule, click the icon for editing (marked in the above
image) for the relevant row. This opens the rule as shown in the example
below.

![](attachments/112390751/112390755.png){width="675" height="614"}

Modify values for the parameters displayed as required and click
**Save** .

### Deploying business rules

To deploy a saved business rule in a worker node, follow the steps
below:

1.  Start the worker node by issuing one of the following commands:  
    -   On Windows: `            worker.bat --run           `
    -   On Linux/Mac OS: `             ./worker.sh            `

2.  Start the dashboard server and access the Business Rules Manager.
3.  Click the icon for deploying (marked in the image below) for the
    relevant row. As a result, a message appears to inform you that the
    rule is successfully deployed.  
    ![](attachments/112390751/112390760.png){width="900"}

  

### Undeploying business rules

To undeploy a business rule, click the icon for undeploying (marked in
the image below) for the relevant row. As a result, a message appears to
inform you that the rule is successfully undeployed.

![](attachments/112390751/112390759.png){width="900"}

### Viewing deployment information

If you want to view information relating to the deployment of a business
rule, click the icon for viewing deployment information (marked in the
image below) for the relevant row.  
  
![](attachments/112390751/112390753.png){width="900" height="223"}

As a result, the deployment information including the host and port of
the nodes in which the rule is deployed and the deployment status are
displayed as shown in the image below.  
![](attachments/112390751/112390752.png){width="343" height="250"}

Possible deployment statuses are as follows:

-   **Saved** : The business rule is created, but not yet deployed in
    any SP node.
-   **Deployed** : The business rule is created and deployed in all the
    nodes of the current SP setup.
-   **Partially Deployed:** The business rule is created and deployed
    only in some of the nodes in the SP cluster. This status is also
    assigned when you click **Save and Deploy** instead of **Save** at
    the time you [create the business rule](_Creating_Business_Rules_) .
-   **Partially Undeployed:** The business rule has been previously
    deployed and then undeployed only in some of the nodes in the SP
    cluster.

### Deleting business rules

To delete a business rule, click the icon for deleting (marked in the
image below) for the relevant row. A message appears to confirm whether
you want to proceed with the deletion. Click **Delete** in the message.
As a result, another message appears to inform you that the rule is
successfully deleted.

![](attachments/112390751/112390756.png){width="900"}

  
