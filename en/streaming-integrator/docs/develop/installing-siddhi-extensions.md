# Installing Siddhi Extensions

Streaming Integrator Tooling uses Siddhi extensions to connect with various data sources. Siddhi extensions can be installed or un-installed using the Extension Installer.

!!!Tip
    The Extension Installer can install/un-install extensions within Streaming Integrator Tooling. When deploying Siddhi applications in Streaming Integrator Server, these have to be manually done.

To install or un-install Siddhi extensions, follow the steps given below.

## Accessing the Extension Installer

1. Start the Streaming Integrator Tooling by issuing one of the following commands from the `<SI_HOME>/bin` directory.

    - For Windows: `tooling.bat`

    - For Linux: `./tooling.sh`

2. Click **Tools** menu option and then click **Extension Installer**. 
    
    ![Extensions Installer option in the Tools menu](../images/installing-siddhi-extensions/tools-menu.png)

    As a result, the following dialog box opens.

    ![Extensions Installer dialog box](../images/installing-siddhi-extensions/extensions-installer-dialog-box.png)

3. Locate the extension you wish to install/un-install.

    !!! tip
        Enter the name, or part of the name of an extension in the search bar, which will filter the extension(s) matching the entered keyword.
        
    ![Search Extensions](../images/installing-siddhi-extensions/search-filter-extensions.png)
    
    An extension will have one of the following statuses.
    <table>
        <tr>
            <th>Status</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Installed</td>
            <td>
            <b>The extension has been completely installed.</b> Jar of the extension itself, and all its dependencies (if any) have been installed.
            </td>
        </tr>
        <tr>
            <td>Not Installed</td>
            <td>
            <b>The extension has not been installed.</b> Jar of the extension itself has not been installed. Dependencies (if any) might have been installed due to <a href="#shared-dependencies-among-multiple-extensions">shared dependencies</a>.
            </td>
        </tr>
        <tr>
            <td>Partially Installed</td>
            <td>
            Jar of the extension itself has been installed. But, some of the dependencies of the extension have to be installed.
            </td>
        </tr>
        <tr>
            <td>Restart Required</td>
            <td>
                Installation or un-installation has finished for this extension. The editor has to be restarted in order to update the status.
            </td>
        </tr>
    </table>


4. An icon is shown next to the status as shown below, when there are [manually installable dependencies](#manually-installable-dependencies).

    ![Manually Installable Dependencies Available for Extension](../images/installing-siddhi-extensions/manually-installable-dependencies-available.png)


## Installing an extension

1. Click on the **Install** button next to an extension.

    ![Not Installed Extension](../images/installing-siddhi-extensions/a-not-installed-extension.png)


    The confirmation dialog will be shown as follows.

    ![Confirm Installation](../images/installing-siddhi-extensions/install-confirmation.png)

2. Click on the **Install** button to confirm. Installation will begin.

    ![Installing](../images/installing-siddhi-extensions/installing.png)

3. When the installation finishes, the following dialog will be shown. A restart is required after the installation.

    ![Restart Required After Installation](../images/installing-siddhi-extensions/restart-required-installation.png)

4. After restarting the editor, the particular extension's status will be updated.

    ![Status Change as Installed](../images/installing-siddhi-extensions/installed-status.png)


## Un-installing an extension

1. Click on the **UnInstall** button next to an extension.

    ![Installed Extension](../images/installing-siddhi-extensions/an-installed-extension.png)

    The confirmation dialog will be shown as follows.

    ![Confirm Un-Installation](../images/installing-siddhi-extensions/un-install-confirmation.png)

2. Click on the **UnInstall** button to confirm un-installation.

3. If the particular extension shares any dependency with other extension(s), a dialog box will be shown with information about [shared dependencies](#shared-dependencies-among-multiple-extensions). Otherwise, un-installation will begin.

    ### Shared Dependencies among Multiple Extensions

    Some dependencies are common for more than one extension. For example, _MySQL Connector_ is used by the _RDBMS MySQL extension_, as well as the _Change Data Capture MySQL extension_. In cases similar to this, un-installing one such extension will implicitly delete dependencies of other extension(s) too. Therefore, you will have to re-install those extensions, in order to use them.

    When trying to un-install such an extension, the following dialog box will be shown.
    
    ![Shared Dependencies Exist Dialog Box](../images/installing-siddhi-extensions/shared-dependencies-exist-dialog-box.png)

    This denotes that, some dependencies of _this_ extension (which you are trying to un-install), are also used by some other extension(s). Those extensions are shown in **bold**, and the dependencies that are common to _this_ extension are listed under them.

    The above screenshot shows that, the dependency `mysql-connector-java` is _also_ required by the **rdbms-mysql** extension, and the dependency `siddhi-io-cdc` is _also_ used by the extensions:  **cdc-oracle**, **cdc-postgresql**, **cdc-mssql** and **cdc-mongodb**.

    Pressing the **Confirm** button will continue the un-installation. The shown dependencies will be deleted as part of the un-installation, and therefore, the listed down extensions will loose some of their dependencies. You will have to re-install those extensions in order to use them.

4. The following dialog box will appear once the un-installation is complete. A restart is required after the un-installation.

    ![Restart Required After Installation](../images/installing-siddhi-extensions/restart-required-un-installation.png)


## Manually Installable Dependencies
Certain dependencies of some extensions can not be automatically downloaded through the Extension Installer. These dependencies should be manually downloaded and installed. When there is at least one such dependency for an extension, an icon will be shown as follows, next to the extension's status.
    
![Manually Installable Dependencies Available for Extension](../images/installing-siddhi-extensions/manually-installable-dependencies-available.png)

Clicking this icon will open the following dialog box.
    
![Manually Installable Dependency Instructions](../images/installing-siddhi-extensions/manually-installable-instructions.png)

The dialog box will show each dependency - that should be manually installed, of the particular extension, along with the following information.

- **Instructions** - Instructions to follow, in order to download (and convert) the jar of the dependency.
- **Installation Locations** - After following the instructions, the resultant jar should be placed in its specific location, based on the following table.

    <table>
        <tr>
            <th>Location</th>
            <th>Directory</th>
            <th>Jar Types</th>
        </tr>
        <tr>
            <td rowspan=2>runtime</td>
            <td>
                <code>&lt;SI_HOME&gt;/lib</code> or <code>&lt;SI_HOME&gt;/bundles</code> based on the instructions.
            </td>
            <td>
                <b>bundle in runtime:</b> An OSGi bundle should be placed in this directory. Conversion instructions will be available in the <code>instructions</code>.
            </td>
        </tr>
        <tr>
            <td>
                <code>&lt;SI_HOME&gt;/jars</code>
            </td>
            <td>
                <b>jar in runtime:</b> A non-OSGi bundle can be placed in this directory.
            </td>
        </tr>
        <tr>
            <td rowspan=2>samples</td>
            <td>
                <code>&lt;SI_HOME&gt;/samples/sample-clients/lib</code>
            </td>
            <td>
                <b>jar in samples:</b> A non-OSGi bundle can be placed in this directory.
            </td>
        </tr>
        <tr>
          <td></td>
          <td>
            <b>bundle in samples</b> is <b>not</b> applicable.
          </td>
        </tr>
    </table>

Installation of extensions of which, manually installable dependencies are present, will not be complete until you install these dependencies manually.


## Configuring Extension Dependencies

Configurations of extensions are loaded from the configuration file, which is located in `<SI_HOME>/wso2/server/resources/extensionsInstaller/extensionDependencies.json`.

When you are working with [custom extensions](../admin/writing-Custom-Siddhi-Extensions.md#writing-custom-siddhi-extensions), and if you want a custom extension to be installable from the Extension Installer, you should add the extension's configuration to the configuration file.

An extension's configuration is a JSON object that looks like the following.

```json
"<extension_name>": {
  "extension": {...},
  "dependencies": [
    {...},
    {...}
  ]
}
```
`<extension_name>` - which is the key of this JSON object, is the uniquely identifiable name of the extension. This is same as [`extension`](#extension)`.name`.

#### `extension`

This _object_ contains information about the extension, denoted by the following properties.

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>
      <code>name</code>
    </td>
    <td>
      Uniquely identifiable name of the extension.
    </td>
  </tr>
  <tr>
    <td>
      <code>displayName</code>
    </td>
    <td>
      Displayable name of the extension.
    </td>
  </tr>
  <tr>
    <td>
      <code>version</code>
    </td>
    <td>
      Version of the extension.
    </td>
  </tr>
</table>

The following is an example of the `extension` object, taken from the configuration of the `jms` extension.

```json
  "jms": {

    "extension": {
      "name": "jms",
      "displayName": "JMS",
      "version": "2.0.2"
    },

    "dependencies": [...]
  }
```


#### `dependencies`

This is an _array_. Each member of this _array_ is an _object_, which denotes information of a dependency of the extension, through the following properties.

!!! Info
        The jar of the Siddhi extension itself should be added as a dependency too. For example, in the configuration for the `jms` extension, you can see that `siddhi-io-jms` has been listed as a dependency under `dependencies`.

<table>
  <tr>
    <th>
      Property
    </th>
    <th>
      Description
    </th>
  </tr>
  <tr>
    <td>
      <code>name</code>
    </td>
    <td>
      Uniquely identifiable name of the dependency. If this dependency denotes the jar of the Siddhi extension itself, this starts with <code>siddhi-</code>.
    </td>
  </tr>
  <tr>
    <td>
      <code>version</code>
    </td>
    <td>
      Version of the dependency.
    </td>
  </tr>
  <tr>
    <td><code>download</code></td>
    <td>
      Denotes download information of the dependency, through the following properties.
      <table>
        <tr>
          <td>
            <code>autoDownloadable</code>
          </td>
          <td>
            Whether the dependency is auto downloadable (<code>true</code>) or not (<code>false</code> - <a href="#manually-installable-dependencies">manually installable</a>).
          </td>
        </tr>
        <tr>
          <td>
            <code>url</code>
          </td>
          <td>
            <b>Applicable only if the dependency is auto downloadable.</b> The download URL to download the dependency's jar.
          </td>
        </tr>
        <tr>
          <td>
            <code>instructions</code>
          </td>
          <td>
            <b>Applicable only if the dependency is <a href="#manually-installable-dependencies">manually installable</a>.</b> Instructions to download (and convert) the dependency's jar.
          </td>
        </tr>
      </table>
    </td>
  </tr>
  <tr>
    <td>
      <code>usages</code>
    </td>
    <td>
      This is an <i>array</i>. Each member of this <i>array</i> is an <i>object</i>, that denotes a directory where the dependency's jar should be present. Each such directory (location) is denoted by the following properties:
      <table>
        <tr>
          <td><code>type</code></td>
          <td>
            Type of the jar, whether an OSGi bundle (<code>BUNDLE</code>) or not (<code>JAR</code>).
          </td>
        </tr>
        <tr>
          <td><code>usedBy</code></td>
          <td>
            Type of location, where this jar is used. (<code>RUNTIME</code> or <code>SAMPLES</code>).
          </td>
        </tr>
      </table>
      For more information, please refer <b>installation locations</b> under <a href="#manually-installable-dependencies">Manually Installable Dependencies</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>lookupRegex</code>
    </td>
    <td>
      Regex pattern for the jar's file name. This is used to lookup and detect whether the jar is present in the locations, mentioned under <code>usages</code>.
    </td>
  </tr>
</table>

The following examples denote members of the `dependencies` array, taken from the configuration of the `jms` extension.

**Example1: Auto downloadable dependency**

This denotes the `hawtbuf` dependency of the `jms` extension, which is auto downloadable from the URL specified in `download.url`.

```json
  "jms": {
    "extension": {...},

    "dependencies": [

      {
        "name": "hawtbuf",
        "version": "1.9",
        "download": {
          "autoDownloadable": true,
          "url": "https://repo1.maven.org/maven2/org/fusesource/hawtbuf/hawtbuf/1.9/hawtbuf-1.9.jar"
        },
        "usages": [
          {
            "type": "BUNDLE",
            "usedBy": "RUNTIME"
          },
          {
            "type": "JAR",
            "usedBy": "SAMPLES"
          }
        ],
        "lookupRegex": "hawtbuf([_-])1.9.jar"
      },

      ...
    ]
  }
```

**Example2: Manually installable dependency**

This denotes the `activemq-client` dependency of the `jms` extension, which should be manually downloaded, and conversions should be done based on the given `download.instructions`.

```json
  "jms": {
    "extension": {...},

    "dependencies": [
      ...,

      {
        "name": "activemq-client",
        "version": "5.9.0",
        "download": {
          "autoDownloadable": false,
          "instructions": "Download the jar from 'https://repo1.maven.org/maven2/org/apache/activemq/activemq-client/5.9.0/activemq-client-5.9.0.jar' and convert ..."
        },
        "usages": [
          {
            "type": "BUNDLE",
            "usedBy": "RUNTIME"
          }
        ],
        "lookupRegex": "activemq([_-])client([_-])5.9.0(.*).jar"
      }
    ]
  }
```