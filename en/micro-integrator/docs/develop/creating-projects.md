# Creating ESB Projects

Create the project directories that are required for storing the various synapse artifacts that build your integration use case.

## ESB Solution Project

An ESB solution consists of one or several project directories. These directories store the various ESB artifacts that you create for your integration sequence.

<ol>
    <li>
        Open <b>WSO2 Integration Studio</b> and click <b>ESB Project → Create New</b> in the <b>Getting Started</b> view as shown below.</br></br><img src="../../assets/img/create_project/new_esb_project.png">
    </li>
    <li>
        In the <b>New ESB Solution Project</b> dialog that opens, enter a name for the ESB config project. Select the relevant check boxes if you want to create a <a href="#registry-resource-project">Registry Resources project</a>, <a href="#connector-exporter-project">Connector Exporter project</a>, and/or a <a href="#composite-application-project">Composite Application project</a> along with the <a href="#esb-config-project">ESB Config project</a>.</br></br><img src="../../assets/img/create_project/new_esb_soln_dialog.png">
    </li>
    <li>
        Click <b>Finish</b> to save the projects. The ESB projects are listed in the project explorer as shown below.</br></br>
        <img src="../../assets/img/create_project/proj_explorer.png">
    </li>
</ol>

## ESB Config Project

This project directory stores the ESB artifacts that are used when defining a mediation flow.

<ol>
    <li>
        Open <b>WSO2 Integration Studio</b> and click <b>Miscellaneous → Create New Config Project</b> in the <b>Getting Started</b> view as shown below.</br></br><img src="../../assets/img/create_project/new_config_project.png">
    </li> 
    <li>
        In the dialog that opens, select <b>New ESB Config Project</b> and click <b>Next</b>.
    </li>
    <li>
        Enter a name for the ESB config project.
    </li>
    <li>
        Click <b>Finish</b> and see that the project is now listed in the project explorer.
    </li>
</ol>

You can now start creating the [synapse artifacts](../develop/creating-artifacts/creating-an-api.md) in your ESB Config project.

## Registry Resource Project

Create this project directory if you want to create registry resources for your mediation flow. You can later use these registry artifacts when you define your mediation sequences in the ESB config project.
            
<ol>
    <li>
        Open <b>WSO2 Integration Studio</b> and click <b>Miscellaneous → Create New Registry Project</b> in the <b>Getting Started</b> view as shown below.</br></br><img src="../../assets/img/create_project/new_registy_project.png">
    </li>
    <li>
        In the dialog that opens, enter a name for the registry project. 
    </li>
    <li>
        Click <b>Finish</b> and see that the project is now listed in the project explorer.  
    </li>
</ol>

See the instructions on <a href="../../develop/creating-artifacts/creating-registry-resources/#creating-registry-resources">creating and using registry artifacts</a>.

## Data Services Project

Create this project directory to start creating data services (.dbs files) for exposing various datasources as a service.</br>
<ol>
    <li>
        Open <b>WSO2 Integration Studio</b> and click <b>DS Project → Create New Data Service</b> in the <b>Getting Started</b> view as shown below.
                    <img src="../../assets/img/create_project/data_services_project.png">
    </li>
    <li>
        In the dialog that opens, enter a project name and click <b>Next</b>.
    </li>
    <li>
        Click <b>Finish</b> and see that the project is now listed in the project explorer.
    </li>
</ol>

You can now start <a href="../../develop/creating-artifacts/data-services/creating-data-services/">managing data service artifacts</a> using <b>WSO2 Integration Studio</b>.

## Connector Exporter Project

Create this project directory if you have used ESB connectors in your medition sequence (defined in the ESB config project). All connector artifacts need to be stored in a connector exporter project before packaging. See the instructions on [creating and using connectors](../develop/creating-artifacts/adding-connectors.md).

## Composite Application Project

This poject directory allows you to package all the artifacts (stored in other ESB projects) into one composite application (C-APP). This C-APP can then be deployed in the ESB server. See the instructions on packaging ESB artifacts.

<!--
<table>
    <tr>
        <td><b>ESB Solution</b></td>
        <td>An ESB solution consists of one or several project directories. These directories store the various ESB artifacts that you create for your integration sequence.
            <ol>
                <li>
                    Open <b>WSO2 Integration Studio</b> and click <b>ESB Project → Create New</b> in the <b>Getting Started</b> view as shown below.</br></br>
                    <img src="../../assets/img/create_project/new_esb_project.png">
                </li>
                <li>
                    In the <b>New ESB Solution Project</b> dialog that opens, enter a name for the ESB config project. Select the relevant check boxes if you want to create a <a href="#registry-resource-project">Registry Resources project</a>, <a href="#connector-exporter-project">Connector Exporter project</a>, and/or a <a href="#composite-application-project">Composite Application project</a> along with the <a href="#esb-config-project">ESB Config project</a>.</br></br>
                    <img src="../../assets/img/create_project/new_esb_soln_dialog.png">
                </li>
                <li>
                    Click <b>Finish</b> to save the projects. The ESB projects are listed in the project explorer as shown below.</br></br>
                    <img src="../../assets/img/create_project/proj_explorer.png">
                </li>
            </ol>
        </td>
    </tr>
    <tr>
        <td id="esb-config-project"><b>ESB Config Project</b></td>
        <td>
            This project directory stores the ESB artifacts that are used when defining a mediation flow.
            <ol>
               <li>
                    Open <b>WSO2 Integration Studio</b> and click <b>Miscellaneous → Create New Config Project</b> in the <b>Getting Started</b>view as shown below.</br></br>
                    <img src="../../assets/img/create_project/new_config_project.png">
                </li> 
                <li>
                    In the dialog that opens, select <b>New ESB Config Project</b> and click <b>Next</b>.
                </li>
                <li>
                    Enter a name for the ESB config project.
                </li>
                <li>
                    Click <b>Finish</b> and see that the project is now listed in the project explorer.
                </li>
            </ol>
            You can now start creating the <a href="#creating-synapse-artifacts">synapse artifacts</a> in your ESB Config project.
        </td>
    </tr>
    <tr>
        <td id="registry-resource-project"><b>Registry Resource Project</b></td>
        <td>
            Create this project directory if you want to create registry resources for your mediation flow. You can later use these registry artifacts when you define your mediation sequences in the ESB config project.
            <ol>
                <li>
                    Open <b>WSO2 Integration Studio</b> and click <b>Miscellaneous → Create New Registry Project</b> in the <b>Getting Started</b> view as shown below.</br></br>
                    <img src="../../assets/img/create_project/new_registy_project.png">
                </li>
                <li>
                   In the dialog that opens, enter a name for the registry project. 
                </li>
                <li>
                  Click <b>Finish</b> and see that the project is now listed in the project explorer.  
                </li>
            </ol>
            See the instructions on <a href="../../use-cases/tasks/registry_artifacts/working-with-registry-artifacts">creating and using registry artifacts</a>.
        </td>
    </tr>
    <tr>
        <td id="mediator-project"><b>Mediator Project</b></td>
        <td>
           Create this project directory to start creating custom mediator artifacts. You can use these customer mediators when you define the mediation flow in your ESB config project.
           <ol>
               <li>
                   Open <b>WSO2 Integration Studio</b> and click <b>Miscellaneous → Create Mediator Project</b> in the <b>Getting Started</b> view as shown below.</br></br>
                   <img src="../../assets/img/create_project/new_mediator_project.png">
               </li>
               <li>
                 In the dialog that opens, select <b>Create New Mediator</b> and click <b>Next</b>.  
               </li>
               <li>
                 Enter a project name, package name, and class name.</br></br>
                 <img src="../../assets/img/create_project/new_mediator_artifact_dialog.png">
               </li>
               <li>
                   Click <b>Finish</b> and see that the project is now listed in the project explorer.
               </li>
           </ol>
           You can now start creating <a href="../../use-cases/tasks/working-with-mediators">custom mediators</a>.
        </td>
    </tr>
    <tr>
        <td id="data-services-project"><b>Data Services Project</b></td>
        <td>
            Create this project directory to start creating data services (.dbs files) for exposing various datasources as a service.</br></br>
            <ol>
                <li>
                    Open <b>WSO2 Integration Studio</b> and click <b>DS Project → Create New Data Service</b> in the <b>Getting Started</b> view as shown below.
                    <img src="../../assets/img/create_project/data_services_project.png">
                </li>
                <li>
                    In the dialog that opens, enter a project name and click <b>Next</b>.
                </li>
                <li>
                    Click <b>Finish</b> and see that the project is now listed in the project explorer.
                </li>
            </ol>
            You can now start <a href="../../use-cases/tasks/managing-data-services-artifacts">managing data service artifacts</a> using <b>WSO2 Integration Studio</b>.
        </td>
    </tr>
    <tr>
        <td id="connector-exporter-project"><b>Connector Exporter Project</b></td>
        <td>
            Create this project directory if you have used ESB connectors in your medition sequence (defined in the ESB config project). All connector artifacts need to be stored in a connector exporter project before packaging. See the instructions on <a href="../../use-cases/tasks/connectors/working-with-connectors">creating and using connectors</a>.
        </td>
    </tr>
    <tr>
        <td id="composite-application-project"><b>Composite Application Project</b></td>
        <td>
            This poject directory allows you to package all the artifacts (stored in other ESB projects) into one composite application (C-APP). This C-APP can then be deployed in the ESB server. See the instructions on packaging ESB artifacts.
        </td>
    </tr>
</table>
-->

## Alternative: Creating Projects

You can use the above ESB projects and other various projects as follows:

1.  Right-click on the **Project Explorer** and click **New → Project** as shown below.
    ![Create new project](../../assets/img/create_project/new_project_root.png)
2.  In the **New Project** dialog that opens, select the required project.  
    ![Create new project dialog](../../assets/img/create_project/new_project_root_dialog.png)