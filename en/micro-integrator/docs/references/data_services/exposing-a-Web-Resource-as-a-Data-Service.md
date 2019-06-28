# Exposing a Web Resource as a Data Service

This tutorial will guide you on how to expose a web resource as a data
service using the ESB profile of WSO2 Enterprise Integrator (WSO2 EI).
You can create a data service that can fetch selected data from a web
resource and display the data in XML, JSON or RDF form.

Follow the instructions below. Also, see the samples in [Data
Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Creating a data
    service](#ExposingaWebResourceasaDataService-Creatingadataservice)
    -   [Connecting to the
        datasource](#ExposingaWebResourceasaDataService-Connectingtothedatasource)
    -   [Creating a query to GET
        data](#ExposingaWebResourceasaDataService-CreatingaquerytoGETdata)
    -   [Creating a SOAP operation to invoke the
        query](#ExposingaWebResourceasaDataService-CreatingaSOAPoperationtoinvokethequery)
    -   [Creating a REST resource to invoke the
        query](#ExposingaWebResourceasaDataService-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#ExposingaWebResourceasaDataService-Finishcreatingthedataservice)
-   [Invoking your data service using
    SOAP](#ExposingaWebResourceasaDataService-InvokingyourdataserviceusingSOAP)
-   [Invoking your data service using
    REST](#ExposingaWebResourceasaDataService-InvokingyourdataserviceusingREST)

  

### Creating a data service

Now, let's start creating the data service from scratch:

1.  Download the product installer from
    [here](http://wso2.com/integration/) , and run the installer.  
    Let's call the installation location of your product the
    **\<EI\_HOME\>** directory.

    If you installed the product using the **installer** , this is
    located in a place specific to your OS as shown below:

    <table style="width:100%;">
    <colgroup>
    <col style="width: 9%" />
    <col style="width: 90%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>OS</th>
    <th>Home directory</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Mac OS</td>
    <td><code>                /Library/WSO2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>Windows</td>
    <td><code>                C:\Program Files\WSO2\EnterpriseIntegrator\6.5.0\               </code></td>
    </tr>
    <tr class="odd">
    <td>Ubuntu</td>
    <td><code>                /usr/lib/wso2/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    <tr class="even">
    <td>CentOS</td>
    <td><code>                /usr/lib64/EnterpriseIntegrator/6.5.0               </code></td>
    </tr>
    </tbody>
    </table>

2.  Start the ESB profile:

    -   [**On MacOS/Linux/CentOS**](#cdec92ec54504ee68eefa37c5489cba8)
    -   [**On Windows**](#0ae1e8763eee4920bfa6848199f8810d)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Log in to the management console of ESB profile of WSO2 EI using the
    following URL on your browser:  "
    [https://localhost:9443/carbon/](https://10.100.5.65:9443/carbon/)
    ".
4.  Click **Create** under the **Data Service** menu to open the
    **Create Data Service** window.
5.  Enter the following data service name.

    |                   |             |
    |-------------------|-------------|
    | Data Service Name | WebResource |

6.  Leave the default values for the other fields.
7.  Click **Next** to go to the **Datasources** screen.

#### Connecting to the datasource

You can add a web resource as the datasource by following the steps
given below.

1.  Click **Add New Datasource** and enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>Enter <strong>WebHarvestDataSource</strong> .</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>This specifies the type of datasource for which you will create the data service. Select <strong>Web Datasource</strong> from the list.</td>
    </tr>
    <tr class="odd">
    <td>Inline Web Harvest Config</td>
    <td><div class="content-wrapper">
    S elect <strong>Inline Web Harvest Config</strong> and enter the configuration as shown below:
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <pre class="html/xml" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: html/xml; gutter: false; theme: Confluence"><code>&lt;config&gt;
     &lt;var-def name=&#39;AppInfo&#39;&gt;
      &lt;xslt&gt;
       &lt;xml&gt;
        &lt;html-to-xml&gt;
         &lt;http method=&#39;get&#39; url=&#39;https://play.google.com/store/apps&#39;/&gt;
        &lt;/html-to-xml&gt;
       &lt;/xml&gt;
       &lt;stylesheet&gt;
       &lt;![CDATA[ &lt;xsl:stylesheet version=&quot;1.0&quot; xmlns:xsl=&quot;http://www.w3.org/1999/XSL/Transform&quot;&gt;
            &lt;xsl:output method=&quot;xml&quot; omit-xml-declaration=&quot;yes&quot; indent=&quot;yes&quot;/&gt;
            &lt;xsl:template match=&quot;/&quot;&gt;
                     &lt;AppInfo&gt;
                        &lt;xsl:for-each select=&quot;//*[@class=&#39;details&#39;]&quot;&gt;
                        &lt;App&gt;
                         &lt;Title&gt;&lt;xsl:value-of select=&quot;a[@class=&#39;title&#39;]&quot;/&gt;&lt;/Title&gt;
                         &lt;Description&gt;&lt;xsl:value-of select=&quot;div[@class=&#39;description&#39;]&quot;/&gt;&lt;/Description&gt;
                        &lt;/App&gt;
                      &lt;/xsl:for-each&gt;
                     &lt;/AppInfo&gt;
                    &lt;/xsl:template&gt;
             &lt;/xsl:stylesheet&gt;
       ]]&gt;
       &lt;/stylesheet&gt;
      &lt;/xslt&gt;
     &lt;/var-def&gt;
    &lt;/config&gt;</code></pre>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  Save the datasource.
3.  Click **Next** to go to the **Queries** screen.  

#### Creating a query to GET data

Now let's start writing a query for getting data from the web resource.
The query will specify the data that should be fetched by this query,
and the format that should be used to display the data.

1.  Click **Add New Query** and enter the following details:

    |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
    |-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Query ID          | E nter **webquery** as the query ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Datasource        | Select the datasource for which you are going to write a query. Select the web datasource that you created previously.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | Scrapper Variable | Enter **AppInfo** . When you add a query to a Web datasource, you must enter a **Scraper Variable** . This scraper variable must be the same as the output name in the web datasource configuration, which returns the output from the configuration. In this example, the `               var-def              ` name in the configuration is `               weatherInfo              ` ( `               <              ` `               var-def              ` `               name              ` `               =              ` `               'AppInfo'              ` `               >              ` ). |

2.  Define **Output Mapping** : Now, let's specify how the data fetched
    from the datasource should be displayed in the output. We will
    create output mappings for the following data in the web resource:
    **Title** and **Description** .
    1.  Start by giving the following details:

        |                    |                                                                                                                                                 |
        |--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
        | Output Type        | S pecify the format in which the query results should be presented. You can select XML, JSON or RDF. We will use **XML** for this tutorial.     |
        | Grouped by element | Specify a grouping for all the output mappings. This will be the XML element that will group the query result. Enter **AppInfo** in this field. |
        | Row Name           | S pecify the XML element that should group each individual result.  Enter **App** in this field.                                                |

    2.  Click **Add New Output Mapping** to start creating the output
        mapping for the **Title** field.  
    3.  Click **Add** to save the output mapping.
    4.  Now, add another output mapping for the **Description** column.

    5.  You will now have the following output mappings listed for the
        **webquery** query:  
        ![](attachments/119130756/119130758.png)

3.  Click **Next** to go to the **Operations** screen.

#### Creating a SOAP operation to invoke the query

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.  

    |                |               |
    |----------------|---------------|
    | Operation Name | GetResourceOp |
    | Query ID       | WebQuery      |

2.  Save the operation.

You can now invoke the data service query using SOAP.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_a_Web_Resource_as_a_Data_Service_) for
instructions.

1.  Click **Add New Resource** and enter the following information.  

    |                 |          |
    |-----------------|----------|
    | Resource Path   | AppInfo  |
    | Resource Method | GET      |
    | Query ID        | webquery |

2.  Save the resource.

You can now invoke the data service query using REST.

#### Finish creating the data service

Once you have defined the operation, click **Finish** to complete the
data service creation process. You will now be taken to the **Deployed
Services** screen, which shows all the data services deployed on the
server.

### Invoking your data service using SOAP

You can try the data service you created by using the TryIt tool that is
in your product by default.

1.  Go to the **Deployed Services** screen.
2.  Click the **Try this service** link for the WebResource data
    service. The TryIt Tool will open with the data service.
3.  Select the **GetResource** operation and click **Send.**
4.  The response will be published.

### Invoking your data service using REST

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

``` java
    curl -X GET http://localhost:8280/services/WebResource.HTTPEndpoint/AppInfo
```

This will return the response in XML.
