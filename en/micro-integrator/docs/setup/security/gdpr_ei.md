# General Data Protection Regulation (GDPR) for WSO2 EI

WSO2 Enterprise Integrator (WSO2 EI) consists of four profiles (ESB,
Message Broker, Business Process Server, and Analytics) that can persist
a user's personally identifiable information (PII) in various sources,
namely log files and RDBMSs. However, organizations that use WSO2 EI
have a legal obligation to remove all instances of a user's PII from the
system if the relevant user requests. For example, consider a situation
where an employee resigns from the organization and, thereby, requests
the organization to remove all instances of one's PII from the
organization's system. You can fulfill this requirement by anonymizing
the user's PII in the system, or (in some cases) by completely removing
such PII from the system.

See the topics given below for instructions on how to remove PII from
each profile of WSO2 EI.

## What is GDPR?

The General Data Protection Regulation (GDPR) is a new legal framework
that was formalized by the European Union (EU) in 2016. It comes into
effect from 28, May 2018. GDPR requires any organization that processes
Personally Identifiable Information (PII) of individuals who live in
Europe, to be compliant with the regulations. Organizations that fail to
demonstrate GDPR compliance are subjected to financial penalties.

> **Do you want to learn more about GDPR?**

> If you are new to GDPR, we recommend that you take a look at our
tutorial series on ***Creating a Winning GDPR Strategy.***

-   Part 1 - [Introduction to
    GDPR](https://wso2.com/library/article/2017/12/introduction-to-gdpr/)

-   Part 2 - [7 Steps for GDPR
    Compliance](https://wso2.com/library/article/2017/12/7-steps-for-gdpr-compliance/)

-   Part 3 - [Identity and Access Management to the
    Rescue](https://wso2.com/library/article/2018/2/identity-and-access-management-to-the-rescue/)

-   Part 4 - [GDPR Compliant Consent
    Design](https://wso2.com/library/articles/2018/03/creating-a-winning-gdpr-strategypart-4-gdpr-compliant-consent-design/)

For more resources on GDPR, see the white papers, case studies, solution
briefs, webinars, and talks published on our [WSO2 GDPR
homepage](https://wso2.com/solutions/regulatory-compliance/gdpr/) . You
can also find the original GDPR legal text
[here](http://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32016R0679)
.

## How WSO2 EI persists a user's PII

The ESB profile can persist PII in various log files (carbon logs, audit logs, API logs, and service-specific logs) depending on the mediation logic defined. The ESB does not persist a user's PII in any RDBMS by default.

### Tools for removing PII in WSO2 EI

The following tools are shipped with WSO2 EI:

-   WSO2 EI is shipped with the **Forget-Me Tool** , which can anonymize
    a user's PII in log files and RDBMSs by replacing all occurrences of
    the deleted user with either a randomly generated UUID value or a
    specified pseudonym. This tool is stored in the
    `           <EI_HOME>/wso2/tools/forget-me/          ` directory of
    WSO2 EI. Find out about all the capabilities of the Forget-Me tool
    from
    [here](https://docs.wso2.com/display/ADMIN44x/Removing+References+to+Deleted+User+Identities+in+WSO2+Products)
    .  
    **Important!** In the case of log files, note that the Forget-Me
    Tool does not replace PII values in the actual log files. Instead,
    the tool will create a new set of log files with anonymized PII
    values. The organization can then remove the original log files.

     > If you want to use the Forget-Me tool to remove PII in multiple WSO2
        products at the same time, you can use the standalone version of the
        tool.  
        For information on how to build and run the Forget-Me tool in
        standalone mode, see [Removing References to Deleted User Identities
        in WSO2
        Products](https://docs.wso2.com/display/ADMIN44x/Removing+References+to+Deleted+User+Identities+in+WSO2+Products)
        in the WSO2 Administration Guide.


-   In addition to this tool, the BPS profile also contains a set of SQL
    scripts that can remove PII by completely removing any process
    instances associated with a particular user. This method is only
    applicable to BPEL and Human Task processes.

### Prerequisites for removing PII

As explained in [How WSO2 EI persists a user's
PII](#GeneralDataProtectionRegulation(GDPR)forWSO2EI-how_PII_is_stored)
, the ESB profile and the BPS profile will store user information in log
files. Note that we can only remove a deleted user's PII from archived
log files, and not the live log files that are connected to the system.

Therefore, **before you start removing PII** stored by the ESB profile
and the BPMN component, be sure that the relevant user has been inactive
in the system for a sufficient amount of time. This will ensure that all
of the user's PII contained in log files are successfully archived. You
can then follow the instructions given below to remove the user's PII
references from the archived log files.

### Removing PII from the ESB profile

**Before you begin** ,

-   Find out about [how the ESB profile stores a user's
    PII](#GeneralDataProtectionRegulation(GDPR)forWSO2EI-how_PII_is_stored)
    .
-   See the [prerequisites for removing PII from the
    ESB](#GeneralDataProtectionRegulation(GDPR)forWSO2EI-prerequisites)
    .
-   In the instructions given below, we will use a proxy service that
    logs PII to demonstrate how PII can be removed from the ESB profile.
    However, please note that it is **not recommended** to log PII from
    a proxy service.


### Anonymizing PII references

You can use the [Forget-Me
Tool](#GeneralDataProtectionRegulation(GDPR)forWSO2EI-Tools) to remove
references to personally identifiable information (PII) from logs in the
ESB profile. For example, consider a proxy service that logs the
username that is sent through a payload. A Log mediator can be used for
this as shown below.

``` java
    <log level="custom">
    <property expression="//Authentication/username" name="USER_NAME"/>
    </log>
```

The user name that is used when you invoke this query will be logged in
the following log files: wso2carbon.log file, audit.log file, warn.log,
and the [service-specific log
file](https://docs.wso2.com/display/EI6xx/Creating+a+Proxy+Service#CreatingaProxyService-Enablinglogsforservices)
that is enabled for the proxy service.

``` java
    [EI-Core]  INFO - LogMediator USER_NAME = Sam
```

Let's look at how to anonymize the username value in log files.

1.  Every log statement follows the same pattern where the "USER\_NAME"
    keyword is followed by an actual username (in this example it is
    "Sam"). The regex pattern of this log statement will be as shown
    below. The Forget-Me Tool will use the below regex pattern to
    anonymize the username.

    This pattern should be added to the
    `           ei-patterns.xml          ` file (stored in the
    `           <EI_HOME>/wso2/tools/forget-me/conf/log-config/          `
    directory).

    ``` java
            <pattern key="pattern3">
                   <detectPattern>(.)*(USER_NAME)(.)*${username}(.)*</detectPattern>
                   <replacePattern>${username}</replacePattern>
            </pattern>
    ```

2.  Update the `           config.json          ` file (stored in the
    `           <EI_HOME>/wso2/tools/forget-me/conf/          `
    directory) as shown below. This file contains references to all the
    log files (except any [service-specific log
    file](https://docs.wso2.com/display/EI6xx/Creating+a+Proxy+Service#CreatingaProxyService-Enablinglogsforservices)
    ) in the system that store the above user information. If you have
    enabled a service-specific log file, you need to add that file name
    (see the element descriptions given below).

    ``` java
            {
             "processors" : [
               "log-file"
             ],
             "directories": [
               {
                 "dir": "log-config",
                 "type": "log-file",
                 "processor" : "log-file",
                 "log-file-path" : "<EI_HOME>/repository/logs",
                 "log-file-name-regex" : "(audit.log|warn.log|wso2carbon.log)(.)*"
               }
             ]
            }
    ```

    The elements in the above configuration are explained below.

    -   **"processors"** : The processors listed for this element
        specifies whether the tool will on log files, RDBMSs, or
        analytics streams. In the case of the ESB profile, we only need
        to remove PII from log files, and therefore, the processor is
        set to "log-file".
    -   **"directories"** : This element lists the directories that
        correspond to the processors. In the case of the ESB profile, we
        need to specify the directories that store log files.
    -   **"log-file-path"** : This specifies the directory path to the
        log files. Note that all the relevant log files are stored in
        the `             <EI_HOME>/repository/logs/            `
        directory.

        > Be sure to replace the "log-file-path" value with the correct
                absolute path to the location where the log files are stored. If
                you are **on Windows** , be sure to use the forward slash ("/")
                instead of the back slash ("\\"). For example:
                `             C:/Users/Administrator/Desktop/wso2ei-6.2.0/repository/log            `
                .


    -   **"log-file-name-regex"** : This gives the list of log files
        (stored in the log-file-path) that will persist the user's PII.
        Note that the above log-file-name-regex includes the audit.log,
        warn.log, and wso2carbon.log files, **as well as** the archived
        files of the same logs. If you have enabled a [service-specific
        log
        file](https://docs.wso2.com/display/EI6xx/Creating+a+Proxy+Service#CreatingaProxyService-Enablinglogsforservices)
        , **be sure to add** the file name to this list.

3.  Open a command prompt and navigate to the
    `           <EI_HOME>/bin          ` directory.

4.  Execute the following command to anonymize the user information that
    was added to the ei-patterns.xml file.  

    -   On Linux:

        ``` java
                ./forgetme.sh -U Sam
        ```

    -   On Windows:

        ``` java
                    forgetme.bat -U
        ```

    This will result in the following:

    1.  Copies will be created of all the log files specified in the
        config.json file. The following is the format of the log copy :
        `             anon-<time_stamp>-<original_log_name>.log            `
        . For example ,
        `             anon-1520946791793-warn.log            ` .

    2.  The PII will be anonymized in the copies. The log files will
        display the user information as a pseudonym.

        ``` java
                    [EI-Core]  INFO - LogMediator USER_NAME = 86c3bfd9-f97c-4b08-9f15-772dcb0c1c
        ```

        > For the list of commands you can run using the Forget-Me tool, see
        this
        [link](https://docs.wso2.com/display/ADMIN44x/Removing+References+to+Deleted+User+Identities+in+WSO2+Products)
        .


### Deleting original (archived) log files

Note that the PII is not removed from the original log files. It is the
responsibility of the organization to remove the original log files that
contain the user's PII.
