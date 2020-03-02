# Creating a Project Solution

An ESB solution consists of one or several project directories. These directories store the various ESB artifacts that you create for your integration sequence. This solution allows you to create all the project directories that are required for your integration solution using one wizard.

Follow the steps given below.

1.  Open **WSO2 Integration Studio** and click **ESB Project → Create New** in the **Getting Started** view as shown below.
    <img src="../../assets/img/create_project/new_esb_project.png">

2.  In the **New ESB Solution Project** dialog that opens, enter a name for the ESB config project, and then select the check boxes for the projects you will need in your solution.
    <img src="../../assets/img/create_project/new_esb_soln_dialog.png">

    <table>
        <tr>
            <th>
                Project Type
            </th>
            <th>
                Description
            </th>
        </tr>
        <tr>
            <td>
                <a href="../../develop/creating-projects/#esb-config-project">ESB Config project</a>
            </td>
            <td>
                This is the main project directory that will store the mediation sequence and related mediation artifacts.
            </td>
        </tr>
        <tr>
            <td>
                <a href="../../develop/packaging-artifacts">Composite Application Project</a>
            </td>
            <td>
                This project directory is required for packaging the integration artifacts into a CAR file.
            </td>
        </tr>
        <tr>
            <td>
                <a href="../../develop/creating-projects/#registry-resource-project">Registry Resources project</a>
            </td>
            <td>
                Create this project if you will use registry resources in your integration solution.
            </td>
        </tr>
        <tr>
            <td>
                <a href="../../develop/creating-artifacts/adding-connectors">Connector Exporter project</a>
            </td>
            <td>
                Create this project if you will use **ESB connectors** in your integration solution.
            </td>
        </tr>
        <tr>
            <td>
                <a href="../../develop/create-docker-project">Docker Exporter project</a>
            </td>
            <td>
                Create this project if you want to be able to generate all the Docker resources for your integration solution.
            </td>
        </tr>
        <tr>
            <td>
                <a href="../../develop/create-kubernetes-project">Docker Exporter project</a>
            </td>
            <td>
                Create this project if you want to be able to generate all the Kubernetes resources for your integration solution.
            </td>
        </tr>
    </table>

4.  Click **Finish** to save the projects. The projects are listed in the project explorer.