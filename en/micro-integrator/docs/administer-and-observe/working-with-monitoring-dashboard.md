# Introduction
Monitoring dashboard comes as a separate distribution similar to the CLI tool. 
This dashboard provides a graphical representation of synapse artifacts that have been deployed in a 
specified micro integrator server instance.
The dashboard communicates with the management API, in order to function. Therefore, when using the 
monitoring dashboard, enabling the Management API in the server instance is mandatory.

# Starting the monitoring dashboard
- Start a Micro integrator instance with management api enabled.

```
sh micro-integrator.sh -DenableManagementApi
```

- Start the dashboard server.
```
sh dashboard.sh
```
- The dashboard server will start as follows.
```
Web app 'dashboard' is available at 'https://127.0.0.1:9743/dashboard
```

- Please note in a non-production environment (with the self signed certificate), you have to add
 the certificate of the micro integrator instance to the browser as a trusted source.
  Example: Direct the browser to  https://localhost:9164/magagement and add the site as trusted.
  (This step will not be required with a custom production certificate)
  
- Go to the login page 
Example : 
```
https://127.0.0.1:9743/dashboard/login
```

- Enter your credentials to the form.

![login form for monitoring dashboard](../assets/img/monitoring-dashboard/login.png)

- After a successful login, you will be redirected to the home page from where you can browse the
 deployed artifacts in the micro integrator server instance.
 
 ![login form for monitoring dashboard](../assets/img/monitoring-dashboard/home.png)

#Configurations

By default management api is shipped with a CORS configuration which allows all origins. This configuration can be found at MI-HOME/conf/internal-apis.xml file. 

Example:  
```
<cors>
       <enabled>true</enabled>
       <allowedOrigins>https://127.0.0.1:9743</allowedOrigins>
       <allowedHeaders>Authorization</allowedHeaders>
 </cors>

```
If required, you can remove the wild card and add a specific origin for this configuration for security requirements.  

As management dashboard is utilizing the management api, the user store is bound to the said api. Therefore, if you want to add a new user to view the management dashboard, you have to add a new user to the userstore defined in the internal-apis.xml.
Please note that in order to use the management dashboard you need to use JWT based authentication handler in `internal-apis.xml` file.

Example:
```
<UserStore>
    <users>
        <user>
            <username>admin</username>
            <password>admin</password>
        </user>
    </users>
</UserStore>
 ```
If the ` <UserStore>` element is not defined in `internal-apis.xml` user store will default to the carbon user store defined in user-mgt.xml.