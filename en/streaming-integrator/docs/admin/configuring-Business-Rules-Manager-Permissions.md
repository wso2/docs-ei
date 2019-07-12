# Configuring Business Rules Manager Permissions

There are two permission levels for a business rules application:

-   **Manager** : User roles with this permission level have
    administrative privileges over business rules. They are allowed to
    create, view, edit, deploy or delete business rules.
-   **Viewer** : User roles with this permission level are only allowed
    to view business rules.

The following topics cover how to configure Business Rules Manager
permissions.

-   [Prerequisites](#ConfiguringBusinessRulesManagerPermissions-Prerequisites)
-   [Configuring
    permissions](#ConfiguringBusinessRulesManagerPermissions-Configuringpermissions)

## Prerequisites 

Before configuring Business Rules Manager permissions, the user roles to
be assigned permissions must be already defined in the user store with
the required user IDs. For detailed instructions, see [User
Management](https://docs.wso2.com/display/SP440/User+Management) .

## Configuring permissions

Roles related to the Business Rules Manager need to be added under the
`         wso2.business.rules.manager        ` component namespace in
the `         <SP_HOME>/conf/ dashboard/deployment.yaml        ` file.

The following is a sample configuration of user roles for the Business
Rules Manager.  

``` xml
    wso2.business.rules.manager:
      roles:
        manager:
          - name: role1
            id: 1
        viewer:
          - name: role2
            id: 2
```
