# Troubleshooting WSO2 Integration Studio

The following are some common errors that you may encounter when using
WSO2 Integration Studio and the way to troubleshoot and fix them.

-   [Getting the 'Save could not be completed'
    error?](#TroubleshootingWSO2IntegrationStudio-Gettingthe'Savecouldnotbecompleted'error?)
-   [Removing an
    artifact](#TroubleshootingWSO2IntegrationStudio-Removinganartifact)
-   [Retrieving a missing project
    view](#TroubleshootingWSO2IntegrationStudio-Retrievingamissingprojectview)

### Getting the 'Save could not be completed' error?

Are you getting the following error while trying to save the artifacts
using the MacOS?

![](attachments/119133563/119133564.png){width="631" height="250"}

To avoid this error, Mac users need to copy the
`         Eclipse.app        ` file into the
`         Applications        ` directory . If the Eclipse link is
outside the Applications directory it does not have the permission to
write files. As a result, this error is thrown.

### Removing an artifact

Once you remove an artifact, you need to save and reopen the CApp .pom
file. If not, the .pom file gets corrupted once the artifact is removed.

### Retrieving a missing project view

If your project view suddenly goes missing, you can get it back by
navigating to
`         Window -> Perspective -> Reset Perspective        ` from the
toolbar.

  
