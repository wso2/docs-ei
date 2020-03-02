# Installing Siddhi Extensions

Streaming Integrator Tooling allows you to install or un-install Siddhi extensions through the Extensions Installer.

To install or un-install Siddhi extensions, follow the steps given below.

1. Start the Streaming Integrator Tooling by issuing one of the following commands from the `<SI_HOME>/bin` directory.

    - For Windows: `tooling.bat`

    - For Linux: `./tooling.sh`

    The Streaming Integrator Tooling opens as shown below.

    ![Streaming Integrator Welcome Page](../images/exporting-Siddhi-Applications/SI-Welcome_Page.png)

2. Click **Tools** menu option and then click **Extension Installer**. 
    
    <!-- // TODO insert tools menu image -->

    As a result, the following dialog box opens.

    <!-- // TODO insert dialog -->

3. Locate the extension you wish to install/un-install. Each extension's name, installation status, and the action button will be shown.

    !!! tip
        If the extension you are searching for is not shown within the first set of extensions that are listed down, enter the name of the extension in the search bar, which will filter the extension(s) matching the entered keyword.
        
    <!-- // TODO insert search keyword-filtered extension -->
    
    Following are the possible values for an extension's status.
    <table>
        <tr>
            <th>Status</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>**Installed**</td>
            <td>The extension has been completely installed.</td>
        </tr>
        <tr>
            <td>**Not-Installed**</td>
            <td>The extension has not been installed.</td>
        </tr>
        <tr>
            <td>**Partially Installed**</td>
            <td>
                At least one of the dependencies of the extension has to be installed. This can be due to a failure in installing dependencies, or there is a [dependency that should be manually installed](#manually-installable-dependencies).
            </td>
        </tr>
    </table>

4. The editor should be restarted after installing or un-installing an extension.

####Manually Installable Dependencies
Certain dependencies of some extensions can not be automatically downloaded through the Siddhi Extensions Installer. These dependencies should be manually downloaded and installed. When there is at least one such dependency for an extension, an icon will be shown as follows, next to the extension's status.
    
<!-- // TODO insert Extension row with (i) image -->

Clicking this icon will open the following dialog box.
    
<!-- // TODO insert manually installable instructions dialog image -->

The dialog box will show each dependency of the particular extension that should be manually installed, along with the following information.

- **Instructions** - Instructions to follow, in order to download the `*.jar` file for the dependency.
- **Installation Locations** - The downloaded `*.jar` file should be placed in its specific location, as given below.

    <table>
        <tr>
            <th>Location</th>
            <th>Directory</th>
        </tr>
        <tr>
            <td>runtime</td>
            <td>
                <code>&lt;SI_HOME&gt;/lib</code>
            </td>
        </tr>
        <tr>
            <td>samples</td>
            <td>
                <code>&lt;SI_HOME&gt;/samples/sample-clients/lib</code>
            </td>
        </tr>
    </table>

    !!! Info
        When the type is _bundle_, and the downloaded `*.jar` file is not an OSGi bundle, the jar should be converted to an OSGi bundle. For this, navigate to the `<SI_HOME>/bin` directory, and then issue the command `./jartobundle.sh <PATH_TO_DOWNLOADED_NON-OSGi_JAR> ../lib`.