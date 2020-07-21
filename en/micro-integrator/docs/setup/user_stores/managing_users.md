# Managing Users

Micro Integrator users with admin privileges can manage other users in an [LDAP](../setting_up_a_userstore/#configuring-an-ldap-user-store) or [RDBMS](../setting_up_a_userstore/#configuring-an-rdbms-user-store) user store that is connected to the Micro Integrator server. These user management tasks include <b>viewing</b>, <b>adding</b>, and <b>removing</b> users.

## Admin users

If a user with admin privileges does not exist in your user store, the admin credentials will be created when you invoke the Micro Integrator's [management API](../../../administer-and-observe/working-with-management-api) for the first time. That is, when you log in to the Micro Integrator server from the <b>CLI tool</b>/<b>dashboard</b>, or directly invoke the management API, the user credentials you use will get stored in the user store and admin privileges will be assigned.

An existing admin user can log in to the Micro Integrator server from the CLI tool or the dashboard to add new users with admin privileges. An admin user can only be removed by the creator.

## Managing users from the CLI

See the [Micro Integrator CLI documentation](../../../administer-and-observe/using-the-command-line-interface) to set up the tool. Be sure to log in to the Micro Integrator server (from the CLI) with your admin user name and password.

Use the [mi user](../../../administer-and-observe/using-the-command-line-interface/#mi-user) option in the CLI with the required commands as shown in the following examples:

```bash
# To add a new user.
mi user add [new user-id] [password] [is-admin]

# To remove a user
mi user remove [user-id]

# To list all the users
mi user show

# To list user by user ID
mi user show [user ID]

# To list users by user role
mi user show -r [role name]

# To list users matching a user name regex pattern
mi user show -p [user name regex pattern]
```

## Managing users from the Dashboard

See the [Micro Integrator Dashboard documentation](../../../administer-and-observe/working-with-monitoring-dashboard) to set up the dashboard. Be sure to log in to the Micro Integrator server (from the dashboard) with your admin user name and password.

Select <b>Users</b> in the left-hand navigator and use the <b>Users</b> tab to view the list of existing users. You can also delete other users (non-admin users and admin users created by you). 

<img src="../../../assets/img/monitoring-dashboard/dashboard-users-1.png" width="700">

Go to the <b>Add User</b> tab to create new users. Note that you can assign admin privileges during the creation.

<img src="../../../assets/img/monitoring-dashboard/dashboard-users-2.png" width="700">

