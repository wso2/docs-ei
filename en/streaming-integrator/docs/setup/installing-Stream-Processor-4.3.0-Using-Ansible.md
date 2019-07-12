# Installing Stream Processor 4.3.0 Using Ansible

**Prerequisites** :

-   I nstall Ansible by following the Installation guide
    [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
    .

**Steps** :

1.  Clone WSO2 Stream Processor Ansible git repository and switch to the
    relevant resource directory by executing the following commands.  

    ``` java
        git clone https://github.com/wso2/ansible-sp
        cd ansible-sp
    ```

2.  Download the Deb or RPM binary distributions from following
    locations and copy to the files directory.
    -   WSO2 Stream Processor 4.3.0 package
3.  Download the JDBC driver from the following location and copy to the
    files directory.  
    -   [MySQL
        Connector/J](https://dev.mysql.com/downloads/connector/j/5.1.html)
4.  Run the playbook.

The existing Ansible scripts contain the configurations to set up a
single node WSO2 Stream Processor pattern. In order to deploy the
pattern, you need to replace the \[ip\_address\] given in the inventory
file under dev folder by the IP of the location where you need to host
the Stream Processor . An example is given below.

``` java
    [sp]
    dashboard_1 ansible_host=<ip_address> ansible_user=<ssh_user>
    
    
    editor_1 ansible_host=<ip_address> ansible_user=<ssh_user>
    
    
    worker_1 ansible_host=<ip_address> ansible_user=<ssh_user>
```

Run the following command to run the scripts.

``` java
    ansible-playbook -i dev site.yml
```

Once the deployment is started, try to access the web UIs via the
following URLs and default credentials on the web browser .

``` java
    Editor - https://<ip-address>:9390/editor
    Dashboard - https://<ip-address>:9643/monitoring
    
    Username: admin
    Password: admin
```

  

### Customize the WSO2 Ansible scripts

The templates that are used by the Ansible scripts are in j2 format
in-order to enable parameterization. If you need to alter the
configurations given, please change the parameterized values in the yaml
files under group\_vars and host\_vars. You can add customizations to
custom.yml under tasks of each role.

**Step 1**

Uncomment the following line in main.yml under the role you want to
customize.

``` java
    Import_tasks: custom.yml
```

**Step 2**

Add the configurations to the custom.yml . A sample is given below.

``` java
    - name: "Copy custom file"
      template:
        src: path/to/example/file/example.xml.j2
        dest: destination/example.xml.j2
      when: "(inventory_hostname in groups['apim'])"
```

  
