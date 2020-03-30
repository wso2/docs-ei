# Creating a Maven Multi Module Project

A Maven Multi Module (MMM) project allows you to manage multiple projects such as Config projects, Composite Application projects, and Registry Resource projects as a single entity. 

The MMM project is the parent project in an integration solution and the subprojects are added as modules. By building the parent MMM project, you can build all the subprojects in the integration solution simultaneously. This allows you to seamlessly push your integration solutions to a **CI/CD** pipeline. Therefore, it is recommended as a best practice to create your Config project and other projects inside an MMM project.

## Creating the Maven Multi Module Project
Follow the steps given below.

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create New Maven Multi Module Project** in the **Getting Started** view as shown below.

    <img src="../../assets/img/create_project/create_maven_multi_maven_project" width="1000">

2.  In the **Maven Modules Creation Wizard** that opens, enter a name for the MMM project and other parameters as shown below.

    <img src="../../assets/img/create_project/new_maven_multi_module.png" width="500">

    Enter the following information:

    <table>
        <tr>
            <th>
                Parameter
            </th>
            <th>
                Description
            </th>
        </tr>
        <tr>
            <td>
                Artifact Id
            </td>
            <td>
                <b>Required</b>.</br></br> This will be the name of your MMM project.
            </td>
        </tr>
    </table>

3.  Click **Finish**. The MMM project is created in the project explorer. 

    <img src="../../assets/img/create_project/proj_explorer_maven_multi_module.png" width="400">

# Creating subprojects in the MMM project

Now you can create other projects inside the MMM project. For example, let's create an **Config** project and a **Composite Application** project:

1.  Right-click the created MMM project in the project explorer and go to **New -> Integration Project**. This will open the **New Integration Project** wizard. 
2.  Enter a name for the Config project in the **Integration Project Name** field.
3.  Be sure that the **Create Composite Appliction Project** check box is selected.
3.  Click **Finish** to complete the process. 

See that the new Config project and Composite Application project are created inside your MMM project folder (in the project explorer).

<img src="../../assets/img/create_project/proj_explorer_Integration_proj_in_multi_module.png" width="400">
