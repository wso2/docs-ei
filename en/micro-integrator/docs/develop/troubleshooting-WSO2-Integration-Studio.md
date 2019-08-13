# Troubleshooting WSO2 Integration Studio

The following are some of the ways to troubleshoot errors that you may encounter when working with WSO2 Integration Studio.

## Getting the 'Save could not be completed' error?

You may get the following error while trying to save the artifacts
using the MacOS.

![troubleshooting](../../assets/img/workbench/troubleshooting-integration-studio.png)

To avoid this error, MacOS users need to copy the
`         Eclipse.app        ` file into the
`         Applications        ` directory . If the Eclipse link is
outside the **Applications** directory, it does not have the permission to
write files. As a result, this error is thrown.

## Removing an artifact

Once you remove an artifact, you need to save and reopen the `CApp.pom`
file. If not, the `.pom` file gets corrupted once the artifact is removed.

## Retrieving a missing project view

If your project view suddenly goes missing, you can get it back by
navigating to **Window -> Perspective -> Reset Perspective** from the
tool bar.

  
