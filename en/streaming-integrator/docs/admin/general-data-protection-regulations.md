# General Data Protection Regulations (GDPR) for Streaming Integrator

The General Data Protection Regulation (GDPR) is a new legal framework formalized by the European Union (EU) in 2016. 
This regulation is effective since 28, May 2018, and can affect any organization that processes Personally Identifiable 
Information (PII) of individuals who live in Europe. Organizations that fail to demonstrate GDPR compliance are 
subjected to financial penalties.

!!! info

    Do you want to learn more about GDPR?

    If you are new to GDPR, we recommend that you take a look at our article
    series on ***Creating a Winning GDPR Strategy.***

    - Part 1 - [Introduction to GDPR](https://wso2.com/library/article/2017/12/introduction-to-gdpr/)

    - Part 2 - [7 Steps for GDPR Compliance](https://wso2.com/library/article/2017/12/7-steps-for-gdpr-compliance/)

    - Part 3 - [Identity and Access Management to the Rescue](https://wso2.com/library/article/2018/2/identity-and-access-management-to-the-rescue/)

    - Part 4 - [GDPR Compliant Consent Design](https://wso2.com/library/articles/2018/03/creating-a-winning-gdpr-strategypart-4-gdpr-compliant-consent-design/)

    For more resources on GDPR, see the white papers, case studies, solution briefs, webinars, and talks published on 
    our [WSO2 GDPR homepage](https://wso2.com/solutions/regulatory-compliance/gdpr/). You can also find the original 
    GDPR legal text [here](http://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32016R0679).
    
## Removing personally identifiable information via the Forget-me tool

In the Streaming Integrator, streams specify the schema for events to be selected into the streaming integration event 
flow to be processed. This schema can include user IDs and other PII (Personally Identifiable Information) that you want
 to delete from log files and such. This can be done via the [Forget-me Tool](_Forget-me_Tool_Overview_).


**Step 1: Configure the config.json file**

The `<SI_HOME>/wso2/tools/identity-anonymization-tool-x.x.x/conf/config.json` file specifies the locations from which 
persisted data need to be removed.

The `log-file` processor is specified in the configuration file of the Forget-Me tool as shown on the sample below in
order to remove data with PII from the logs. If you have configured logs with PII to be saved in another location, you 
can add it to this list of processors.

``` js
{
  "processors" : [
    "log-file"
  ],
  "directories": [
    {
      "dir": "log-config",
      "type": "log-file",
      "processor" : "log-file",
      "log-file-path" : "logs",
      "log-file-name-regex" : "(.)*"
    }
  ]
}
```

This extract shows the default configuration of the Streaming Integrator. The Streaming Integrator only saves
PII in log files by default. Therefore, this configuration allows the
Forget-me tool to delete these logs that are saved in the `<SI_HOME>/wso2/server/logs` directory.

**Step 2: Execute the Forget-me tool**

To execute the Forget-me tool, issue the following command pointing to
the `         <SP_HOME>        ` directory.

`         forget-me -U <USERNAME> -d <CONF_DIR> -carbon <SP_HOME>        `

  


