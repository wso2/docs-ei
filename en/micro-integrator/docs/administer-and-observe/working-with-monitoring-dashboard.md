# Micro Integrator Dashboard

The Micro Integrator dashboard provides a graphical view of the synapse artifacts that are deployed in a specified Micro Integrator server instance. This dashboard is an alternative to the [Micro Integrator CLI](../../administer-and-observe/using-the-command-line-interface), which allows you to monitor your deployments from the command line. The dashboard as well as the CLI communicates with the management API of WSO2 Micro Integrator to function.

## Install and run the dashboard

!!! Warning
    In a non-production environment (with the self signed certificate), you have to add the certificate of the micro integrator instance to the browser as a trusted source. For example, direct the browser to `https://localhost:9164/management` and add the site as trusted. This step will not be required with a custom production certificate.

1.  To download the dashboard, go to [**WSO2 Micro Integrator** website](https://wso2.com/integration/micro-integrator/#) -> **Download** -> **Other Resources**, and click **Monitoring Dashboard**.
2.  Extract the downloaded ZIP file. This will be the `MI_DASHBOARD_HOME` directory.
3.  Open a terminal, navigate to the `MI_DASHBOARD_HOME/bin` directory, and execute the following command to start the dashboard server:

    ```bash
    sh dashboard.sh
    ```
    The dashboard server will start as follows.

    ```bash
    Web app 'dashboard' is available at 'https://127.0.0.1:9390/dashboard
    ```

## Sign in to the dashboard
  
1.  Copy the dashboard URL to your browser.

    ```bash
    https://127.0.0.1:9390/dashboard/login
    ```

2.  Enter the following details to sign in.

    ![login form for monitoring dashboard](../assets/img/monitoring-dashboard/login.png)

    <table>
        <tr>
            <th>
                Host
            </th>
            <td>
                The host name for the running Micro Integrator instance.
            </td>
        </tr>
        <tr>
            <th>
                Port
            </th>
            <td>
                The port exposing the management API of your running Micro Integrator instance. The default port is <b>9201</b>.
            </td>
        </tr>
        <tr>
            <th>
                User
            </th>
            <td>
                The user name to sign in.</br></br>
                <b>Note</b>: This should be a valid user name that is saved in the Micro Integrator server's user store. See <a href="../../../setup/user_stores/setting_up_a_userstore">configuring user stores</a> for information.
            </td>
        </tr>
        <tr>
            <th>
                Password
            </th>
            <td>
                The password of the user name.
            </td>
        </tr>
    </table> 

3.  Click <b>SIGN IN</b> and you are redirected to the home page of the Micro Integrator dashboard.
     
    ![login form for monitoring dashboard](../assets/img/monitoring-dashboard/home.png)
