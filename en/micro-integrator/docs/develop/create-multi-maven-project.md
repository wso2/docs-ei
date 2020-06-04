# Creating a Maven Multi Module Project

A Maven Multi Module (MMM) project allows you to manage multiple projects such as Config projects, Composite Application projects, and Registry Resource projects as a single entity. 

The MMM project is the parent project in an integration solution and the subprojects are added as modules. By building the parent MMM project, you can build all the subprojects in the integration solution simultaneously. This allows you to seamlessly push your integration solutions to a **CI/CD** pipeline. Therefore, it is recommended as a best practice to create your Config project and other projects inside an MMM project.

## Step 1: Creating the project
Follow the steps given below.

1.  Open **WSO2 Integration Studio** and click **Miscellaneous → Create Maven Multi Module Project** in the **Getting Started** view as shown below.

    <img src="../../assets/img/create_project/create_mmm_project.png" width="1000">

2.  In the **Maven Modules Creation Wizard** that opens, enter an artifact ID and other parameters as shown below. The artifact ID will be the name of your MMM project.

    <img src="../../assets/img/create_project/new_maven_multi_module.png" width="500">

3.  Click **Finish**. The MMM project is created in the project explorer. 

    <img src="../../assets/img/create_project/proj_explorer_mmm.png" width="300">

## Step 2: Creating subprojects in the MMM project

Now you can create other projects inside the MMM project. For example, let's create a **Config** project and a **Composite Application** project:

1.  Right-click the created MMM project in the project explorer and go to **New -> Integration Project**. This will open the **New Integration Project** wizard. 

    <img src="../../assets/img/create_project/mmm_integration_proj.png" width="700">

2.  Enter a name for the Config project in the **Integration Project Name** field.
3.  Select the relevant check boxes to create other subprojects.
4.  Click **Finish** to complete the process. 

See that the new Config project and other Composite Application project are created inside your MMM project folder (in the project explorer).

<img src="../../assets/img/create_project/proj_explorer_mmm_proj.png" width="400">
