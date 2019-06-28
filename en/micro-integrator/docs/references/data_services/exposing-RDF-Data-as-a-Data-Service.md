# Exposing RDF Data as a Data Service

This tutorial will guide you on how to expose an RDF datasource as a
data service using the **Create New Data Service** wizard. To
demonstrate this feature, we will use a sample RDF file that is shipped
with WSO2 EI by default. Given below are the details of this datasource:

The RDF file ( `         Movies.rdf        ` ), stored in the
`         <EI_HOME>/samples/data-services/resources        ` folder
contains data about some popular movies. Each movie data has the
following sub elements: "title", "director", "year", "genre" and
"actor".  

![](images/icons/grey_arrow_down.png){.expand-control-image} Movies.rdf
file

\<?xml version="1.0"?\>  
\<rdf:RDF  
xmlns:rdf="
[http://www.w3.org/1999/02/22-rdf-syntax-ns\#](http://www.w3.org/1999/02/22-rdf-syntax-ns)
"  
xmlns:cd="
[http://www.popular.movies/cd\#](http://www.popular.movies/cd) "\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Roman> Holiday"\>  
\<cd:title\>Roman Holiday\</cd:title\>  
\<cd:director\>William Wyler\</cd:director\>  
\<cd:year\>1953\</cd:year\>  
\<cd:genre\>Romance\</cd:genre\>  
\<cd:actor\>Gorella Gori\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Bridget> Jones's Diary"\>  
\<cd:title\>Bridget Jones's Diary\</cd:title\>  
\<cd:director\>Cilla Ware\</cd:director\>  
\<cd:year\>2001\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>John Clegg\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/The> Proposal"\>  
\<cd:title\>The Proposal\</cd:title\>  
\<cd:director\>Anne Fletcher\</cd:director\>  
\<cd:year\>2009\</cd:year\>  
\<cd:genre\>Romance\</cd:genre\>  
\<cd:actor\>Alexis Garcia\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/The> Princess Diaries"\>  
\<cd:title\>The Princess Diaries\</cd:title\>  
\<cd:director\>Garry Marshall\</cd:director\>  
\<cd:year\>2001\</cd:year\>  
\<cd:genre\>Comedy\</cd:genre\>  
\<cd:actor\>John McGivern\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Austenland> "\>  
\<cd:title\>Austenland\</cd:title\>  
\<cd:director\>Jerusha Hess\</cd:director\>  
\<cd:year\>2013\</cd:year\>  
\<cd:genre\>Adaptation\</cd:genre\>  
\<cd:actor\>Georgia King\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Beginners> "\>  
\<cd:title\>Beginners\</cd:title\>  
\<cd:director\>Mike Mills\</cd:director\>  
\<cd:year\>2011\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Seth T Walker\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Flipped> "\>  
\<cd:title\>Flipped\</cd:title\>  
\<cd:director\>Rob Reiner\</cd:director\>  
\<cd:year\>2010\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Michele Messmer\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Silver> Linings Playbook"\>  
\<cd:title\>Silver Linings Playbook\</cd:title\>  
\<cd:director\>David O. Russell\</cd:director\>  
\<cd:year\>2012\</cd:year\>  
\<cd:genre\>Adaptation\</cd:genre\>  
\<cd:actor\>Anthony Lawton\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Begin> Again"\>  
\<cd:title\>Begin Again\</cd:title\>  
\<cd:director\>John Carney\</cd:director\>  
\<cd:year\>2014\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Eric Burton\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Adventureland> "\>  
\<cd:title\>Adventureland\</cd:title\>  
\<cd:director\>Greg Mottola\</cd:director\>  
\<cd:year\>2009\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Ryan Mcfarland\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/The> Spectacular Now"\>  
\<cd:title\>The Spectacular Now\</cd:title\>  
\<cd:director\>James Ponsoldt\</cd:director\>  
\<cd:year\>2013\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Andre Royo\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/WALL-E> "\>  
\<cd:title\>WALL-E\</cd:title\>  
\<cd:director\>Andrew Stanton\</cd:director\>  
\<cd:year\>2008\</cd:year\>  
\<cd:genre\>Adventure\</cd:genre\>  
\<cd:actor\>Ben Burtt\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Love> Actually"\>  
\<cd:title\>Love Actually\</cd:title\>  
\<cd:director\>Richard Curtis\</cd:director\>  
\<cd:year\>2003\</cd:year\>  
\<cd:genre\>Comedy\</cd:genre\>  
\<cd:actor\>Tiffany Boysell\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Kicking> and Screaming"\>  
\<cd:title\>Kicking and Screaming\</cd:title\>  
\<cd:director\>Jesse Dylan\</cd:director\>  
\<cd:year\>2005\</cd:year\>  
\<cd:genre\>Comedy\</cd:genre\>  
\<cd:actor\>Willie Amakye\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Harold> and Maude"\>  
\<cd:title\>Harold and Maude\</cd:title\>  
\<cd:director\>Hal Ashby\</cd:director\>  
\<cd:year\>1971\</cd:year\>  
\<cd:genre\>Comedy\</cd:genre\>  
\<cd:actor\>G Wood\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Catch> Me If You Can"\>  
\<cd:title\>Catch Me If You Can\</cd:title\>  
\<cd:director\>Steven Spielberg\</cd:director\>  
\<cd:year\>2002\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Antoine Drolet Dumoulin\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/Ratatouille> "\>  
\<cd:title\>Ratatouille\</cd:title\>  
\<cd:director\>Jan Pinkava\</cd:director\>  
\<cd:year\>2007\</cd:year\>  
\<cd:genre\>Drama\</cd:genre\>  
\<cd:actor\>Teddy Newton\</cd:actor\>  
\</rdf:Description\>

\<rdf:Description  
rdf:about=" <http://www.popular.movies/cd/The> Dark Knight"\>  
\<cd:title\>The Dark Knight\</cd:title\>  
\<cd:director\>Christopher Nolan\</cd:director\>  
\<cd:year\>2008\</cd:year\>  
\<cd:genre\>Crime\</cd:genre\>  
\<cd:actor\>Andy Luther\</cd:actor\>  
\</rdf:Description\>

\</rdf:RDF\>

See the following topics for instructions. Also, see the samples in
[Data Integration
Samples](https://docs.wso2.com/display/EI650/Data+Integration+Samples) .

-   [Creating a data
    service](#ExposingRDFDataasaDataService-Creatingadataservice)
    -   [Connecting to the
        datasource](#ExposingRDFDataasaDataService-Connectingtothedatasource)
    -   [Creating a query to GET
        data](#ExposingRDFDataasaDataService-CreatingaquerytoGETdata)
    -   [Creating a SOAP operation to invoke the
        query](#ExposingRDFDataasaDataService-CreatingaSOAPoperationtoinvokethequery)
    -   [Creating a REST resource to invoke the
        query](#ExposingRDFDataasaDataService-CreatingaRESTresourcetoinvokethequery)
    -   [Finish creating the data
        service](#ExposingRDFDataasaDataService-Finishcreatingthedataservice)
-   [Invoking your data service using
    SOAP](#ExposingRDFDataasaDataService-InvokingyourdataserviceusingSOAP)
-   [Invoking your data service using
    REST](#ExposingRDFDataasaDataService-InvokingyourdataserviceusingREST)

------------------------------------------------------------------------

### Creating a data service

Follow the steps given below.

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

    -   [**On MacOS/Linux/CentOS**](#7a649d50ffa4466fa01f7cd262851b80)
    -   [**On Windows**](#d3d335d548524f8e822c22d7e47c407d)

    Open a terminal and execute the following command:

    ``` java
        wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

3.  Log in to the product's management conso le.
4.  Click **Data Service → Create** , to start creating a data service.
5.  Enter the following name for the data service.

    |                   |                |
    |-------------------|----------------|
    | Data Service Name | RDFDataService |

6.  Click **Next** to enter the datasource connection details.

#### Connecting to the datasource

Follow the steps given below.

1.  Click **Add New Datasource** and enter the following details:

    |                   |                                                                                                                                                                                       |
    |-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Datasource ID     | RDF                                                                                                                                                                                   |
    | Datasource Type   | RDF                                                                                                                                                                                   |
    | RDF File Location | Enter the path to the RDF file. In this tutorial we are using a sample RDF file that is shipped with your product. The file location is ./samples/data-services/resources/Movies.rdf. |

2.  Save the datasource.
3.  Click **Next** , to start creating queries.

#### Creating a query to GET data

Now let's start writing a query for getting data from the
`         Movies.        ` rdf file. The query will specify the data
that should be fetched by this query, and the format that should be used
to display data when the query is invoked.

1.  Click **Add New Query** and enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td>GetMoviebyGenre</td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>RDF</td>
    </tr>
    <tr class="odd">
    <td>SPARQL</td>
    <td><div class="content-wrapper">
    <p>In this field, enter the sparql query describing the data that should be retrieved from the RDF datasource. We will use the following query:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>PREFIX cd: &lt;http:<span class="co">//www.popular.movies/cd#&gt;</span></span>
    <span id="cb1-2"><a href="#cb1-2"></a> </span>
    <span id="cb1-3"><a href="#cb1-3"></a>SELECT ?title ?director ?year ?genre ?actor </span>
    <span id="cb1-4"><a href="#cb1-4"></a>WHERE {   </span>
    <span id="cb1-5"><a href="#cb1-5"></a>      ?movie cd:title ?title. </span>
    <span id="cb1-6"><a href="#cb1-6"></a>      ?movie cd:director ?director. </span>
    <span id="cb1-7"><a href="#cb1-7"></a>      ?movie cd:year ?year. </span>
    <span id="cb1-8"><a href="#cb1-8"></a>      ?movie cd:genre ?genre. </span>
    <span id="cb1-9"><a href="#cb1-9"></a>      ?movie cd:actor ?actor.</span>
    <span id="cb1-10"><a href="#cb1-10"></a>}</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  The input mapping section is used to specify the type of input that
    should be given in order to get the output result. Click **Add New
    Input Mapping** to start creating the input mapping. We will create
    an input mapping for 'genre', which will allow us to query for data
    based on the genre of the movie.

    <table style="width:100%;">
    <colgroup>
    <col style="width: 34%" />
    <col style="width: 65%" />
    </colgroup>
    <tbody>
    <tr class="odd">
    <td>Mapping Name</td>
    <td>genre</td>
    </tr>
    <tr class="even">
    <td>Sparql Type</td>
    <td>String</td>
    </tr>
    </tbody>
    </table>

3.  Now let's specify output mappings to specify how the data fetched
    from the query should be displayed. We will create output mappings
    for all the types of data in the RDF file: **title** , **director**
    , **year** , **genre** and **actor** . Follow the steps given
    below.  
    1.  Start by giving the following information:

        |                    |                                                                                                                                                   |
        |--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
        | Output type        | You can select XML, JSON or RDF. We will use **XML** for this tutorial. This specifies the format in which the query results should be presented. |
        | Grouped by element | Enter **Movies** . This will be the XML element that will group the query result.                                                                 |
        | Row Name           | Enter **Movie** . This is the XML element that should group each individual result.                                                               |

    2.  Click **Add New Output Mapping** to start creating the output
        mapping for the **title** column.
    3.  Click **Add** to save the output mapping. You will now have one
        output mapping listed for the **GetMoviesbyGenre** query.
    4.  Now, add the following output mappings:

        | Mapping Type | Element Name | Datasource Type | Datasource Column Name | Parameter Type | Schema Type |
        |--------------|--------------|-----------------|------------------------|----------------|-------------|
        | Element      | title        | Column          | title                  | SCALAR         | string      |
        | Element      | director     | Column          | director               | SCALAR         | string      |
        | Element      | year         | Column          | year                   | SCALAR         | string      |
        | Element      | genre        | Column          | genre                  | SCALAR         | string      |
        | Element      | actor        | Column          | actor                  | SCALAR         | string      |

    5.  Click **Main Configuration** to return to the **Query** screen.

4.  Save the query, and click **Next** to go to the **Operations**
    screen.

#### Creating a SOAP operation to invoke the query

To invoke the query, you need to define an operation.

1.  Click **Add New Operation** and enter the following information.  

    |                |                    |
    |----------------|--------------------|
    | Operation Name | GetMoviesbyGenreOp |
    | Query ID       | GetMoviesbyGenre   |

2.  Save the operation.

You can now invoke the data service query using SOAP.

#### Creating a REST resource to invoke the query

Now, let's create REST resources to invoke the query created above.
Alternatively, you can create SOAP operations to invoke the queries. See
the [previous section](_Exposing_RDF_Data_as_a_Data_Service_) for
instructions.

1.  Click **Add New Resource** and enter the following information.  

    |                 |                  |
    |-----------------|------------------|
    | Resource Path   | Movies/{genre}   |
    | Resource Method | GET              |
    | Query ID        | GetMoviesbyGenre |

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
2.  Click the Try this service link for the Excel data service. The
    TryIt Tool will open with the **RDF** service.  
3.  Enter **Drama** as the genre: \<xs:Genre
    xmlns:xs="http://ws.wso2.org/dataservice"\>Drama\</xs:Genre\>
4.  Select the **GetMoviesbyGenre** operation and click **Send.**
5.  The following XML response will be published on the TryIt tool:

    ``` java
            Movies xmlns="http://ws.wso2.org/dataservice">
               <Movie>
                  <title>Ratatouille</title>
                  <director>Jan Pinkava</director>
                  <year>2007</year>
                  <genre>Drama</genre>
                  <actor>Teddy Newton</actor>
               </Movie>
               <Movie>
                  <title>Catch Me If You Can</title>
                  <director>Steven Spielberg</director>
                  <year>2002</year>
                  <genre>Drama</genre>
                  <actor>Antoine Drolet Dumoulin</actor>
               </Movie>
               <Movie>
                  <title>The Spectacular Now</title>
                  <director>James Ponsoldt</director>
                  <year>2013</year>
                  <genre>Drama</genre>
                  <actor>Andre Royo</actor>
               </Movie>
               <Movie>
                  <title>Adventureland</title>
                  <director>Greg Mottola</director>
                  <year>2009</year>
                  <genre>Drama</genre>
                  <actor>Ryan Mcfarland</actor>
               </Movie>
               <Movie>
                  <title>Begin Again</title>
                  <director>John Carney</director>
                  <year>2014</year>
                  <genre>Drama</genre>
                  <actor>Eric Burton</actor>
               </Movie>
               <Movie>
                  <title>Flipped</title>
                  <director>Rob Reiner</director>
                  <year>2010</year>
                  <genre>Drama</genre>
                  <actor>Michele Messmer</actor>
               </Movie>
               <Movie>
                  <title>Beginners</title>
                  <director>Mike Mills</director>
                  <year>2011</year>
                  <genre>Drama</genre>
                  <actor>Seth T Walker</actor>
               </Movie>
               <Movie>
                  <title>Bridget Jones's Diary</title>
                  <director>Cilla Ware</director>
                  <year>2001</year>
                  <genre>Drama</genre>
                  <actor>John Clegg</actor>
               </Movie>
            </Movies>
    ```

### Invoking your data service using REST

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

``` java
    curl -X GET http://localhost:8280/services/RDFDataService.HTTPEndpoint/Movies/Drama
```

This will return the response in XML.
