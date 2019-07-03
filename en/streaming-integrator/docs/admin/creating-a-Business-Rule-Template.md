# Creating a Business Rule Template

  

To create a business template using the Business Template editor, follow
the procedure below:

1.  Go to `          <SP_HOME>         ` from the terminal and Access
    the Stream Processor Studio via the
    `          http://<HOST_NAME>:<EDITOR_PORT>/editor         ` URL.  
    -   On Windows: `            editor.bat --run           `
    -   On Linux/Mac OS:  ./ `             editor.sh            `

2.  Access the Business Rules Template Editor via the
    `           http://<HOST_NAME>:<PORT>/template-editor          `
    URL.

        !!! info
    
        The default URL is
        `                       http://localhost:9390/template-editor                     `
    

3.  The Template Editor opens as shown below. There are two views from
    which you can interact and create a template group. **Design view**
    allows you to visualize a template group and interact with it.
    **Code view** allows you to interact with a template group by typing
    content. (For more information about template group structure, see
    [Business Rules Templates](_Business_Rules_Templates_) .)  

        !!! warning
    
        Do not template sensitive information such as passwords in a Siddhi
        application or expose them directly in a Siddhi application. For
        detailed instructions to protect sensitive data by obfuscating them,
        see [Protecting Sensitive Data via the Secure
        Vault](https://docs.wso2.com/display/SP440/Protecting+Sensitive+Data+via+the+Secure+Vault)
        .
    

    ![](attachments/112390768/112390789.png){width="1000"}  
      
    The following sections explain the two methods of creating a
    template group.  to create a template group.  

    -   [Create from Design
        View](#CreatingaBusinessRuleTemplate-CreatefromDesignView)
    -   [Create from code
        view](#CreatingaBusinessRuleTemplate-Createfromcodeview)

## Create from Design View

To create a business rules template group from the design view, follow
the procedure below:  

1.  Enter a UUID (Universally Unique Identifier), name and a description
    for the template group as follows.

    | Field       | Name                             |
    |-------------|----------------------------------|
    | UUID        | sweet-factory                    |
    | Name        | Sweet Factory                    |
    | Description | Analyzes Sweet Factory scenarios |

      
    ![](attachments/112390768/112390788.png){width="571"}  
      

2.  Expand the first rule template that exists by default, and enter the
    following details. (Note that, you need to configure the deployment
    nodes as explained in [Prerequisites for Business
    Rules](Creating-Business-Rules_112390732.html#CreatingBusinessRules-Prerequisites)
    )

    | Field Name     | Value                                                                                               |
    |----------------|-----------------------------------------------------------------------------------------------------|
    | UUID           | identifying-continuous-production-decrease                                                          |
    | Name           | Identify Continuous Production Decrease                                                             |
    | Description    | Alert factory managers if the rate of production continuously decreases for a specified time period |
    | Type           | Template                                                                                            |
    | Instance Count | One                                                                                                 |

      
    ![](attachments/112390768/112390771.png){width="593" height="612"}  
      

3.  To include a Siddhi application template, expand the first template
    that is displayed by default, and enter the following Siddhi
    application template.

    ![](attachments/112390768/112390786.png){height="400"}

    ``` java
        @App:name('SweetFactory-TrendAnalysis')
    
        @source(type='http', @map(type='json'))
        define stream SweetProductionStream (name string, amount double, factoryId int);
    
        @sink(type='log', @map(type='text', @payload("""
        Hi ${username},
        Production at Factory {{factoryId}} has gone
        from {{initalamout}} to {{finalAmount}} in ${timeInterval} seconds!""")))
        define stream ContinousProdReductionStream (factoryId int, initaltime long, finalTime long, initalamout double, finalAmount double);
    
        from SweetProductionStream#window.timeBatch(${timeInterval} sec)
        select factoryId, sum(amount) as amount, currentTimeMillis() as ts
        insert into ProdRateStream;
    
        partition with ( factoryId of ProdRateStream )
        begin
          from every e1=ProdRateStream,
          e2=ProdRateStream[ts - e1.ts <= ${timeRange} and e1.amount > amount ]*,
          e3=ProdRateStream[ts - e1.ts >= ${timeRange} and e1.amount > amount ]
          select e1.factoryId, e1.ts as initaltime, e3.ts as finalTime, e1.amount as initalamout, e3.amount as finalAmount
          insert into ContinousProdReductionStream;
        end;
    ```

4.  To add variable attributes to the script, click **Add Variables** .

        !!! info
    
        A script is a javascript that can be applied when the inputs
        provided by the business user who uses the template need to be
        processed before replacing the values for the template variables.
        e.g., If the average value is not provided, a function within the
        script can derive it by calculating it from the minimum value and
        the maximum value provided by the business user.
    

      
    ![](attachments/112390768/112390781.png){width="400"}  
      

5.  To specify the attributes that need to be considered as variables,
    select the relevant check boxes under **Select templated elements**
    . In this example, you can select the **username** and **timeRange**
    check boxes to to select the attributes with those names as the
    variables  
    ![](attachments/112390768/112390784.png){height="250"}  
    Then click **Add Script** to update the script with the selected
    variuables with auto-generated function bodies as shown below.  
    ![](attachments/112390768/112390783.png){width="616" height="619"}  
      
6.  Edit the script to add the required functions. In this example,
    let's rename `           myFunction1(input)          ` to
    `           getUsername(email)          ` , and
    `           myFunction2(input          ` ) to
    `           validateTimeRange(number)          ` .  
    ![](attachments/112390768/112390782.png){height="400"}

    ``` js
        var username = getUsername('${userInputForusername}');
        var timeRange = validateTimeRange('${userInputFortimeRange}');
        /**
        * Extracts the username from given email
        * @returns Extracted username
        * @param email Provided email
        */
        function getUsername(email) {
            if (email.match(/\S+@\S+/g)) {
                if (email.match(/\S+@\S+/g)[0] === email) {
                    return email.split('@')[0];
                }
                throw 'Invalid email address provided';
            }
            throw 'Invalid email address provided';
        }
    
    
        /**
        * Validates the given value for time range
        * @returns Processed input
        * @param input User given value
        */
        function validateTimeRange(number) {
            if (!isNaN(number) && (number > 0)) {
                return number;
            } else {
                throw 'A positive number expected for time range';
            }
        }
    ```

7.  To generate properties, click **Generate** against **Properties**
    .  
    ![](attachments/112390768/112390780.png){width="185"}  
    This expands the **Properties** section as follows.  
    ![](attachments/112390768/112390770.png){width="473" height="250"}
8.  Enter values for the available properties as follows. For this
    example, let's enter values as shown in the following table.

        !!! info
    
        A property is defined for each templated attribute (defined in the
        `           ${templatedElement          ` } format) so that it is
        self descriptive for the business user who uses the template. The
        values configured for each property is as follows:
    
        -   **Field Name** : The name with which the templated attribute is
            displayed to the business user.
    
        -   **Field Description** : A description of the property for the
            business user to understand its purpose.
    
        -   **Default Value** : The value assigned to the property by
            default. The business user can change this value if required.
    
        -   **Options** : this is an optional configuration that allows you
            to define a set of values for a property so that the business
            user can select the required value from a list. This is useful
            when the the possible value for the property is a limited set
            options.
    

    | Property                                             | Field Name                                                  | Field Description                                                                              | Default Value                                    |
    |------------------------------------------------------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------|
    | `               timeInterval              `          | `               Time interval (in seconds)              `   | `               Production amounts are considered per time interval              `             | `               6              `                 |
    | `               userInputForusername              `  | `               Manager Email ID              `             | `               Email address to show in greeting              `                               | `               example@email.com              ` |
    | `               userInputFortimeRange              ` | `               Time Range (in milliseconds)              ` | `               Time period in which, product amounts are analyzed for decrease              ` | `               5              `                 |

      
    ![](attachments/112390768/112390779.png){height="400"}

      

9.  To save the template, click the save icon at the top of the page.  
    ![](attachments/112390768/112390769.png)

  

## Create from code view

When you use the code view, the same parameters for which you enter
values in the design view are represented as JSON keys. For each
parameter, you can specify a value against the relevant JSON key as
shown in the extract below.

![](attachments/112390768/112390775.png){height="250"}

When you update the code view with a valid template group definition,
the design view is updated simultaneously as shown below.  
  
![](attachments/112390768/112390773.png){width="885" height="400"}

However, if the content you enter in the code view is an invalid
template group, the design view is not updated, and an error is
displayed as follows.

![](attachments/112390768/112390774.png){height="250"}  
  

  
  
When an error is detected in the entered template group structure, the
**Recover** button is displayed with the error message.

![](attachments/112390768/112390772.png){width="602"}  
  
When you click **Recover** , the code view is receted to the latest
detected valid template group definition. At any given time, the design
view displays information based on the latest detected valid template
group definition.

!!! info

It is not recommended to add Siddhi application templates and scripts
using the code view because they need to be provided as a single line,
and the possible escape characters should be handled carefully.


  
  
