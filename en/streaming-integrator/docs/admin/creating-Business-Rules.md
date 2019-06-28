# Creating Business Rules

This section explains how to create a business rule. A business rule can
be created from a template or from scratch.

-   [Prerequisites](#CreatingBusinessRules-Prerequisites)
-   [Creating a business rule from a
    template](#CreatingBusinessRules-Creatingabusinessrulefromatemplate)
-   [Creating a business rule from
    scratch](#CreatingBusinessRules-Creatingabusinessrulefromscratch)

### Prerequisites

In order to create a business rule from a template, the following
prerequisites must be completed:

-   Both the Dashboard and Worker runtimes of the Stream Processor
    server must be started.
-   The templates for business rules must be defined in a template
    group, and must be configured in the
    `          <SP_HOME>/conf/dashboard/deployment.yaml         ` file.
    For detailed instructions, see [Business Rules
    Templates](_Business_Rules_Templates_) .

### Creating a business rule from a template

Creating business rules from an existing template allows you to use
sources, sinks and filters already defined and assign variable values to
process events.

To create a business rule from a template, follow the procedure below:

1.  Go to `          <SP_HOME>         ` from the terminal and start the
    Dashboard runtime of WSO2 SP with one of the following commands:  
    -   On Windows: `            dashboard.bat --run           `
    -   On Linux/Mac OS:  ./ `             dashboard.sh            `

2.  Start the Worker runtime of WSO2 SP with one of the following
    commands:
    -   On Windows: `            worker.bat --run           `
    -   On Linux/Mac OS:  ./ `             worker.sh            `

3.  Access the Business Rule Manager via one of the following URLs.

      

    | Protocol | URL Format                                                                   | Example                                                            |
    |----------|------------------------------------------------------------------------------|--------------------------------------------------------------------|
    | HTTP     | `               http://<SP_HOST>:<HTTP_PORT>/business-rules              `   | `               http://0.0.0.0:9090/business-rules              `  |
    | HTTPS    | `               https://<SP_HOST>:<HTTPS_PORT>/business-rules              ` | `               https://0.0.0.0:9443/business-rules              ` |

    This opens the following:  
    ![](attachments/112390732/112390740.png){height="150"}

4.  Click **Create** to open the following page.  
    ![](attachments/112390732/112390739.png){width="682" height="250"}
5.  Then click **From Template** to open the **Select a Template Group**
    page where the available templates are displayed.  
6.  Click on the template group that contains the required template to
    create a rule from it. In this example, the rule is created based on
    a template in the `          Sweet Factory         ` template group
    that is packed with WSO2 SP by default. Therefore, click **Sweet
    Factory** to open this template group.  
    ![](attachments/112390732/112390734.png){height="250"}
7.  In the template group, expand the **Rule Template** list as shown
    below, and click on the required template. For this example, click
    **Identify Continuous Production Decrease** .  
    ![](attachments/112390732/112390733.png){height="250"}  
8.  If you want to change the rule template from which you want to
    create the rule, select the required value for the **Rule Template**
    field.
9.  Enter a name for the business rule in the **Business Rule Name**
    field.
10. Enter values for the rest of the fields following the instructions
    in the UI.

        !!! info
    
        The fields displayed for the rule differ based on the template
        selected.
    

11. If you want to save the rule and deploy it later, click **Save** .
    If you want to deploy the rule immediately, click **Save and
    Deploy** .

### Creating a business rule from scratch

Creating a rule from scratch allows you to define the filter logic for
the rule at the time of creating instead of using the filter logic
already defined in a template. However, you can select the required
source and sink configurations from an existing template.

To create a business rule from scratch, follow the procedure below:

1.  Start the Dashboard Portal of WSO2 SP with one of the following
    commands:  
    -   On Windows: `            dashboard.bat --run           `
    -   On Linux/Mac OS: `             sh dashboard.sh            `

2.  Start the Worker runtime of WSO2 SP with one of the following
    commands:
    -   On Windows: `            worker.bat --run           `
    -   On Linux/Mac OS: `             ./worker.sh            `

3.  Access the Business Rule Manager via one of the following URLs.

      

    | Protocol | URL Format                                                                   | Example                                                                                           |
    |----------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
    | HTTP     | `               http://<SP_HOST>:<HTTP_PORT>/business-rules              `   | `                               http://0.0.0.0:9090/business-rules                             `  |
    | HTTPS    | `               https://<SP_HOST>:<HTTPS_PORT>/business-rules              ` | `                               https://0.0.0.0:9443/business-rules                             ` |

    This opens the following:  
    ![](attachments/112390732/112390740.png){height="150"}

4.  Click **Create** to open the following page, and then click **From
    Scratch** .  
    ![](attachments/112390732/112390739.png){height="250"}
5.  This opens the **Select a Template Group** page where the available
    template groups are displayed as shown in the example below.  
    ![](attachments/112390732/112390749.png){width="342" height="250"}
6.  Click on the template group from which you want to select the
    required sources and sinks for your business rule. For this
    example, click **Stock Exchange** to open that template group as
    shown below.  
    ![](attachments/112390732/112390738.png){height="400"}
7.  Click **Input** to expand the **Input** section. Then select the
    rule template from which the source and input configurations for the
    business rule must be selected.  
    ![](attachments/112390732/112390737.png){height="250"}  
    This displays the list of available sources and the exposed
    attributes of the selected template as shown below.  
    ![](attachments/112390732/112390736.png){width="710"}
8.  Click **Filters** to expand the **Filters** section, and click **+**
    to add a new filter. A table is displayed as shown below.  
    ![](attachments/112390732/112390743.png){width="813" height="360"}
9.  To define a filter, follow the steps below:  

    1.  In the **Attribute** field, select the attribute based on which
        you want to define the filter condition.
    2.  In the **Operator** field, select an operator.
    3.  In the **Value/Attribute** field, enter the value or the
        attribute based on which the operator is applied to the
        attribute you selected in step **a** .

    e.g., If you want to filter events where the price is less than 100,
    select values for the fields as follows:

    | Field               | Value                                |
    |---------------------|--------------------------------------|
    | **Attribute**       | `               price              ` |
    | **Operator**        | `               <              `     |
    | **Value/Attribute** | `               100              `   |

      
    Once you have defined two or more filters, enter the rule logic in
    the **Rule Logic** field using `           OR          ` ,
    `           AND          ` and `           NOT          `
    conditions. The examples of how you can use these keywords are
    explained in the table below.

    | Keyword                            | Example                                                                                |
    |------------------------------------|----------------------------------------------------------------------------------------|
    | `               OR              `  | `               1 OR 2              ` returns events that match either filter 1 or 2.  |
    | `               AND              ` | `               1 AND 2              ` returns events that match both filters 1 and 2. |
    | `               NOT              ` | `               NOT 1              ` returns events that do not match filter 1.        |

10. Click **Output** to expand the **Output** section. Then select the
    rule template from which the sink and output connfigurations for the
    business rule must be selected.  
    ![](attachments/112390732/112390742.png)  
    This displays the section for mapping configurations as shown in the
    example below.  
    ![](attachments/112390732/112390735.png){width="700"}
11. Select the relevant attribute names for the **Input** column. When
    publishing the events to which the rule is applied via the selected
    predefined sink, each input event you select is published with the
    corresponding name in the **Output** column.

        !!! info
    
        The output mappings displayed differ based on the output rule
        template you select.
    

12. If you want to save the rule and deploy it later, click **Save** .
    If you want to deploy the rule immediately, click **Save and
    Deploy** .
