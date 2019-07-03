# Downloading and Installing Siddhi Extensions

The Siddhi extensions supported for WSO2 SP are shipped with WSO2 SP by
default. If you need to download and install a different version of an
extension, it can be downloaded from the [Siddhi Extensions
Store](https://store.wso2.com/store/assets/analyticsextension/list) . To
download and install an Siddhi extension, follow the sections below.

-   [Downloading Siddhi
    extensions](#DownloadingandInstallingSiddhiExtensions-DownloadingSiddhiextensions)
-   [Installing Siddhi
    extensions](#DownloadingandInstallingSiddhiExtensions-InstallingSiddhiextensions)

## Downloading Siddhi extensions

To download the Siddhii extensions, follow the steps below

1.  Open the [Siddhi Extensions
    page](https://store.wso2.com/store/assets/analyticsextension/list) .
    The available Siddhi extensions are displayed as follows.  
    ![](attachments/112391148/112391149.png){width="900"}
2.  Click on the required extension. In this example, let's click on the
    **IBM MQ** extension.  
    ![](attachments/112391148/112391154.png){height="250"} In the dialog
    box that appears, enter your e-mail address and click **Submit** .
    The extension JAR is downloaded to the default location in your
    machine (based on your settings).
3.  If you are not using the latest version of WSO2 SP/CEP/DAS, and you
    want to select the version of the extension that matches your
    current product version, expand **Version Support** in the left
    navigator for the selected extension.

        !!! tip
    
        Each extension has a separate **Version Support** navigator item for
        SP, CEP and DAS.
    

    ![](attachments/112391148/112391153.png)

4.  If you need to download an older version of an extension, follow the
    substeps below.
    1.  Once you have clicked on the required extension, click on the
        **Older Versions** tab. Then click on the link displayed within
        the tab.  
        ![](attachments/112391148/112391152.png){width="900"}  
        You are directed to the maven central page where all the
        available versions of the extension are listed.  
        ![](attachments/112391148/112391155.png){width="654"}
    2.  Click on the relavent version. It directs you to the download
        page. To download the bundle, click on it.  
        ![](attachments/112391148/112391150.png){width="715"
        height="250"}

## Installing Siddhi extensions

To install the Siddhi extension in your WSO2 SP pack, place the
extension JAR you downloaded in the `         <SP_HOME>/lib        `
directory.

  
