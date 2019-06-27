# Scheduling Tasks

Task scheduling is used to invoke a data service operation periodically
or for a specified number of times. You cannot insert data into the
database using scheduling tasks. But you can use schedule tasks to
update operations, such as the SQL update and delete operations.

The scheduling functionality is useful when a specific data service
operation is associated with an [input or output
event-trigger](Receiving-Notifications-from-Data-Services_119133285.html#ReceivingNotificationsfromDataServices-Inputeventtrigger)
. When a scheduled task that is associated with an event-trigger is run,
the event is automatically fired by evaluating the event trigger
criteria. For example, we can schedule a task on the
`         getProductQuantity        ` operation and set an event to send
an email if the quantity goes down to some level.

In this tutorial, you create an operation to add data to an employee
database by keeping the Email column as `         NULL        ` . Next,
you create a scheduled task to update the emails that are
`         NULL        ` to `         no-reply@wso2.com        ` .

  

!!! tip

Before you begin!

Follow the steps given below to set up a MySQL database for this
tutorial.

!!! tip

If you have already tried the previous tutorials, you can skip steps
1-5.


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

2.  Install the MySQL server.
3.  Download the JDBC driver for MySQL from
    [here](http://dev.mysql.com/downloads/connector/j/) and copy it
    to your `            <EI_HOME>/lib           ` directory.

        !!! note
    
        If the driver class does not exist in the relevant folders when you
        create the datasource, you will get an exception, such as '
        `            Cannot load JDBC driver class com.mysql.jdbc.Driver           `
        '.
    

4.  Create a database named `            Employees           ` .

    ``` java
        CREATE DATABASE Employees;
    ```

5.  Create the Employee table inside the Employees database:

    ``` java
            USE Employees;
        
            CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
    ```

6.  Add the following records to the table:

    ``` java
            insert into Employees (EmployeeNumber, FirstName, LastName, Salary) values (100, 'Chris', 'Adam', '10000');
            insert into Employees (EmployeeNumber, FirstName, LastName, Email, Salary) values (101, 'Alex', 'pat', 'alex@wso2.com', '10000');
            insert into Employees (EmployeeNumber, FirstName, LastName, Salary) values (102, 'John', 'Doe', '10000');
    ```

7.  View the data added to the database by executing the command given
    below. Note that the emailn address is marked as
    `            NULL           ` for Chris and John.

    ``` java
            Select * from Employees;
    ```

    Example output:

        !!! info
    
        More records are displayed if you have tried the previous tutorials.
    

    ``` java
        +----------------+-----------+----------+---------------+--------+
        | EmployeeNumber | FirstName | LastName | Email         | Salary |
        +----------------+-----------+----------+---------------+--------+
        |            100 | Chris     | Adam     | NULL          | 10000  |
        |            101 | Alex      | pat      | alex@wso2.com | 10000  |
        |            102 | John      | Doe      | NULL          | 10000  |
        +----------------+-----------+----------+---------------+--------+
    ```


  

**Let's get started!**

-   [Creating the data service](#SchedulingTasks-Creatingthedataservice)
-   [Creating a scheduled task](#SchedulingTasks-Creatingascheduledtask)
-   [Confirming the scheduled task
    functionality](#SchedulingTasks-Confirmingthescheduledtaskfunctionality)

### Creating the data service

!!! info

If you have already tried the [Defining Nested
Queries](_Defining_Nested_Queries_) tutorial, you have already created
the `         EmployeeDataService        ` data service. If so, you can
edit the existing data service, and add new queries and operations
required for this tutorial. To do this, click on **EmployeeDataService**
in the **Deployed Services** page, and then click **Edit Data Service
(Wizard)** .

![](images/icons/grey_arrow_down.png){.expand-control-image} Click here
to view an image of the link.

![](attachments/119133308/119133309.png){width="900" height="501"}

To add new queries and operations, start from the [Creating a query to
update data](#SchedulingTasks-operation) section.


  

Follow the steps given below.

1.  Start the WSO2 ESB profile.

    -   [**On MacOS/Linux/CentOS**](#13a7a67a24854a01b55a7b4b313b5104)
    -   [**On Windows**](#85cb66fe71f8429a8503c2b43a115010)

    Open a terminal and execute the following command:

    ``` java
        sudo wso2ei-6.5.0-integrator
    ```

    Go to **Start Menu -\> Programs -\> WSO2 -\> Enterprise Integrator
    6.5.0 Integrator.** This will open a terminal and start the ESB
    profile.

2.  Open the ESB profile's Management Console using
    <https://localhost:9443/carbon> , and log in using
    `          admin         ` as the username and the password.
3.  Click **Data Service → Create** , to start creating a data service.
4.  Enter the following name for the data service.

    |                   |                     |
    |-------------------|---------------------|
    | Data Service Name | EmployeeDataService |

5.  Click **Next** to enter the datasource connection details.

#### Connecting to the datasource

Follow the steps given below.

1.  Click **Add New Datasource** and enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Datasource ID</td>
    <td>Datasource</td>
    </tr>
    <tr class="even">
    <td>Datasource Type</td>
    <td>RDBMS</td>
    </tr>
    <tr class="odd">
    <td>Datasource Type (Default/External)</td>
    <td>Leave <strong>Default</strong> selected.</td>
    </tr>
    <tr class="even">
    <td>Database Engine</td>
    <td>MySQL</td>
    </tr>
    <tr class="odd">
    <td>Driver Class</td>
    <td><code>               com.mysql.jdbc.Driver              </code></td>
    </tr>
    <tr class="even">
    <td>URL</td>
    <td><code>               jdbc:mysql://localhost:3306/Employees              </code></td>
    </tr>
    <tr class="odd">
    <td>User Name</td>
    <td>Enter your MySQL server's username.</td>
    </tr>
    <tr class="even">
    <td>Password</td>
    <td>Enter your MySQL server's password.<br />
    If you have not assigned a password, keep this field empty.</td>
    </tr>
    </tbody>
    </table>

        !!! info
    
        If you enter **External** instead of the Default datasource type,
        your datasource should be supported by an external provider class,
        such as
        `           com.mysql.jdbc.jdbc2.optional.MysqlXADataSource.          `
        You can select the **External** option and enter the name and value
        of connection properties by clicking **Add Property** . For
        example,  
        ![](attachments/119133308/119133311.png){width="533" height="250"}  
        After an external datasource is created, it can be used as a
        usual datasource in queries. See the tutorial on [handling
        distributed transactions](_Handling_Distributed_Transactions_) for
        more information on using external datasources.
    

2.  Save the datasource.
3.  Click **Next** , to start creating queries.

#### Creating a query to update data

Now, let's create a query that can update an existing employee record in
the datasource.

1.  Click **Add New Query** to start creating a new query.

2.  Enter the following details:

    <table>
    <tbody>
    <tr class="odd">
    <td>Query ID</td>
    <td><code>               UpdateEmployeeDetails              </code></td>
    </tr>
    <tr class="even">
    <td>Datasource</td>
    <td>Datasource</td>
    </tr>
    <tr class="odd">
    <td>SQL</td>
    <td><div class="content-wrapper">
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>update Employees set Email = &#39;no-reply<span class="at">@wso2</span>.<span class="fu">com</span>&#39; where Email is NULL;</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

#### Creating SOAP operations to invoke queries

Now, let's create SOAP operations to invoke the queries created above.
Alternatively, you can create REST resources to invoke the queries. See
the [next section](#SchedulingTasks-CreateRESTresourcestoinvokequeries)
for instructions.

1.  Click **Add New Operation** and enter the details as shown below.

    |                |                                                      |
    |----------------|------------------------------------------------------|
    | Operation Name | `               UpdateEmployeeOp              `      |
    | Query ID       | `               UpdateEmployeeDetails              ` |

2.  Save the operation.

3.  Click **Finish** to complete creating the data service.

### Creating a scheduled task

Follow the steps given below to schedule a task.

1.  On the WSO2 ESB management console, click **Data Services \>
    DS Scheduled Tasks.**  
    ![Data Services Scheduled Tasks
    menu](attachments/119133308/119133317.png)

2.  Click **Add New Task** to open the **New Scheduled Task** screen.

    <table>
    <tbody>
    <tr class="odd">
    <td>Task Name</td>
    <td>Enter a name for the task in the <strong>Task Name</strong> field.<br />
    For this tutorial, enter <code>               UpdateEmailTask              </code> as the name.</td>
    </tr>
    <tr class="even">
    <td>Task Repeat Count</td>
    <td>Number of cycles to be executed. If you enter 0, the task will execute once. If you enter 1, the task will execute twice and so on.<br />
    Enter <code>               1              </code> to execute the task twice.</td>
    </tr>
    <tr class="odd">
    <td>Task Interval</td>
    <td>The time gap between two consecutive task executions.<br />
    Enter <code>               300              </code> .</td>
    </tr>
    <tr class="even">
    <td>Start Time</td>
    <td>The starting time of the scheduled task. If this is not given, the task will start as soon as it is scheduled.<br />
    For this tutorial, let's keep this field blank.</td>
    </tr>
    <tr class="odd">
    <td>Scheduling Type</td>
    <td><div class="content-wrapper">
    <ol>
    <li><ul>
    <li><p><strong>DataService Operation</strong> : If this option is selected, the task will be invoking a data service operation.<br />
    Select <strong>Data Service Operation</strong> for this tutorial.<br />
    Follow the steps given below:</p>
    <ol>
    <li><strong>Data Service Name:</strong> Name of the relevant data service. Select <code>                      EmployeeDataService                     </code> .</li>
    <li><p><strong>Operation Name:</strong> Data service operation to be executed from the task. Select <code>                       UpdateEmployeeOp                      </code> .</p></li>
    </ol>
        <p>!!! info</p>
        <p>Note that only data services with HTTP endpoints are available when scheduling tasks to invoke data service operations. Also, you can use only operations with no input parameters when scheduling.</p>
    </p></li>
    <li><p><strong>DataService Task Class</strong> : If this option is selected, the task will be using a custom class that implements the <code>                     org.wso2.carbon.dataservices.task.DataTask                    </code> interface.</p>
    <div id="expander-1549340057" class="expand-container">
    <div id="expander-control-1549340057" class="expand-control">
    <img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Click here for more details.
    </div>
    <div id="expander-content-1549340057" class="expand-content">
    <p>If the scheduling type is <strong>DataService Task Class</strong> , you must specify the Java class that implements the <code>                       org.wso2.carbon.dataservices.task.DataTask                      </code> interface in the <strong>DataService Task Class</strong> field.</p>
        !!! info
        <p>The definition of the interface is as follows:</p>
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a><span class="kw">package</span><span class="im"> org.wso2.carbon.dataservices.task;</span></span>
        <span id="cb1-2"><a href="#cb1-2"></a></span>
        <span id="cb1-3"><a href="#cb1-3"></a><span class="co">/**</span></span>
        <span id="cb1-4"><a href="#cb1-4"></a> <span class="co">*</span> This interface represents a scheduled data task<span class="co">.</span></span>
        <span id="cb1-5"><a href="#cb1-5"></a> <span class="co">*/</span></span>
        <span id="cb1-6"><a href="#cb1-6"></a><span class="kw">public</span> <span class="kw">interface</span> DataTask </span>
        <span id="cb1-7"><a href="#cb1-7"></a>    <span class="dt">void</span> <span class="fu">execute</span>(DataTaskContext ctx);</span>
        <span id="cb1-8"><a href="#cb1-8"></a>}</span></code></pre></div>
        </div>
        </div>
        <p>The following code snippet shows a sample DataTask implementation:</p>
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a><span class="kw">package</span><span class="im"> samples;</span></span>
        <span id="cb2-2"><a href="#cb2-2"></a><span class="kw">import</span><span class="im"> java.util.HashMap;</span></span>
        <span id="cb2-3"><a href="#cb2-3"></a><span class="kw">import</span><span class="im"> java.util.Map;</span></span>
        <span id="cb2-4"><a href="#cb2-4"></a><span class="kw">import</span><span class="im"> org.wso2.carbon.dataservices.core.DataServiceFault;</span></span>
        <span id="cb2-5"><a href="#cb2-5"></a><span class="kw">import</span><span class="im"> org.wso2.carbon.dataservices.core.engine.ParamValue;</span></span>
        <span id="cb2-6"><a href="#cb2-6"></a><span class="kw">import</span><span class="im"> org.wso2.carbon.dataservices.task.DataTask;</span></span>
        <span id="cb2-7"><a href="#cb2-7"></a><span class="kw">import</span><span class="im"> org.wso2.carbon.dataservices.task.DataTaskContext;</span></span>
        <span id="cb2-8"><a href="#cb2-8"></a></span>
        <span id="cb2-9"><a href="#cb2-9"></a><span class="kw">public</span> <span class="kw">class</span> SampleDataTask <span class="kw">implements</span> DataTask {    </span>
        <span id="cb2-10"><a href="#cb2-10"></a>   <span class="at">@Override</span>    </span>
        <span id="cb2-11"><a href="#cb2-11"></a>   <span class="kw">public</span> <span class="dt">void</span> <span class="fu">execute</span>(DataTaskContext ctx) {</span>
        <span id="cb2-12"><a href="#cb2-12"></a>       <span class="bu">Map</span>&lt;<span class="bu">String</span>, ParamValue&gt; params = <span class="kw">new</span> <span class="bu">HashMap</span>&lt;<span class="bu">String</span>, ParamValue&gt;();</span>
        <span id="cb2-13"><a href="#cb2-13"></a>       params.<span class="fu">put</span>(<span class="st">&quot;increment&quot;</span>, <span class="kw">new</span> <span class="fu">ParamValue</span>(<span class="st">&quot;1000&quot;</span>));</span>
        <span id="cb2-14"><a href="#cb2-14"></a>       params.<span class="fu">put</span>(<span class="st">&quot;employeeNumber&quot;</span>, <span class="kw">new</span> <span class="fu">ParamValue</span>(<span class="st">&quot;1002&quot;</span>));</span>
        <span id="cb2-15"><a href="#cb2-15"></a>       <span class="kw">try</span> {</span>
        <span id="cb2-16"><a href="#cb2-16"></a>           ctx.<span class="fu">invokeOperation</span>(<span class="st">&quot;RDBMSSample&quot;</span>, <span class="st">&quot;incrementEmployeeSalary&quot;</span>, params);</span>
        <span id="cb2-17"><a href="#cb2-17"></a>       } <span class="kw">catch</span> (DataServiceFault e) {</span>
        <span id="cb2-18"><a href="#cb2-18"></a>         <span class="co">// handle exception</span></span>
        <span id="cb2-19"><a href="#cb2-19"></a>       }</span>
        <span id="cb2-20"><a href="#cb2-20"></a>    }</span>
        <span id="cb2-21"><a href="#cb2-21"></a>} </span></code></pre></div>
        </div>
        </div>

    </div>
    </div></li>
    </ul></li>
    </ol>
    </div></td>
    </tr>
    </tbody>
    </table>

    ![](attachments/119133308/119133310.png){width="600"}

3.  Click **Schedule** .

### Confirming the scheduled task functionality

In the [before you begin](#SchedulingTasks-before_you_begin) section you
noticed that Chris and John had the email address as
`         NULL        ` . Now, let's check the email addresses of Chris
and John by running the command given below:

``` java
    select * from Employees;
```

Example output: You see that their email addresses are updated to
`         no-reply@wso2.com        ` as you specified in the
[operation](#SchedulingTasks-Operation) above. The scheduled task takes
this operation and runs it to update the data.

``` java
    +----------------+-----------+----------+-------------------+--------+
    | EmployeeNumber | FirstName | LastName | Email             | Salary |
    +----------------+-----------+----------+-------------------+--------+
    |            100 | Chris     | Adam     | no-reply@wso2.com | 10000  |
    |            101 | Alex      | pat      | alex@wso2.com     | 10000  |
    |            102 | John      | Doe      | no-reply@wso2.com | 10000  |
    +----------------+-----------+----------+-------------------+--------+
```
