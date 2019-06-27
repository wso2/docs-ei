# Sending a Simple Message to a Datasource

Let’s try a simple scenario where a patient makes an inquiry specifying
the doctor's specialization (category) to retrieve a list of doctors
that match the specialization. The required information is stored in an
H2 database that is shipped with this product. We will create a data
service in the ESB profile of WSO2 Enterprise Integrator (WSO2 EI),
which will expose the information in the database, thereby decoupling
the client and the database layer in the back end. The client will then
communicate with the data service hosted in WSO2 EI to get the required
information instead of communicating directly with the back end.

  

**In this tutorial** , we will define a data service in the ESB profile
of WSO2 EI to expose the back-end database. A client can then invoke the
data service to send messages to the database. If you want to use a
back-end service instead of a database, see the tutorial on [sending a
simple message to a service](_Sending_a_Simple_Message_to_a_Service_) .

![](attachments/119132403/119132408.png)

Let's get started!

This tutorial includes the following sections:

-   [Downloading and set up WSO2
    EI](#SendingaSimpleMessagetoaDatasource-DownloadingandsetupWSO2EI)
-   [Starting WSO2
    EI](#SendingaSimpleMessagetoaDatasource-StartingWSO2EI)
-   [Exposing a datasource through a data
    service](#SendingaSimpleMessagetoaDatasource-Exposingadatasourcethroughadataservice)
-   [Sending requests to the
    ESB](#SendingaSimpleMessagetoaDatasource-SendingrequeststotheESB)

### Downloading and set up WSO2 EI

!!! tip

Before you begin

1.  Install Oracle Java SE Development Kit (JDK) version 1.8.\* and set
    the JAVA\_HOME environment variable.
2.  Go to the [product page](https://wso2.com/integration/) of **WSO2
    Enterprise Integrator** , select **Other Installation Options** ,
    and download the **Binary** distribution. Extract the ZIP file of
    the binary. This will be your `          <EI_HOME>         `
    directory.


Let's set up a back-end database for the healthcare service. We will
create a database named `         DATA_SERV_QSG        ` in the
`         <EI_HOME>/samples/data-services/database        ` directory
for this purpose.

1.  Download the
    `                     dataServiceSample.zip                   `
    file and extract it to a location on your computer. Let's call
    this location `          <Dataservice_Home>         ` . This
    contains a DB script for updating the back-end database with
    the channeling information of the healthcare service.
2.  Open a terminal, navigate to the
    `           <Dataservice_Home>          ` directory and execute the
    following command:

        !!! tip
    
        When executing the below command, replace the
        `           <PATH_TO_EI_HOME>          ` with the folder path of
        your WSO2 EI distribution. For example, if your WSO2 EI distribution
        (i.e., `           <EI_HOME>          ` ) is located in the
        `           /Users/Documents/          ` directory, execute the
        following command:
        `           ant -Ddshome=           /Users/Documents/           wso2ei-6.x.x          `
    
        Also, you need to install [Apache Ant](https://ant.apache.org/) to
        execute this command.
    

    ``` java
        ant -Ddshome=<PATH_TO_EI_HOME>
    ```

The database is now updated with information on all available doctors in
the healthcare service.

### Starting WSO2 EI

Follow the steps given below to start the ESB runtime of WSO2 EI and
create the data service"

1.  To start the ESB profile, open a terminal, navigate to the
    `           <EI_HOME>/bin/          ` directory, and execute one of
    the following commands:

    -   [**On MacOS/Linux/CentOS**](#814b503621224e8fbafe827578efbe7f)
    -   [**On Windows**](#5ca44b960199460c91cc7c659dc4de0c)

    ``` java
            sh integrator.sh
    ```

    ``` java
            integrator.bat
    ```

<!-- -->

1.  In your Web browser, navigate to the ESB profile's management
    console using the following URL:
    `                     https://localhost:9443/carbon                    /         `
    .
2.  Log into the management console using the following credentials:
    -   Username: `            admin           `
    -   Password: `            admin           `

### Exposing a datasource through a data service

Now, let's start creating the data service using the management console.

1.  On the **Main** tab, click **Create** under **Data Service** .

2.  In the **Create Data Service** screen, enter
    `           DOCTORS_DataService          ` as the data service name,
    and click **Next** .

3.  Click **Add New Datasource** and enter the following details:

    | Field           | Value                                                                                      |
    |-----------------|--------------------------------------------------------------------------------------------|
    | DatasourceID    | `               default              `                                                     |
    | Datasourcetype  | Select **RDBMS** and **default** .                                                         |
    | Database Engine | `               H2              `                                                          |
    | Driver Class    | `               org.h2.Driver              `                                               |
    | URL             | `               jdbc:h2:file:./samples/data-services/database/DATA_SERV_QSG              ` |
    | User Name       | `               wso2ds              `                                                      |
    | Password        | `               wso2ds              `                                                      |

4.  Click **Save** and then click **Next** to start defining a query.

5.  Now, let's start writing a query to get data from the datasource.  
    First, you need to define a query to get the details of all the
    available doctors from the database.

    1.  Click **Add New Query** to open the **Add New Query** screen.
    2.  Enter the following values:  

        <table>
        <thead>
        <tr class="header">
        <th>Field</th>
        <th>Value</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td><strong>Query ID</strong></td>
        <td><code>                 select_all_DOCTORS_query                </code></td>
        </tr>
        <tr class="even">
        <td><strong>Datasource</strong></td>
        <td><code>                 default                </code></td>
        </tr>
        <tr class="odd">
        <td><strong>SQL</strong></td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>SELECT NAME, HOSPITAL, SPECIALITY, AVAILABILITY, CHARGE FROM PUBLIC.<span class="fu">DOCTORS</span></span></code></pre></div>
        </div>
        </div>
        </div></td>
        </tr>
        </tbody>
        </table>

    3.  Click ****Generate Response**** to automatically create mappings
        for the fields.

    4.  Under the **Result (Output Mapping)** section, change the values
        of the following fields.  

        |                        |                                                      |
        |------------------------|------------------------------------------------------|
        | **Grouped by element** | `                 DOCTORSCollection                ` |
        | **Row name**           | `                 DOCTOR                `            |

        The output mapping will be as shown below.  
        ![](attachments/119132403/119132405.png){width="800"
        height="338"}

                !!! info
        
                Output mapping specifies how the data that is fetched from your
                query will be shown in the response. Note that, by default, the
                output type is **XML** .
        

6.  Click **Save** .
7.  Now, let's create another query that can get the doctor information
    based on specialization.
    1.  Click **Add New Query** to open the **Add New Query** screen.
    2.  Enter the following values:

        <table>
        <thead>
        <tr class="header">
        <th>Field</th>
        <th>Value</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td><strong>Query ID</strong></td>
        <td><code>                 select_DOCTORS_from_SPECIALITY_query                </code></td>
        </tr>
        <tr class="even">
        <td><strong>Datasource</strong></td>
        <td><code>                 default                </code></td>
        </tr>
        <tr class="odd">
        <td><strong>SQL</strong></td>
        <td><div class="content-wrapper">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>SELECT NAME, HOSPITAL, SPECIALITY, AVAILABILITY, CHARGE FROM PUBLIC.<span class="fu">DOCTORS</span> WHERE SPECIALITY=?</span></code></pre></div>
        </div>
        </div>
        </div></td>
        </tr>
        </tbody>
        </table>

    3.  Click **Generate Input Mappings** and a new input mapping record
        will be created.

    4.  Edit the record and change the **Mapping Name** to SPECIALITY,
        and click **Save** . You will now have the following input
        mapping:  
        ![](attachments/119132403/119132406.png){width="842"
        height="109"}

                !!! info
        
                Input mappings allow you to add parameters to a query so that
                you can set the parameter value when executing the
                query. According to the above definition, you need to provide
                the SPECIALTY as an input in order to retrieve the data
                corresponding to the SPECIALTY.
        

    5.  Click **Main Configuration** , to go back to the main
        configuration after adding the input mapping.  

    6.  Click **Generate Response** to automatically create mappings for
        the fields.

    7.  Under the **Result (Output Mapping)** section, change the values
        of the following fields  

        |                        |                                               |
        |------------------------|-----------------------------------------------|
        | **Grouped by element** | `                 DOCTORList                ` |
        | **Row name**           | `                 DOCTOR                `     |

        The output mapping will be as shown below.  
        ![](attachments/119132403/119132407.png){width="800"
        height="363"}  

                !!! info
        
                Output mapping specifies how the data that is fetched from your
                query is shown in the response. Note that, by default, the
                output type is **XML** .
        

8.  Click **Save** and then click **Next** to open the **Operations**
    screen.  
    Since we are exposing the data as a REST resource, we do not need an
    operation. An operation is needed only if you are exposing the data
    as a SOAP operation.
9.  Click Next to open the **Resources** screen.
    1.  Click **Add New Resources** to open the **Add Resources**
        screen.  
        Let's first create a resource to invoke the
        `             select_all_DOCTORS_query            ` :  

        | Field               | Value                                                       |
        |---------------------|-------------------------------------------------------------|
        | **Resource Path**   | `                 /getAllDoctors                `           |
        | **Resource Method** | `                 GET                `                      |
        | **Query ID**        | `                 select_all_DOCTORS_query                ` |

    2.  Click **Save** to save the resource.

10. Now, let's create another resource to invoke the
    `          select_DOCTORS_from_SPECIALITY_query         ` .
    1.  Click **Add New Resources** to open the **Add Resources** screen
        and enter the following details:

        | Field               | Value                                                                   |
        |---------------------|-------------------------------------------------------------------------|
        | **Resource Path**   | `                 /getDoctors                `                          |
        | **Resource Method** | `                 GET                `                                  |
        | **Query ID**        | `                 select_DOCTORS_from_SPECIALITY_query                ` |

    2.  Click **Save** to save the resource.

11. Click **Finish** after you have defined the resources to complete
    the data service creation process. You are now taken to the
    **Deployed Services** screen, which shows all the data services
    deployed on the server including the one you created.  
    ![created new data service
    group](attachments/119132403/119132404.png "created new data service group"){width="1149"
    height="411"}  

### Sending requests to the ESB

Let's send a request to the data service, which is now deployed in the
ESB profile of WSO2 EI. You will need a REST client like curl for this.

1.  Open a command line terminal and enter the following request to get
    the information of all the doctors specializing under surgery.

    ``` java
        curl -v http://localhost:8280/services/DOCTORS_DataService/getDoctors?SPECIALITY=surgery
    ```

        !!! info
    
        You can use the specialties listed below in your request too:
    
        -   `             cardiology            `
    
        -   `             gynaecology            `
    
        -   `             ent            `
    
        -   `             paediatric            `
    

2.  You see the following response message from data service with a list
    of all available doctors and relevant details.

    ``` xml
        <DOCTORSLIST xmlns="http://ws.wso2.org/dataservice">
        <DOCTOR>
            <NAME>thomas collins</NAME>
            <HOSPITAL>grand oak community hospital</HOSPITAL>
            <SPECIALITY>surgery</SPECIALITY>
            <AVAILABILITY>9.00 a.m - 11.00 a.m</AVAILABILITY>
            <CHARGE>7000</CHARGE>
        </DOCTOR>
        <DOCTOR>
            <NAME>anne clement</NAME>
            <HOSPITAL>clemency medical center</HOSPITAL>
            <SPECIALITY>surgery</SPECIALITY>
            <AVAILABILITY>8.00 a.m - 10.00 a.m</AVAILABILITY>
            <CHARGE>12000</CHARGE>
        </DOCTOR>
        <DOCTOR>
            <NAME>seth mears</NAME>
            <HOSPITAL>pine valley community hospital</HOSPITAL>
            <SPECIALITY>surgery</SPECIALITY>
            <AVAILABILITY>3.00 p.m - 5.00 p.m</AVAILABILITY>
            <CHARGE>8000</CHARGE>
        </DOCTOR>
        </DOCTORSLIST>
    ```
