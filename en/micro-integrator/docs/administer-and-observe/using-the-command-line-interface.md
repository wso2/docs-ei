# Micro Integrator CLI

The Micro Integrator CLI allows you to monitor the synapse artifacts (deployed in a specified Micro Integrator server) from the command line. The CLI is an alternative to the [management dashboard](../../administer-and-observe/working-with-monitoring-dashboard), which provides a graphical view of the deployed artifacts.

The dashboard as well as the CLI communicates with the management API of WSO2 Micro Integrator to function. Therefore, be sure to [enable the Management API](#enable-the-management-api) in the server before using the dashboard or the CLI.

## Enable the Management API

To use the CLI tool, you need to enable the management API when you
start your WSO2 Micro Integrator instance. Pass the following system property:

```bash
-DenableManagementApi
```

-   When you run the Micro Integrator on Docker, start your Docker
    container by passing the `enableManagementApi` system property:

    ```bash
    docker run -p 8290:8290 -p 9164:9164 -e JAVA_OPTS="-DenableManagementApi=true" <Docker_Image_Name>
    ```

    !!! Note
        -  The default address is **https://localhost** and the port is **9164**.
        -  You can change the default host and port of the dashboard by using the [remote](#commands) command.

-   When you run the Micro Integrator on a VM, use the following command
    to enable the `enableManagementApi` system property:

    ```bash
    sh micro-integrator.sh -DenableManagementApi
    ```

-   The Management API is enabled for the embedded Micro Integrator in WSO2 Integration Studio by default.

## Install and run the CLI

1.  To download the dashboard, go to [**WSO2 Micro Integrator** website](https://wso2.com/integration/micro-integrator/#) -> **Download** -> **Other Resources**, and click **CLI Tooling**.
2.  If you are using a UNIX-based operating system (Linux, Solaris, and Mac OS X), be sure to set the `MI_CLI_HOME/bin` folder path as the PATH:

    ```bash
    export PATH=/path/to/mi/cli/directory/bin:$PATH
    ```
3.  Execute the following command to start the CLI:

    ```bash
    ./mi
    ```
4.  The available commands are listed as follows:

    ```bash
    mi is a Command Line Tool for Management of WSO2 Micro Integrator

    Usage:
    mi [command]

    Available Commands:
    api              Manage deployed Apis
    compositeapp     Manage deployed composite apps
    connector        Manage connectors
    dataservice      Manage deployed data services
    endpoint         Manage deployed endpoints
    help             Help about any command
    inboundendpoint  Manage deployed inbound endpoints
    localentry       Manage local entries
    log-level        Manage log4j2 properties
    messageprocessor Manage message processors
    messagestore     Manage message stores
    proxyservice     Manage deployed proxy services
    remote           Add, remove, update, or select Micro Integrator
    sequence         Manage deployed seqeunces
    task             Manage deployed tasks
    template         Manage templates
    version          Version of the CLI

    Flags:
    -h, --help      help for mi
    -v, --verbose   Enable verbose mode

    Use "mi [command] --help" for more information about a command.
    ```

## Log in to the CLI

To login using the CLI, use the following command. This will ask for the username and password. The default username is "admin" and the default password is "admin". 

```bash
mi remote login
```

If you want to login using a one line command, use the following command:

!!! Note 
    If you are on **Windows**, you must always login with the following command.

```bash
mi remote login [username] [password]
```

To logout from the CLI, please use the following command: 

```bash
mi remote logout
```

## Using the CLI 

### Usage

```bash
mi [command]
```

### version

```bash
mi version 
```

### Global Flags

```bash
--verbose
    Enable verbose logs (Provides more information on execution)
--help, -h
    Display information and example usage of a command
```

### Commands

-   **remote**
    ```bash
    Usage:
        mi remote [command] [arguments]

    Available Commands:
        add [nick-name] [host] [port]        Add a Micro Integrator
        remove [nick-name]                   Remove a Micro Integrator
        update [nick-name] [host] [port]     Update a Micro Integrator
        select [nick-name]                   Select a Micro Integrator on which commands are executed
        show                                 Show available Micro Integrators
        login                                Login to use the Management API (will be prompted for username and password)
        login [username] [password]          Login (inline username and password)

    Examples:
        # To add a Micro Integrator
        mi remote add TestServer 192.168.1.15 9164

        # To remove a Micro Integrator
        mi remote remove TestServer

        # To update a Micro Integrator
        mi remote update TestServer 192.168.1.17 9164

        # To select a Micro Integrator
        mi remote select TestServer

        # To show available Micro Integrators
        mi remote show
        
        # login to the current (selected)  Micro Integrator instance
        mi remote login     # will be prompted for username and password
            
        # login (with inline username and password)
        mi remote login admin admin
    ```
 -  **log-level**
     ```bash
     Usage:
        mi log-level [command] [arguments]

     Available Commands:
        show [logger-name]                   Show information about a logger
        update [logger-name] [log-level]     Update the log level of a logger

     Examples:
        # Show information about a logger
        mi log-level show org-apache-coyote

        # Update the log level of a logger
        mi log-level update org-apache-coyote DEBUG
     ```
 -  **api**
    ```bash
    Usage:
        mi api [command] [argument]

    Available Commands:
        show [api-name]                      Get information about one or more Apis

    Examples:
        # To List all the apis
        mi api show

        # To get details about a specific api
        mi api show sampleApi
    ```
-   **compositeapp**
    ```bash
    Usage:
        mi compositeapp [command] [argument]

    Available Commands:
        show [app-name]                      Get information about one or more Composite apps

    Examples:
        # To List all the composite apps
        mi compositeapp show

        # To get details about a specific composite app
        mi compositeapp show sampleApp
    ```
-   **endpoint**
    ```bash
    Usage:
        mi endpoint [command] [argument]

    Available Commands:
        show [endpoint-name]                 Get information about one or more Endpoints

    Examples:
        # To List all the endpoints
        mi endpoint show

        # To get details about a specific endpoint
        mi endpoint show sampleEndpoint
    ```
-   **inboundendpoint**
    ```bash
    Usage:
        mi inboundendpoint [command] [argument]

    Available Commands:
        show [inboundendpoint-name]          Get information about one or more Inbounds

    Examples:
        # To List all the inbound endpoints
        mi inboundendpoint show

        # To get details about a specific inbound endpoint
        mi inboundendpoint show sampleEndpoint
    ```
-   **proxyservice**

    ```bash
    Usage:
        mi proxyservice [command] [argument]

    Available Commands:
        show [proxyservice-name]             Get information about one or more Proxies

    Examples:
        # To List all the proxy services
        mi proxyservice show

        # To get details about a specific proxy service
        mi proxyservice show sampleProxy
    ```
-   **sequence**
    ```bash
    Usage:
        mi sequence [command] [argument]

    Available Commands:
        show [sequence-name]                 Get information about one or more Sequences

    Examples:
        # To List all the sequences
        mi sequence show

        # To get details about a specific sequence
        mi sequence show sampleProxy
    ```
-   **task**
    ```bash
    Usage:
        mi task [command] [argument]

    Available Commands:
        show [task-name]                     Get information about one or more Tasks

    Examples:
        # To List all the tasks
        mi task show

        # To get details about a specific task
        mi task show sampleProxy
    ```
-   **dataservice**
    ```bash
    Usage:
        mi dataservice [command] [argument]

    Available Commands:
        show [data-service-name]             Get information about one or more Dataservices

    Examples:
        # To List all the dataservices
        mi dataservice show

        # To get details about a specific task
        mi dataservice show SampleDataService
    ```

-   **connectors**
    ```bash
     Usage:
         mi connector [command]

     Available Commands:
         show             Get information about the connectors

     Examples:
         # To List all the connectors
         mi connector show
    ```
-   **templates**   
    ```bash
     Usage:
         mi template [command] [template-type] [template-name]

     Available Commands:
         show  [template-type]                  Get information about the given template type
         show  [template-type] [template-name]  Get information about the specific template

     Examples:
         # To List all the templates
         mi template show

         # To List all the templates of given template type
         mi template show endpoint

         # To get details about a specific template
         mi template show endpoint sampleTemplate
    ```
-   **messageprocessor**
    ```bash
     Usage:
         mi messageprocessor [command] [messageprocessor-name]

     Available Commands:
         show  [messageprocessor-name]  Get information about one or more Message Processor

     Examples:
         # To List all the message processor
         mi messageprocessor show

         # To get details about a specific message processor
         mi messageprocessor show  sampleMessageProcessor
    ```
-   **messagestore**
    ```bash
     Usage:
         mi messagestore [command] [messagestore-name]

     Available Commands:
         show  [messagestore-name]  Get information about one or more Message Store

     Examples:
         # To List all the message store
         mi messagestore show

         # To get details about a specific message store
         mi messagestore show  sampleMessageStore
    ```
-   **localentry**
    ```bash
     Usage:
         mi localentry [command] [localentry-name]

     Available Commands:
         show  [localentry-name]  Get information about one or more Local Entries

     Examples:
         # To List all the local entries
         mi localentry show

         # To get details about a specific local entry
         mi localentry show  sampleLocalEntry
    ```
