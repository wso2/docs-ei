# Downloading and Installing Siddhi Extensions

The Siddhi extensions supported for the Streaming Integrator are shipped with the product by
default. If you need to download and install a different version of an
extension, it can be downloaded from the [Siddhi Extensions
Store](https://store.wso2.com/store/assets/analyticsextension/list). To
download and install an Siddhi extension, follow the sections below.


## Downloading Siddhi extensions

To download the Siddhii extensions, follow the steps below

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

## Installing Siddhi extensions

To install the Siddhi extension in your Streaming Integrator pack, place the extension JAR you downloaded in the 
`<SI_HOME>/lib` directory.

  
