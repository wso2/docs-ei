# Managing Users

You can manage the users in your user store by using the **CLI** tool or the **monitoring dashboard**. If required, you can also directly use the management API for this purpose.

If you are user with admin privileges, you can add new users, view users, and delete users from the user store.

## Prerequisites

- [Enable the management API](../../../administer-and-observe/working-with-management-api/#enabling-the-management-api) when you start the Micro Integrator.
- Set up an [external user store](../setting_up_a_userstore) with **write access** for the Micro Integrator.

## Using the CLI Tool

1.	[Download](https://wso2.com/integration/micro-integrator/tooling/) and set up the Micro Integrator CLI tool.

2.  Initialize the CLI tool from your command line:

    ```bash
    ./mi
    ```

3.	Log in to the CLI tool with your admin credentials.

    !!! Tip
        The default admin user that is shipped with the Micro Integrator is `admin` and the password is also `admin`.

	```bash
	mi remote login
	```

4.  Execute the relevant command:

	```bash
	# To add a new user. This option is only available for admin users, therefore, set the `is-admin` flag to `true`
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

See [Micro Integrator CLI](../../../administer-and-observe/using-the-command-line-interface) for more details.

### Using the Monitoring Dashboard

1.	[Download](https://wso2.com/integration/micro-integrator/tooling/) and setup the Micro Integrator Monitoring dashboard.
2.	Open a terminal, navigate to the `<MI_DASHBOARD_HOME>/bin` directory, and execute the following command to start the dashboard server:
		```bash
		sh dashboard.sh
		```
3.	Open the dashboard using the following URL:
		```bash
		https://127.0.0.1:9743/dashboard/login
		```
4.  You need to log in with your admin credentials.

    !!! Tip
        The default admin user that is shipped with the Micro Integrator is `admin` and the password is also `admin`.

See [Micro Integrator Dashboard](../../../administer-and-observe/working-with-monitoring-dashboard) for more details.
