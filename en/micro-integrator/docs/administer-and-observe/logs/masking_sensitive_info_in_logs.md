# Masking Sensitive Information in Logs

!!! Info
        - The following feature is released as a product update on <b>11th of November, 2020</b>. If you don't already have this update, you can [get the latest updates](https://updates.docs.wso2.com/en/latest/updates/overview/) now.
        - Log masking can impact server performance since each log line is matched to each of the specified patterns. Therefore, it is highly recommended to use only the most necessary patterns.

There can be business sensitive information that are added to logs. When these logs are analyzed, the information is exposed.

To avoid this potential security pitfall, you can mask sensitive information (such as credit card numbers, access tokens, etc.) in the log file when the logs are created. You can also define patterns that need to be masked from the logs.

Log masking in not enabled by default in the Micro Integrator. Therefore, you need to manaully enable it and configure the required masking patterns.

## Enabling log masking

1.  Open the `log4j2.properties` file from the `<MI-HOME>/conf/` folder. and do the necessary changes. 
2.  Add an extra `m` to the log file layout configuration (`layout.pattern` parameter) in which you want the values to be masked as shown below.

    ```
    appender.CARBON_CONSOLE.layout.pattern = [%d] %5p {%c{1}} - %mm%ex%n
    ```
3. Update the [masking patterns](#the-masking-pattern-file) from the `<MI-HOME>/conf/deployment.toml` file as explained below.

## The masking patterns

You can specify one or more masking patterns in the `<MI-HOME>/conf/deployment.toml` file. The following is a sample configuration that showcases how to mask the credit card numbers from the logs.

```toml
[masking_pattern.properties]
"CREDIT_CARD_VISA" = "4[0-9]{6,}$"
"CREDIT_CARD_MASTER" = "(?:5[1-5][0-9]{2}|222[1-9]|22[3-9][0-9]|2[3-6][0-9]{2}|27[01][0-9]|2720)[0-9]{12}"
"CREDIT_CARD_AMEX" = "[34|37][0-9]{14}$"
```

With this configuration, each log line is checked for all the configured patterns. If any match is found, it is masked with ‘*****’.

!!! danger "Using single quotes in TOML configs to avoid parsing escape characters"
    If the strings defined in the `deployment.toml` file are within double quotes, it is parsed along with the escape characters. To avoid this, use single quotes when you need to add escape characters as shown in the example below.
    ```
    [masking_pattern.properties]
    "ACCT_ID" = '(?<=accountId\':)(.*)(?=\')' 
    "ACCT_ID.replace_pattern"='(.?).(?=.*)'
    "ACCT_ID.replacer"="*"
    ```

!!! note
    The following feature is released as a product update on <b>08th of December, 2020</b>. If you don't already have this update, you can [get the latest updates](https://updates.docs.wso2.com/en/latest/updates/overview/) now.

This feature introduces the `replace_pattern` and `replace` properties to the configurations, allowing you to easily match the key-value pairs for the log masking feature. This can be used to mask only the relevant value for the identified key. Follow the steps below to make changes in the `log4j2.properties` file from the `<MI-HOME>/conf/` folder.

1. Replace `appender.CARBON_LOGFILE.layout.pattern = [%d] %5p {%c} - %m%ex%n` with the following.
    ```
    appender.CARBON_LOGFILE.layout.pattern = [%d] %5p {%c} - %mm%ex%n
    ```

2. Replace `appender.AUDIT_LOGFILE.layout.pattern = [%d] %5p {%c} - %m%ex%n` with
    ```
    appender.AUDIT_LOGFILE.layout.pattern = [%d] %5p {%c} - %mm%ex%n
    ```
3. Add the following entries to the `<MI_HOME>/conf/deployment.toml` file.

    ```
    [masking_pattern.properties]
    "ACCT_ID" = "(?<=accountId\":)(.*)(?=\")" 
    "ACCT_ID.replace_pattern"="(.?).(?=.*\")"
    "ACCT_ID.replacer"="*"
    ```