# Configuring Log4j Properties

All WSO2 products are shipped with the log4j logging capabilities , which generates administrative activities and server side logs. The log4j.properties file, which governs how logging is performed by the server can be found in the MI_HOME/conf directory.

There are three main components when configuring log4j: **Loggers**, **Appenders**, and **Layouts**. First, the server stores new values in the database and then changes the appropriate components in the logging framework, enabling the logging properties to be updated immediately.

### Setting the log level

The log level can be set specifically for each appender in the `         log4j.properties        ` file by setting the threshold value. If a log level is not specifically given for an appender as explained below, the root log level (INFO) will apply to all appenders by default.

For example, shown below is how the log level is set to DEBUG for the `CARBON_LOGFILE` appender [Carbon
log](#MonitoringLogs-Carbon_logs) ):

``` java
log4j.appender.CARBON_LOGFILE.threshold=DEBUG
```

Listed below are the log levels that can be configured:

| Level | Description                                                                                                                                                                                                                                                                     |
|-------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| OFF   | The highest possible log level. This is intended for disabling logging.                                                                                                                                                                                                         |
| FATAL | Indicates server errors that cause premature termination. These logs are expected to be immediately visible on the command line that you used for starting the server.                                                                                                          |
| ERROR | Indicates other runtime errors or unexpected conditions. These logs are expected to be immediately visible on the command line that you used for starting the server.                                                                                                           |
| WARN  | Indicates the use of deprecated APIs, poor use of API, possible errors, and other runtime situations that are undesirable or unexpected but not necessarily wrong. These logs are expected to be immediately visible on the command line that you used for starting the server. |
| INFO  | Indicates important runtime events, such as server startup/shutdown. These logs are expected to be immediately visible on the command line that you used for starting the server . It is recommended to keep these logs to a minimum.                                           |
| DEBUG | Provides detailed information on the flow through the system. This information is expected to be written to logs only. Generally, most lines logged by your application should be written as DEBUG logs.                                                                        |
| TRACE | Provides additional details on the behavior of events and services. This information is expected to be written to logs only.                                                                                                                                                    |

### Setting the log pattern

The log pattern defines the output format of the log file. This is the layout pattern that describes the log message format.

**Identifying forged messages**: 
The conversion character 'K' can be used in the pattern layout to log a UUID. For example, the log pattern can be \[%K\] \[%T\] \[%S\] \[%d\] %P%5p {%c} - %x %m {%c}%n, where \[%K\] is the UUID. 

The UUID can be used for identifying forged messages in the log. By default, the UUID will be generated every time the server starts. If required, you can configure the UUID regeneration period by manually adding the following property to the `log4j.properties` file (stored in the `MI_HOME/conf` directory):

``` java
log4j.appender.CARBON_LOGFILE.layout.LogUUIDUpdateInterval=<number_of_hours>
```

### Setting the Sys Log Host

The IP address of the system log server. The syslog server is a dedicated log server for many applications. It runs in a particular TCP port in a separate machine, which can be identified by an IP address.

### Setting the Facility

The log message type sent to the system log server.

### Setting the Threshold

Filters log entries based on their level. For example, threshold set to 'WARN' will allow the log entry to pass into appender. If its level is 'WARN', 'ERROR' or 'FATAL', other entries will be discarded. This is the minimum log level at which you can log a message. See descriptions of the available log levels .

### Configuring Log4j Appenders

This section allows you to configure appenders individually. Log4j allows logging requests to print to multiple destinations. These output destinations are called 'Appenders'. You can attach several appenders to one logger.

-   **CARBON_CONSOLE**: Logs to the console when the server is running.
-   **CARBON_LOGFILE**: Writes the logs to MI_HOME/repository/logs/wso2carbon.log .
-   **SERVICE_APPENDER**: Writes service invocations to `MI_HOME/repository/logswso2-MI-service.log`.             
-   **ERROR_LOGFILE**: Writes warning/error messages to the `MI_HOME/repository/logs/wso2-MI-service.log`.
-   **TRACE _ APPENDER**: Writes tracing/debug messages to the `MI_HOME/repository/logs/wso2-MI-trace.log` for tracing enabled services.
-   **CARBON _ MEMORY**:
-   **CARBON_SYS_LOG**: Allows separating the software that generates messages, from the system that stores them and the software that reports and analyzes them.
-   **CARBON_TRACE_LOGFILE**:

### Configuring Log4j Loggers

A Logger is an object used to log messages for a specific system or application component. Loggers are normally named using a hierarchical dot-separated namespace and have a 'child-parent' relationship. For example, the logger named 'root.sv' is a parent of the logger named 'root.sv.sf' and a child of 'root'.

When the server starts for the first time, all the loggers initially listed in the log4j.properties file appear on the logger name list. This section allows you to browse through all these loggers, define a log level and switch on/off additivity to any of them. After editing, the logging properties are read only from the database.

-   **Logger**: The name of a logger.
-   **Parent Logger**: The name of a parent logger.
-   ****Level: Allows to select level (threshold) from the drop-down menu. After you specify the level for a certain logger, a log request for that logger will only be enabled if its level is equal or higher to the logger’s level. If a given logger is not assigned a level, then it inherits one from its closest ancestor with an assigned level. Refer to the hierarchy of levels given above. See descriptions of the available log levels .
-   **Additivity**: Allows to inherit all the appenders of the parent Logger if set as 'True'.