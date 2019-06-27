# Removing Personally Identifiable Information via the Forget-me Tool

In WSO2 SP, event streams specify the schema for events to be selected
into the SP event flow to be processed. This schema can include user IDs
and other PII (Personally Identifiable Information) that you want to
delete from log files and such. This can be done via the [Forget-me
Tool](_Forget-me_Tool_Overview_) .

-   [Step 1: Configure the config.json
    file](#RemovingPersonallyIdentifiableInformationviatheForget-meTool-Step1:Configuretheconfig.jsonfile)
-   [Step 2: Execute the Forget-me
    tool](#RemovingPersonallyIdentifiableInformationviatheForget-meTool-Step2:ExecutetheForget-metool)

## Step 1: Configure the config.json file

The
`         <SP_HOME>/wso2/tools/identity-anonymization-tool-x.x.x/conf/config.json file        `
specifies the locations from which persisted data need to be removed.

The `         log-file        ` processor is specified in the
configuration file of the Forget-Me tool as shown on the sample below in
order to remove data with PII from the logs. If you have configured logs
with PII to be saved in another location, you can add it to this list of
processors.

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

This extract shows the default configuration of WSO2 SP. SP only saves
PII in log files by default. Therefore, this configuration allows the
Forget-me tool to delete these logs that are saved in
`         <SP_HOME>/wso2/<PROFILE>/logs        ` directory.

## Step 2: Execute the Forget-me tool

To execute the Forget-me tool, issue the following command pointing to
the `         <SP_HOME>        ` directory.

`         forget-me -U <USERNAME> -d <CONF_DIR> -carbon <SP_HOME>        `

  
