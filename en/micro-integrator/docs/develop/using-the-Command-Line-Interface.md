# Using the Command-Line Interface

WSO2 Micro Integrator is shipped with a command-line interface (CLI)
tool, which can be used as the management console.

-   [Prerequisites](#UsingtheCommand-LineInterface-Prerequisites)
-   [Step 1: Install and run the
    CLI](#UsingtheCommand-LineInterface-Step1:InstallandruntheCLI)
-   [Step 2: Configuring the CLI
    (Optional)](#UsingtheCommand-LineInterface-Step2:ConfiguringtheCLI(Optional))
-   [Step 3: Using the CLI to get artifact
    details](#UsingtheCommand-LineInterface-Step3:UsingtheCLItogetartifactdetails)

### Prerequisites

To use the CLI tool, you need to enable the management API when you
start the WSO2 Micro Integrator instance. This can be done by passing
the `         -DenableManagementApi        ` system property when you
start the Micro Integrator. Note that the default address is
[https://localhost](https://localhost/) and the port is **9164** .

-   When you run the Micro Integrator on Docker, start your Docker
    container by passing the '
    `           enableManagementApi          ` ' system property:

    ``` java
        docker run -p 8290:8290 -p 9164:9164 -e JAVA_OPTS="-DenableManagementApi=true" <Docker_Image_Name>
    ```

-   When you run the Micro Integrator on a VM, use the following command
    to enable the ' `           enableManagementApi          ` ' system
    property:

    ``` java
            sh micro-integrator.sh -DenableManagementApi
    ```

### Step 1: Install and run the CLI

1.  To download the CLI, go the **WSO2 Micro Integrator** website →
    [Other Installation
    Options](https://wso2.com/integration/micro-integrator/install/) ,
    click **CLI Tooling** , and download the tool.
2.  Go to the `           <CLI_HOME>          ` directory, and execute
    the following command:

    ``` java
            ./mi
    ```

3.  The available commands are listed as follows:

    ``` java
            mi
            mi is a Command Line Tool for Management of WSO2 Micro Integrator
        
            Usage:
              mi [command]
        
            Available Commands:
              completion  Generates bash completion scripts
              help        Help about any command
              init        Set Management API configuration
              show        List or show details about carbon app, endpoint, api, inbound endpoint, proxy service, task or sequence
              version     Version of the CLI
        
            Flags:
              -h, --help      help for mi
              -v, --verbose   Enable verbose mode
        
            Use "mi [command] --help" for more information about a command.
    ```

**Note** that you can append the location of the executable (mi) to your
$PATH variable to start the tool from any location.

### Step 2: Configuring the CLI (Optional)

To configure the address and the port of the management API in the CLI,
use the 'init' command. This will generate a file called
`         server_config.yaml        ` , which contains the address and
the port. If the init command was not used, the address and the port
will have the default values.

1.  Execute the init command, and specify the hostname and port number:

    **Example**

    ``` java
            mi init
            Enter following parameters to configure the cli
            Host name(default localhost): abc.com
            Port number(default 9164): 9595
            CLI configuration is successful
    ```

2.  The CLI is now configured. Execute the ' `           mi          ` '
    command, and see the available commands:

    ``` java
            mi
            mi is a Command Line Tool for Management of WSO2 Micro Integrator
        
            Usage:
              mi [command]
        
            Available Commands:
              completion  Generates bash completion scripts
              help        Help about any command
              init        Set Management API configuration
              show        List or show details about carbon app, endpoint, api, inbound endpoint, proxy service, task or sequence
              version     Version of the CLI
        
            Flags:
              -h, --help      help for mi
              -v, --verbose   Enable verbose mode
        
            Use "mi [command] --help" for more information about a command.
    ```

### Step 3: Using the CLI to get artifact details

Execute 'mi show' with the required commands, arguments, and flags to
get details.

**Usage**

``` java
    // Used for displaying details of specific artifacts deployed in the Micro Integrator.
    mi show [command] [argument] [flag]
```

**Examples**

``` java
    //To list all the apis
    mi show api
    
    //To list all proxy services
    mi show proxyservice
    
    //To get details about a specific api
    mi show api <api_name>
    //To get details about a specific proxy service
    mi show proxyService <proxy_service_name>
    
    //To get details about a carbon application
    mi show carbonapp <carbon_app_name>
    
    //To get details about an endpoint
    mi show endpoint <endpoint_name>
    
    //To get details about an inbound endpoint
    mi show inboundendpoint <inbound_endpoint_name>
    
    //To get details about a sequence
    mi show sequence <sequence_name>
    
    //To get details about a task
    mi show task <task_name>
```
