# Working with Local Registry Entries

The **local registry** acts as a memory registry where you can store
static content as a key-value pair, where the value could be a static
entry such as a text string, XML code, or a URL. This is useful for the
type of static content often found in XSLT files, WSDL files, URLs, etc.
Local entries can be referenced from mediators in the ESB
profile mediation flows and resolved at runtime.

-   [Creating a new local
    entry](#WorkingwithLocalRegistryEntries-Creatinganewlocalentry)
-   [Importing a local
    entry](#WorkingwithLocalRegistryEntries-importImportingalocalentry)
-   [Using a local
    entry](#WorkingwithLocalRegistryEntries-Usingalocalentry)

When you want to work with local registry entries, you can
use the plug-in (for WSO2 Integration Studio) to create a new local
entry as well as to import an existing local entry, or you
can add, edit, and delete local registry entries via the Management
Console.

The `         <localEntry>        ` element is used to declare registry
entries that are local to the the ESB profile instance as shown below:

``` java
    <localEntry key="string" src="url">text | xml</localEntry>
```

These entries are top-level entries and are globally visible within the
entire system. Values of these entries can be retrieved via the
extension XPath function
`         synapse:get-property(prop-name)        ` , and the keys of
these entries could be specified wherever a registry key is expected
within the configuration.

An entry can be static text specified as inline text or static XML
specified as an inline XML fragment, or it can be specified as a URL
(using the `         src        ` attribute). A local entry shadows any
entry with the same name from a remote Registry.

``` java
    <localEntry key="version">0.1</localEntry>
    <localEntry key="validate_schema">
       <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" ...
       </xs:schema>
    </localEntry>
    <localEntry key="xslt-key-req" src="file:repository/samples/resources/transform/transform.xslt"/>
```

If you want to add local entries before deploying the server, you can
add them to the top-level bootstrap file `         synapse.xml        `
, or to separate XML files in the `         local-entries        `
directory, which are located in under
`         <EI_HOME>\repository\deployment\server\synapse-configs\default        `
. When the server is started, these configurations will be added to the
registry.

### Working with local entries via WSO2 Integration Studio

You can create a new local entry or import an existing local entry from
an XML file, such as a Synapse configuration file using WSO2 Integration
Studio.

!!! tip

You need to have WSO2 Integration Studio installed to work with local
entries. For instructions, see [Installing WSO2 Integration
Studio](https://docs.wso2.com/display/EI650/Installing+WSO2+Integration+Studio)
.


#### Creating a new local entry

Follow these steps to create a new local entry. Alternatively, you can
[import an existing local
entry](#WorkingwithLocalRegistryEntries-import) .

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create
    New ** in the **Getting Started** tab as shown below.

    ![](attachments/119131678/119133615.png){width="800" height="372"}

2.  Enter a project name and click **Finish** .

3.  Right-click the project in the navigator and go to **New → Local
    Entry** to open the **New Local Entry** dialog.
4.  Select **Create a New Local Entry** and click **Next** .
5.  Type a unique name for the local entry, specify one of the following
    types of local entries, and then fill in the advanced configuration
    as described below:  
    -   In-Line Text Entry: Type the text you want to store
    -   In-Line XML Entry: Type the XML code you want to store
    -   Source URL Entry: Type or browse to the URL you want to store
6.  Do one of the following:  
    -   To save the local entry in an existing EI Config project in your
        workspace, click **Browse** and select that project.
    -   To save the local entry in a new EI Config project, click
        **Create new Project** and create the new project.
7.  Click **Finish** . The local entry is created in the local-entries
    folder under the EI Config project you specified, and the local
    entry appears in the editor. Click its icon in the editor to view
    its properties.

#### Importing a local entry

Follow these steps to import an existing local entry from an XML file
(such as a Synapse configuration file) into an EI Config project.
Alternatively, you can [create a new local
entry](https://docs.wso2.com/display/ESB500/Creating+ESB+Artifacts#CreatingESBArtifacts-Creatinganewlocalentry)
.

1.  Right-click the project in the navigator and go to **New → Local
    Entry** to open the **New Local Entry** dialog.
2.  Select **Import Local Entry** and click **Next** .
3.  Specify the local entry file by typing its full path name or
    clicking **Browse** and navigating to the file.
4.  In the **Save Local Entry In** field, specify an existing EI Config
    project in your workspace where you want to save the local entry, or
    click **Create new Project** to create a new EI Config project and
    save the local entry there.
5.  In the **Advanced Configuration** section, select the local entries
    you want to import.
6.  Click **Finish** . The local entries you selected are created in the
    `          local-entries         ` folder under the EI Config
    project you specified, and the first local entry appears in the
    editor.

#### Using a local entry

After you create a local entry, you can reference it from a mediator in
your mediation workflow. For example, if you created a local entry with
XSLT code, you can add an XSLT mediator to the workflow and then
reference the local entry as follows:

1.  Click the XSLT mediator to view its properties, click the **XSLT
    Static Schema Key** property, and then click the browse \[...\]
    button on the far right of the property's value.
2.  Click the **Workspace** link, and then navigate to and select the
    local entry that contains the XSLT code.
3.  Click **OK** .
