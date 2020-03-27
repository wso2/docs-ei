# Creating a Maven Multi Module Project

Maven Multi Module projects allow to manage multiple projects such as Config projects, Composite Application projects, and Registry Resource Projects as a single entity paving way to a seamless **CI/CD** pipeline. Therefore, it is recommended as a best practice to create your Config and other projects in a Maven Multi Module project. 

The Maven Multi Module project act as the parent project and sub-projects are added as modules. Therefore, when you build the Multi Module project, all the projects that are in the Multi Module project get built.

## Prerequisites

It is recommended to [get the latest updates](../../develop/installing-WSO2-Integration-Studio#get-the-latest-updates) for your [WSO2 Integration Studio](../../develop/installing-WSO2-Integration-Studio) before trying these instructions.

## Creating the Maven Multi Module Project
Follow the steps given below.   

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create New Maven Multi Module Project** in the **Getting Started** view as shown below.

    <img src="../../assets/img/create_project/create_maven_multi_maven_project" width="1000">

2.  In the **Maven Modules Creation Wizard** dialog that opens, enter a name for the Multi Module project and other parameters as shown below.

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
                <b>Required</b>. Give a name for the Multi Module project.
            </td>
        </tr>
    </table>

3.  Click **Finish**. The Maven Multi Module project is created in the project explorer. 

    <img src="../../assets/img/create_project/proj_explorer_maven_multi_module.png" width="400">

# Creating An Integration Project in the Maven Multi Module Project
Now you can create other projects inside the **Maven Multi Module Project**. For example, let's create an **Integration Project** by doing the following steps:

1.  Right click on the created **Maven Multi Module Project** in the project explorer, navigate to **New** and select **Integration Project**.

2.  Enter a name for the Config project in the opened **New Integration Project** dialog box.
 
3.  Click Finish and see that the new **Config project** and the **Composite Application project** are now listed inside the **Maven Multi Module project** in the project explorer.

    <img src="../../assets/img/create_project/proj_explorer_Integration_proj_in_multi_module.png" width="400">
