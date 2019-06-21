# Transaction Mediator Example

The [Transaction mediator](_Transaction_Mediator_) supports [distributed
transactions](https://docs.wso2.com/display/EI650/Working+with+Transactions)
using the Java transaction API (JTA). When it comes to distributed
transactions, it involves accessing and updating data on two or more
networked computers, often using multiple databases. For example, two
databases or a database and a message queue such as JMS. You can use the
Synapse configuration language to define when to start, commit, or roll
back the transaction. For example, you can mark the start of a
transaction at the start of a database commit, mark the end of the
transaction at the end of database commit and roll back the transaction
if an error occurs.

Let's explore a basic transaction mediator scenario that demonstrates
how the transaction mediator can be used to manage complex distributed
transactions.

------------------------------------------------------------------------

[Transaction mediator
scenario](#TransactionMediatorExample-Transactionmediatorscenario) \|
[Prerequisites](#TransactionMediatorExample-Prerequisites) \|
[Configuring the example
scenario](#TransactionMediatorExample-Configuringtheexamplescenario) \|
[Testing the example
scenario](#TransactionMediatorExample-Testingtheexamplescenario)

#### Transaction mediator scenario

You have a record in one database and you want to delete that record
from the first database and add it to a second database.  Assume that
these two databases can either be run on the same server or can be in
two remote servers. The database tables are defined in a way that the
same entry cannot be added twice. Therefore, in the successful scenario,
the record will be deleted from the first database and will be added to
the second database. In the failure scenario, the record is already in
the second database, hence the record will not be deleted from the first
table nor will it be added into the second database.

------------------------------------------------------------------------

#### Prerequisites

-   Windows, Linux or Solaris operating systems with WSO2 EI installed.
-   Apache Derby database server. If you do not have the Apache Derby
    database set up, d ownload the Apache Derby distribution from
    [http://db.apache.org](http://db.apache.org/derby/derby_downloads.html)
    .

------------------------------------------------------------------------

#### Configuring the example scenario

1.  Copy and paste the following configuration into \<
    `           EI_HOME>/repository/deployment/server/synapse-configs/<node>/synapse.xml          `
    .  
    The sample configuration used here is similar to [sample
    361](https://docs.wso2.com/display/EI600/Sample+361%3A+Introduction+to+DBReport+Mediator)
    . Here via the *In* sequence a message is sent to the service, and
    via the *Out* sequence an entry from the first database is deleted
    and the second database is updated with that entry. If an entry that
    already exists is added once again to the second database, the
    entire transaction will roll back.

    ``` java
        <definitions xmlns="http://ws.apache.org/ns/synapse">
           <sequence name="myFaultHandler">
                <log level="custom">
                    <property name="text" value="** Rollback Transaction**"/>
                </log>
                <transaction action="rollback"/>
                <send/>
            </sequence>
            <sequence name="main" onError="myFaultHandler">
                <in>
                    <send>
                        <endpoint>
                            <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                        </endpoint>
                    </send>
                </in>
                 <out>
                    <transaction action="new"/>
                    <log level="custom">
                        <property name="text" value="** Reporting to the Database EIdb**"/>
                    </log>
                    <dbreport useTransaction="true" xmlns="http://ws.apache.org/ns/synapse">
                        <connection>
                            <pool>
                                <dsName>java:jdbc/XADerbyDS</dsName>
                                <icClass>org.jnp.interfaces.NamingContextFactory</icClass>
                                <url>localhost:1099</url>
                                <user>EI</user>
                                <password>EI</password>
                            </pool>
                        </connection>
                        <statement>
                             <sql>delete from company where name =?</sql>
                             <parameter expression="//m0:return/m1:symbol/child::text()"
                               xmlns:m0="http://services.samples" xmlns:m1="http://services.samples/xsd"
                                         type="VARCHAR"/>
                        </statement>
                    </dbreport>
                    <log level="custom">
                        <property name="text" value="** Reporting to the Database EIdb1**"/>
                    </log>
                    <dbreport useTransaction="true" xmlns="http://ws.apache.org/ns/synapse">
                        <connection>
                            <pool>
                                <dsName>java:jdbc/XADerbyDS1</dsName>
                                <icClass>org.jnp.interfaces.NamingContextFactory</icClass>
                                <url>localhost:1099</url>
                                <user>EI</user>
                                <password>EI</password>
                            </pool>
                        </connection>
                        <statement>
                            <sql>INSERT into company values (?,'c4',?)</sql>
                            <parameter expression="//m0:return/m1:symbol/child::text()"
                 xmlns:m1="http://services.samples/xsd" xmlns:m0="http://services.samples"
                                       type="VARCHAR"/>
                            <parameter expression="//m0:return/m1:last/child::text()"
                 xmlns:m1="http://services.samples/xsd" xmlns:m0="http://services.samples"
                                       type="DOUBLE"/>
                        </statement>
                    </dbreport>
                    <transaction action="commit"/>
                    <send/>
                </out>
            </sequence>
        </definitions>
    ```

2.  Setup two distributed Derby databases *EIdb* and *EIdb1.* For
    instructions on setting up the Derby databases,  see [Setting up the
    Derby database
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-SetupDerby)
    .

3.  Create a table in *EIdb* by executing the following statement.

    ``` java
            CREATE table company(name varchar(10) primary key, id varchar(10), price double);
    ```

4.  Create a table in *EIdb1* by executing the following statement.

    ``` java
            CREATE table company(name varchar(10) primary key, id varchar(10), price double);
    ```

5.  Insert records into the two tables that you created by executing
    the following statements.

    **To insert records into the table in EIdb**

    ``` java
            INSERT into company values ('IBM','c1',0.0);
            INSERT into company values ('SUN','c2',0.0);
    ```

    **To insert records into the table in EIdb1**

    ``` java
            INSERT into company values ('SUN','c2',0.0);
            INSERT into company values ('MSFT','c3',0.0);
    ```

        !!! info
    
        Note
    
        When inserting records into the tables, the order of the record
        matters.
    

6.  Create the following data source declarations for the distributed
    databases in the
    `           <EI_HOME>/conf/datasources/           master-datasources.xml          `
    file.

    ``` java
        <datasources>
            <xa-datasource>
                <jndi-name>jdbc/XADerbyDS</jndi-name>
                <isSameRM-override-value>false</isSameRM-override-value>
                <xa-datasource-class>org.apache.derby.jdbc.ClientXADataSource</xa-datasource-class>
                <xa-datasource-property name="portNumber">1527</xa-datasource-property>
                <xa-datasource-property name="DatabaseName">EIdb</xa-datasource-property>
                <xa-datasource-property name="User">EI</xa-datasource-property>
                <xa-datasource-property name="Password">EI</xa-datasource-property>
                <metadata>
                    <type-mapping>Derby</type-mapping>
                </metadata>
            </xa-datasource>
        </datasources>
    ```

      

    ``` java
            <datasources>
                <xa-datasource>
                    <jndi-name>jdbc/XADerbyDS1</jndi-name>
                    <isSameRM-override-value>false</isSameRM-override-value>
                    <xa-datasource-class>org.apache.derby.jdbc.ClientXADataSource</xa-datasource-class>
                    <xa-datasource-property name="portNumber">1527</xa-datasource-property>
                    <xa-datasource-property name="DatabaseName">EIdb1</xa-datasource-property>
                    <xa-datasource-property name="User">EI</xa-datasource-property>
                    <xa-datasource-property name="Password">EI</xa-datasource-property>
                    <metadata>
                        <type-mapping>Derby</type-mapping>
                    </metadata>
                </xa-datasource>
            </datasources>
    ```

7.  Deploy the back-end service
    `          SimpleStockQuoteService         ` . For instructions on
    deploying sample back-end services, see [Deploying sample back-end
    services](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Backend)
    .
8.  Start the Axis2 server. For instructions on starting the Axis2
    server, see [Starting the Axis2
    server](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples#SettingUptheESBSamples-Axis2server)
    .

#### Testing the example scenario

**To test the successful scenario**

-   Execute the following command from the
    `           <EI_HOME>/samples/axis2Client          ` directory.

    ``` java
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService
            -Dtrpurl=http://localhost:8280/ -Dsymbol=IBM
    ```

    Executing this command removes the IBM record from the first
    database and adds it to the second database.

    When you check both databases you will see that the IBM record is
    deleted from the first database and added to the second database.

**To test the** f **ailure scenario**

-   Execute the following command from the
    `           <EI_HOME>/samples/axis2Client          ` directory.

    ``` java
            ant stockquote -Daddurl=http://localhost:9000/services/SimpleStockQuoteService
            -Dtrpurl=http://localhost:8280/ -Dsymbol=SUN
    ```

    Executing this command attempts to add an already existing record
    again to the second database, which results in the fault sequence
    being executed. You will see an exception raised for duplicate
    entries and the entire transaction will roll back.

    When you check both databases you will see that a record is neither
    deleted from the first database nor added into the second database.
