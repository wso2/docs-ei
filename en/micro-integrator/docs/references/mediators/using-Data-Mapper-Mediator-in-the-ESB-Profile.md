# Using Data Mapper Mediator in the ESB Profile

-   [Prerequisites](#UsingDataMapperMediatorintheESBProfile-Prerequisites)
-   [Introduction](#UsingDataMapperMediatorintheESBProfile-Introduction)
-   [Creating the ESB configuration
    project](#UsingDataMapperMediatorintheESBProfile-CreatingtheESBconfigurationproject)
-   [Deploying the
    configurations](#UsingDataMapperMediatorintheESBProfile-Deployingtheconfigurations)
-   [Invoking the created REST
    API](#UsingDataMapperMediatorintheESBProfile-InvokingthecreatedRESTAPI)

### Prerequisites

Set up the following prerequisites before you begin.

-   Download and run [WSO2 EI](http://wso2.com/integration) . For
    instructions, see [Running the
    Product](https://docs.wso2.com/display/EI650/Running+the+Product) .
-   Install the WSO2 EI Tooling to use the Data Mapper mediator, which
    supports the data mapping editor. For instructions, see [Installing
    WSO2 Integration
    Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
    .
-   Download and launch a REST client into your web browser. For
    example, this guide uses the [Postman REST
    client](http://www.getpostman.com/) t o send the requests to the ESB
    profile and receive the responses.

### Introduction

This sample demonstrates how to create a mapping configuration for
different data formats using the Data Mapper mediator. It uses a simple
the ESB profile configuration with only a Data Mapper mediator, and a
Respond mediator to check the converted message. T he input employee
message in XML format, and the output engineer message in JSON format,
which is sent to the client as the response.

### Creating the ESB configuration project

Follow the steps below to create an ESB configuration project to contain
the Data Mapping configurations using WSO2 Ei Tooling .

1.  Open WSO2 EI Tooling.
2.  Right click on the **Project Explorer** area, click **New** , and
    then click **EI Solution Project** as shown below.

        !!! info
    
        WSO2 EI Tooling now provides this new option to create an **EI
        Solution Project** for you to define all different configurations
        you need for the project using a wizard.
    

    ![new ESB Solution
    Project](attachments/119131298/119131338.png "new ESB Solution Project"){width="600"}

3.  Enter a name for the project, and untick **Create Connector Exporter
    Project** ( since you do not need Connectors in your configuration)
    in the following wizard page .  
    ![enter details of the new
    project](attachments/119131298/119131337.png "enter details of the new project"){width="500"}
4.  Click **Finish** .You view the following project files created in
    the **Project Explorer** tab.  
    ![created workspace
    files](attachments/119131298/119131352.png "created workspace files")
5.  Right click **ESBDataMappingProject** workspace file, click **New**
    , and then click **REST API** as shown below, to create a new REST
    API project in the ESB profile .  
    ![create a new REST
    API](attachments/119131298/119131336.png "create a new REST API"){width="500"}  
      
    Select **Create A New API Artifact** , and then click **Next** as
    shown below.  
    ![creating the new
    API](attachments/119131298/119131335.png "creating the new API"){width="400"}
6.  Enter a name for the Synapse API Artifact, enter
    `          /convertMenu         ` for **Context** to configure the
    REST API project to listen for POST requests on the
    `          /convertMenu         ` URL, and then click **Finish** as
    shown below.  
    ![](attachments/119131298/119131334.png)
7.  Drag and drop a Data Mapper mediator and a Respond mediator as shown
    below.  
    ![](attachments/119131298/119131315.png)  
8.  Click on the API Resource, and then click on its **Properties** tab,
    and select **True** as the value for the **Post** method as shown
    below, to create the API resource listening to POST requests .  
    ![](attachments/119131298/119131314.png)
9.  Double click on the D ata Mapping mediator, t o configure it. Y ou
    view a dialog box to create a registry resource project.
10. Enter a name for the configuration, and point the Registry Resource
    project to save it as shown below.

        !!! tip
    
        This configuration name is the prefix used for the configuration
        files that you deploy to the EI server related to the Data Mapper.
        Since you created an ESB Solution project, it directly points you to
        that project to save in it. Otherwise, you need to click the
        **Create new project** link, to create a new Registry Resource
        project and then point to it .
    

    ![create the mapping
    configuration](attachments/119131298/119131320.png "create the mapping configuration")

11. Click **OK** . You view the f ollowing Data Mapper diagram editor in
    the new **WSO2 Data Mapper Graphical** perspective.

        !!! info
    
        You can switch to another perspective by either selecting another in
        top toolbar tags or by c licking **Window-\>Perspective-\>Open
        Perspective-\>Other** in the top menu bar.
    

    ![](attachments/119131298/119131313.png)  

12. Create an XML file (e.g., `           input.xml          ` ) by
    copying the following sample content of a food menu, and save it in
    your local file system.

        !!! tip
    
        Use this sample XML message to load the input format to the Data
        Mapper editor.
    

    ``` xml
        <breakfast_menu>
            <food>
                <name>Belgian Waffles</name>
                <price>$5.95</price>
                <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
                <calories>650</calories>
                <orgin>Belgian</orgin>
                <veg>true</veg>
            </food>
            <food>
                <name>Strawberry Belgian Waffles</name>
                <price>$7.95</price>
                <description>Light Belgian waffles covered with strawberries and whipped cream</description>
                <calories>900</calories>
                <orgin>Belgian</orgin>
                <veg>true</veg>
            </food>
            <food>
                <name>Berry-Berry Belgian Waffles</name>
                <price>$8.95</price>
                <description>Light Belgian waffles covered with an assortment of fresh berries and whipped cream</description>
                <calories>900</calories>
                <orgin>Belgian</orgin>
                <veg>true</veg>
            </food>
            <food>
                <name>French Toast</name>
                <price>$4.50</price>
                <description>Thick slices made from our homemade sourdough bread</description>
                <calories>600</calories>
                <orgin>French</orgin>
                <veg>true</veg>
            </food>
            <food>
                <name>Homestyle Breakfast</name>
                <price>$6.95</price>
                <description>Two eggs, bacon or sausage, toast, and our ever-popular hash browns</description>
                <calories>950</calories>
                <orgin>French</orgin>
                <veg>false</veg>
            </food>
        </breakfast_menu>
    ```

13. Right-click on the top title bar of the **Input** box and, click
    **Load Input** as shown below . The operation palettes that appear
    on the left-hand side allows you to provide the input message format
    to begin the mapping.  
    ![](attachments/119131298/119131312.png)
14. Select **XML** as the **Resource Type** as shown below.

        !!! info
    
        You can select one out of the following resource types , to load the
        input and output message formats to Data Mapper.
    
        -   **XML:** to load a sample XML message and WSO2 Data Mapper
            Editor will generate the JSON schema to represent the XML
            according to the WSO2 Data Mapper Schema specification.
        -   **JSON:** to load a sample JSON message.
        -   **CSV:** to load a sample JSON/CSV message. ****For CSV you need
            to provide the column names as the first record** .**
        -   **XSD:** to load an XSD schema file, which defines your XML
            message format.
        -   **JSONSCHEMA:** to load a JSON schema for your message according
            to the WSO2 Data Mapper schema specification.
        -   **CONNECTOR:** to map a message, which is an output of a
            Connector. Select the **Connector** **Type** in the **Input**
            box, and it will list down all available connectors. Then,
            select the operation from the menu that appears in front of Data
            Mapper mediator.
    

    ![select the resource
    type](attachments/119131298/119131318.png "select the resource type")

15. Click the **file system** link in **Select resource from** , select
    the XML file you saved in your local file system in [step
    12](#UsingDataMapperMediatorintheESBProfile-load-input) , and click
    **Open** .  
    You view the input format loaded in the **Input** box in the editor
    as shown below.

    ![](attachments/119131298/119131311.png)

16. Create another XML file (e.g., `           output.xml          ` )
    by copying the following sample content of a food menu, and save it
    in your local file system.  

        !!! tip
    
        Use this sample XML message to load the output format to the Data
        Mapper editor.
    

    ``` xml
        <menu>
            <item>
                <name>Belgian Waffles</name>
                <price>$5.95</price>
                <calories>650</calories>
                <orgin>Belgian</orgin>
                <veg>true</veg>
                <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
            </item>
            <item>
                <name>Strawberry Belgian Waffles</name>
                <price>$7.95</price>
                <calories>900</calories>
                <orgin>Belgian</orgin>
                <veg>true</veg>
                <description>Light Belgian waffles covered with strawberries and whipped cream</description>
            </item>
            <item>
                <name>Berry-Berry Belgian Waffles</name>
                <price>$8.95</price>
                <calories>900</calories>
                <orgin>Belgian</orgin>
                <veg>true</veg>
                <description>Light Belgian waffles covered with an assortment of fresh berries and whipped cream</description>
            </item>
            <item>
                <name>French Toast</name>
                <price>$4.50</price>
                <calories>600</calories>
                <orgin>French</orgin>
                <veg>true</veg>
                <description>Thick slices made from our homemade sourdough bread</description>
            </item>
            <item>
                <name>Homestyle Breakfast</name>
                <price>$6.95</price>
                <calories>950</calories>
                <orgin>French</orgin>
                <veg>false</veg>
                <description>Two eggs, bacon or sausage, toast, and our ever-popular hash browns</description>
            </item>
        </menu>
    ```

17. Right-click on the top title bar of the **Output** box and, click
    **Load Output** as shown below . The operation palettes that appear
    on the left-hand side allows you to provide the output message
    format.  
    ![](attachments/119131298/119131310.png)  
18. Click the **file system** link in **Select resource from** , select
    the XML file you saved in your local file system in [step
    16](#UsingDataMapperMediatorintheESBProfile-load-output) , and click
    **Open** .  
    You view the input format loaded in the **Output** box in the editor
    as shown below.  
    ![](attachments/119131298/119131309.png)
19. Check the **Input** and **Output** boxes with the sample messages,
    to see if the element types (i.e. (Arrays, Objects and Primitive
    values) are correctly identified or not. Following signs will help
    you to identify them correctly.  
    -   {} - represents object elements
    -   \[\] - represents array elements  
    -   \<\> - represents primitive field values
    -   A - represents XML attribute values
20. Click on the Data Mapper mediator. You view the following in the
    **Properties** tab of the Data Mapper mediator configuration as
    shown below.  

    -   **Configuration:** Script file that is used to execute the
        mapping.
    -   **Input Schema:** JSON schema, which represents the input
        message format.
    -   **Output Schema:** JSON schema, which represents the output
        message format.
    -   **Input Type:** Expected input message type (xml/json/csv).
    -   **Output Type:** Target output message type (xml/json/csv).

    Check if you set the input and type and output type correctly.

        !!! note
    
        If you do not set the input type and output type correctly in the
        mediator configuration, your mapping will fail during runtime.
    

    ![](attachments/119131298/119131308.png)

21. Follow the steps below to do the mapping using operators as shown
    below.

        !!! note
    
        -   The mapping done in the below example is that: name is mapped
            via uppercase operator and calories undergo a mathematical
            calculation to get the output as follows:
    
            `             output calories =Round( (calories*1.13) + 6.75)            `
    
        -   You can only connect primitive data values such as Strings,
            numbers, boolean and etc. You cannot map Array and object
            values.
    

    1.  Drag and drop an **Upper Case** operator and connect the
        **name** in both **Input** and **Output** boxes to it.  
        ![](attachments/119131298/119131307.png)
    2.  Connect **price** in the **Input** box to the same in the
        **Output** box.  
        ![](attachments/119131298/119131306.png)
    3.  Connect **description** in the **Input** box to the same in the
        **Output** box.  
        ![](attachments/119131298/119131305.png)
    4.  Connect **origin** in the **Input** box to the same in the
        **Output** box.  
        ![](attachments/119131298/119131304.png)
    5.  Connect **veg** in the **Input** box to the same in the
        **Output** box.  
        ![](attachments/119131298/119131303.png)
    6.  Dran and drop the following operators: **Multiply** , **Add** ,
        **Round**  
        **![](attachments/119131298/119131302.png)  
        **
    7.  Drag and drop a **Constant** operator, and enter **1.13** as its
        **Value** in the **Properties** section.

                !!! tip
        
                To update the titile of the **Constant** box with the value,
                save the diagram, close the **FoodMapping.datamapper\_diagram**
                and re-open it by double-clicking on the **Data Mapper** icon in
                the **FoodMenuConversion.xml** file.
        

        ![](attachments/119131298/119131301.png)

    8.  Drag and drop another **Constant** operator, and enter **6.75**
        as its **Value** in the **Properties** section.  
        ![](attachments/119131298/119131300.png)
    9.  Connect the **calories** variable in both the **Input** and
        **Output** boxes via the **Operators** as shown below.

        | From                         | To                            |
        |------------------------------|-------------------------------|
        | **Input** Box → **calories** | **Multiply** Operator         |
        | **Constant 1.13**            | **Multiply** Operator         |
        | **Multiply** Operator        | **Add** Operator              |
        | **Constant 6.75**            | **Add** Operator              |
        | **Add** Operator             | **Round** Operator            |
        | **Round** Operator           | **Output** Box → **calories** |

        ![](attachments/119131298/119131299.png)  
          

22. Press **Ctrl+S** keys in each tab, to save all the configurations.

### Deploying the configurations

After creating the Data Mapper configurations, follow the steps below to
deploy the created REST API and the configurations in the ESB profile by
including them in a C-App.

1.  Open WSO2 EI Tooling.  

2.  Expand the C-APP project that was created when you created the ESB
    Solution project (i.e. **EIDataMappingProjectCompositeApplication**
    ), and double-click on the POM file. You view the following screen
    to select project files into the C-APP.

        !!! info
    
        You need to refresh the screen to view the registry resource files
        . Once you refresh the screen, you view all the artifacts in the
        workspace.
    

    ![select
    files](attachments/119131298/119131333.png "select files"){width="800"}

3.  Click the refresh button in the top right-hand corner to load newly
    added registry files, as shown below.

    ![click the Refresh
    button](attachments/119131298/119131346.png "click the Refresh button")

4.  Select the REST API file and the three registry resource files
    containing the mapping configuration, input schema, and output
    schema as shown below.  

    -   **Configuration:** Script file that is used to execute the
        mapping.
    -   **Input schema:** JSON schema which represents the input message
        format.
    -   **Output schema:** JSON schema which represents the output
        message format.

    ![select all configuration
    files](attachments/119131298/119131326.png "select all configuration files"){width="700"}

5.  Start WSO2 ESB server. For instructions, see [Running the
    Product](https://docs.wso2.com/display/ESB500/Running+the+Product) .
6.  Click the **Servers** Tab in WSO2 EI Tooling, and click the **No
    servers are available. Click this link to create a new server...**
    link as shown below.  
    ![adding a new
    server](attachments/119131298/119131331.png "adding a new server"){width="800"}
7.  Click **WSO2** , click **WSO2 Carbon remote server** , and then
    click **Next** as shown below.  
    ![](attachments/119131298/119131330.png)  
8.  Enter the URL of the WSO2 EI for **Server URL** , and click
    **Finish** as shown below.  
    ![enter the URL of the ESB
    server](attachments/119131298/119131343.png "enter the URL of the ESB server")
    You view the WSO2 EI server added in the **Servers** tab as shown
    below.  
    ![view the ESB server
    added](attachments/119131298/119131342.png "view the ESB server added")
9.  Right-click on **WSO2 Carbon remote server at localhost** , and then
    click **Add & Remove** .
10. Select the C-App in the **Available:** box, click **Add** to move it
    to the **Configured:** box, and then click **Finish** as shown
    below.  
    ![select C-App](attachments/119131298/119131328.png "select C-App")
    You view the C-App added to the WSO2 EI server as shown below.  
    ![added C-App](attachments/119131298/119131329.png "added C-App")
11. Log in to the WSO2 EI Management Console using the following URL and
    admin/admin credentials: https://\<ESB\_HOST\>:\<ESB\_PORT\>carbon/
12. Click **Main** , and then click **APIs** in the **Service Bus**
    menu. You view the deployed REST API invocation URL as shown
    below.  
    ![ESB Management
    Console](attachments/119131298/119131327.png "ESB Management Console"){width="900"}

### Invoking the created REST API

Follow the steps below to test invoking the created REST API.

1.  Open Postman REST client.
2.  Enter the following details to create the client message, enter the
    content of the XML file you created in [step
    12](#UsingDataMapperMediatorintheESBProfile-load-input) as the
    payload in the text area provided, and click **Send** as shown
    below.  

    -   **URL:** http://:8280/convertMenu
    -   **Method:** POST
    -   **Body:** raw xml /application
    -   **Message:** Enter the inpu

    ![request sent from
    client](attachments/119131298/119131323.png "request sent from client"){width="900"}  
    You view the expected JSON message received as shown below.  
    ![response
    received](attachments/119131298/119131322.png "response received"){width="900"}

Similarly, you can use the above instructions to check the following
message conversions:

-   T he input employee message in XML format, and the output engineer
    message in XML/JSON/CSV formats, which is sent to the client as the
    response. (i.e. XML-\>XML/JSON/CSV)
-   T he input employee message in JSON format, and the output engineer
    message in XML/JSON/CSV formats, which is sent to the client as the
    response. (i.e. JSON-\>XML/JSON/CSV)
-   T he input employee message in CSV format, and the output engineer
    message in XML/JSON/CSV formats, which is sent to the client as the
    response. (i.e. CSV-\>XML/JSON/CSV)

!!! note

In the above sample, the output message format is fully compatible to
represent as JSON and CSV. However, t his is not guaranteed in every
occasion. For example, if you have defined a complex XML output message
with namespaces and attributes, JSON message or CSV will not be built as
expected.

