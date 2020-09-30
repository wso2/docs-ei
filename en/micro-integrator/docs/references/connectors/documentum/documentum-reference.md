!!! note
    This documentation is still work in progress.

# Documentum Connector Reference

The following operations allow you to work with the Documentum Connector. Find an operation name to see parameter details and samples on how to use it.

??? note "Create Folder"
    The Create Folder operation enables you to create a new folder in Documentum.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>repos</td>
            <td>The name of the repository. For example, doctest.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>objectcodeID</td>
            <td>Object Code ID. For example, "0c0277b68002952c"</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>foldername</td>
            <td>Folder name. For example, “Sample123”</td>
            <td>Yes.</td>
        </tr>
    </table>

    **Sample request**

    ```json
    {
        "repos":"doctest",
        "objectcodeID":"0c0277b68002952c",
        "foldername":" Sample123"
    }
    ```

??? note "Find Folder"
    The Find Folder operation is used to find the folder.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>repos</td>
            <td>The name of the repository. For example, doctest.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>folderObjectID</td>
            <td>Folder object ID. For example, “Sample123”</td>
            <td>Yes.</td>
        </tr>
    </table>

    **Sample request**

    ```json
    {
        "repos":"doctest",
        "folderObjectID":"0b0277b680048998"
    }
    ```

??? note "Delete Folder"
    The Delete Folder operation is used to delete the folder.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>repos</td>
            <td>The name of the repository. For example, doctest.</td>
            <td>Yes.</td>
        </tr>
        <tr>
            <td>folderObjectID</td>
            <td>Folder object ID. For example, “Sample123”</td>
            <td>Yes.</td>
        </tr>
    </table>

    **Sample request**

    ```json
    {
        "repos":"doctest",
        "folderObjectID":"0b0277b680048998"
    }
    ```

