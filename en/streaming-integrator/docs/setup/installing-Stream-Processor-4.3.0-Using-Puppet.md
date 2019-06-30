# Installing Stream Processor 4.3.0 Using Puppet

**Prerequisites** :

-   I nstall the Puppet server in the master node by following the
    Installation guide
    [here](https://puppet.com/docs/puppetserver/5.2/install_from_packages.html)
    .

-   Install the Puppet agent in the agent nodes by following the
    Installation guide
    [here](https://puppet.com/docs/puppet/5.4/install_linux.html) .

**Steps** :

1.  Clone WSO2 Stream Processor Puppet git repository and switch to the
    relevant resource directory by executing the following commands.  

    ``` java
        git clone https://github.com/wso2/puppet-sp
        cd puppet-sp
    ```

2.  Download the Deb or RPM binary distributions from following
    locations and copy to the files directory in relevant modules.
    -   WSO2 Stream Processor 4.3.0
        [deb](https://wso2.com/analytics-and-stream-processing/install/apt/)
        [rpm](https://wso2.com/analytics-and-stream-processing/install/yum/)
3.  Download the JDBC driver from the following location and copy to the
    files directory in relevant modules.
    -   [MySQL
        Connector/J](https://dev.mysql.com/downloads/connector/j/5.1.html)
4.  Run the existing scripts without customization.

The existing Puppet scripts contain the configurations to set up a
single node of the WSO2 Stream Processor runtime.

Run the following command to deploy the Stream Processor server.

``` java
    export FACTOR_runtime=sp_editor
    puppet agent -vt
```

Once the deployment is started, try to access the web UIs via the
following URLs and default credentials on the web browser .

``` java
    https://<ip-address>:9443/carbon
```

  

### Customizing the WSO2 Puppet scripts

If you need to alter the configurations given, please change the
parameterized values in the params.pp under manifests of each module.
You can add customizations to the custom.pp file under the same
manifests folder.
