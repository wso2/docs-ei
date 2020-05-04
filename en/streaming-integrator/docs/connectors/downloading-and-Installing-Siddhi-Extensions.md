# Downloading and Installing Siddhi Extensions

The Siddhi extensions supported for the Streaming Integrator are shipped with the product by
default. If you need to download and install a different version of an
extension, you can download it manually or via the command line as covered in the following sections.

## Downloading and installing Siddhi extensions manually

### Downloading Siddhi extensions

To download Siddhi extensions manually from the store and install them, follow the steps below.

To download the Siddhi extensions, follow the steps below

1. Open the [Siddhi Extensions page](https://store.wso2.com/store/assets/analyticsextension/list).
   The available Siddhi extensions are displayed as follows.  
   ![Siddhi Extension Home Page](../images/downloading-and-installing-siddhi-extensions/Siddhi_Extensions.png)

2. Click on the required extension. In this example, let's click on the **IBM MQ** extension.  
   ![Download Extension](../images/downloading-and-installing-siddhi-extensions/Download_Extension.png)

   In the dialog box that appears, enter your e-mail address and click **Submit**. The extension JAR is downloaded to 
   the default location in your machine (based on your settings).

3.  If you are not using the latest version of the Streaming Integrator, and you
    want to select the version of the extension that matches your current product version, expand **Version Support** 
    in the left navigator for the selected extension.

    !!! tip 
        Each extension has a separate **Version Support** navigator item for the Streaming Integrator, SP, CEP and DAS.
    ![Version Support](../images/downloading-and-installing-siddhi-extensions/Extension_Left_Navigator.png)

4. If you need to download an older version of an extension, follow the substeps below.
   1. Once you have clicked on the required extension, click on the **Older Versions** tab. Then click on the link 
   displayed within the tab.  
    ![Older Versions](../images/downloading-and-installing-siddhi-extensions/Extensions_Older_Versions.png)

    You are directed to the maven central page where all the available versions of the extension are listed.  
    ![All available versions](../images/downloading-and-installing-siddhi-extensions/Central Maven Repository.png)

   2. Click on the relavent version. It directs you to the download page. To download the bundle, click on it.  
    ![Download Bundle](../images/downloading-and-installing-siddhi-extensions/Maven_Bundle.png)

### Installing Siddhi extensions

To install the Siddhi extension in your Streaming Integrator pack, place the extension JAR you downloaded in the 
`<SI_HOME>/lib` directory.

### Uninstalling Siddhi extensions

To uninstall a Siddhi extension, delete the relevant extension JAR in the `<SI_HOME>/lib` directory.

## Downloading and installing Siddhi extensions via the command line

To manage Siddhi extensions via the command line, see the following topics.

### Identifying the Siddhi extensions to install/uninstall

The following are some actions that you are required to perform in order to determine which Siddhi extensions you need to install. Navigate to the `<SI_HOME>/wso2/tools/extension-installer` in order to issue these commands.

- **Viewing the list of extensions that are currently installed**

    You can view the complete list of Siddhi extensions that are currently installed in your Streaming Integrator setup. All the extensions listed are completely installed with the dependencies.
    
    To perform this action, issue the appropriate command out of the following based on your operating system:
    
    - **For Windows**     : `extension-installer.bat list`
    - **For Linux/MacOS** : `./extension-installer.sh list`
    
- **Viewing the installation status of all the supported Siddhi extensions**

    You can view the complete list of Siddhi extensions supported for WSO2 Streaming Integrator together with the current installation status for each extension.
    
    The installation status can be one of the following:
    
    |**Installation Status**|**Description**                                                    |
    |-----------------------|-------------------------------------------------------------------|
    |**Installed**          |This indicates that the extension is completely installed. The installation includes the JAR of the extension itself as well as all its dependencies (if any).|
    |**Not-Installed**      |This indicates that the extension has not been installed. The JAR of the extension itself has not been installed. Dependencies (if any) may be already installed due to shared dependencies.|
    |**Partially-Installed**|This indicates that the JAR of the extension itself has been installed, but one or more dependencies of the extension still need to be installed.<br/><br/> If an extension has this status, you can view more information about the dependencies to be installed by checking the installation status of that specific extension individually.|
    |**Restart-Required**   |This indicates that you need to restart Streaming Integrator Tooling in order to complete the installation/un-installation of the extension.|

    To perform this action, issue the appropriate command out of the following based on your operating system:
    
    - **For Windows**     : `extension-installer.bat list --all`
    - **For Linux/MacOS** : `./extension-installer.sh list --all`
    
- **Checking the installation status of a specific Siddhi extension**

    You can view the installation status of a specific extension individually together with details of dependencies that need to be manually downloaded (if any exist).

    To perform this action, issue the appropriate command out of the following based on your operating system:
    
    - **For Windows**     : `extension-installer.bat list <EXTENSION_NAME>`
    - **For Linux/MacOS** : `./extension-installer.sh list <EXTENSION_NAME>`   

### Installing Siddhi Extensions

If the Siddhi applications deployed in your WSO2 Streaming Integrator setup use Siddhi extensions that are not currently installed, you can install all those extensions at once. To do this, issue the appropriate command out of the following based on your operating system.

- **For Windows**     : `extension-installer.bat install`
- **For Linux/MacOS** : `./extension-installer.sh install` 

If you want to install a specific Siddhi extension, issue the appropriate command out of the following based on your operating system.

- **For Windows**     : `extension-installer.bat install <EXTENSION_NAME>`
- **For Linux/MacOS** : `./extension-installer.sh install <EXTENSION_NAME>` 

### Uninstalling Siddhi Extensions

To uninstall a specific Siddhi application, issue the appropriate command out of the following based on your operating system.

- **For Windows**     : `extension-installer.bat uninstall <EXTENSION_NAME>`
- **For Linux/MacOS** : `./extension-installer.sh uninstall <EXTENSION_NAME>` 

  
