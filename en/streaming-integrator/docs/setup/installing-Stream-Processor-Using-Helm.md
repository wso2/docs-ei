# Installing Stream Processor Using Helm

To install WSO2 Stream Processor using Helm, follow the steps below:

1.  Install
    [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
    ,
    [Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md)
    (and Tiller) and [Kubernetes
    client](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
    (tested with v1.10). As a result:
    -   An already setup [Kubernetes
        cluster](https://kubernetes.io/docs/setup/pick-right-solution/)
        with [NGINX Ingress
        Controller](https://kubernetes.github.io/ingress-nginx/deploy/)
        enabled.
    -   A pre-configured Network File System (NFS) to be used as the
        persistent volume for artifact sharing and persistence.
2.  In the NFS server instance, create a Linux system user account named
    `             wso2carbon            ` with
    `             802            ` as the user ID, and a system group
    named `             wso2            ` with
    `             802            ` as the group ID. Add the
    `             wso2carbon            ` user to the
    `             wso2            ` group.

    ``` text
        groupadd --system -g 802 wso2 
        useradd --system -g 802 -u 802 wso2carbon
    ```

## Setting up a minimum HA cluster

To set up WSO2 SP as a minimum HA cluster using Helm, follow the steps
below:

  

!!! info

In the context of this scenario, `         <KUBERNETES_HOME>        `
refers to the the local copy of the GitHub repository, and
`         <HELM_HOME>        ` refers to the
`         <KUBERNETES_HOME>/helm/pattern-distributed        ` directory.


  
  

1.  Clone Kubernetes Resources for WSO2 Stream Processor Git
    repository.  

    `             git clone                           https://github.com/wso2/kubernetes-sp.git                         `

2.  Setup a Network File System (NFS) to be used for persistent storage.
    Then do the following.
    1.  Create and export unique directory within the NFS server
        instance for the following Kubernetes Persistent Volume resource
        defined in the
        `              <HELM_HOME>/pattern-distributed/values.yaml             `
        file:  
        -   `                sharedSiddhiFilesLocationPath               `
    2.  Grant ownership to the `               wso2carbon              `
        user and the `               wso2              ` group, for each
        of the previously created directories.

        ``` text
                sudo chown -R wso2carbon:wso2 <directory_name>
        ```

    3.  Grant read-write-execute permissions to the
        `               wso2carbon              ` user, for each of the
        previously created directories.

        ``` java
                    chmod -R 700 <directory_name>
        ```

3.  Provide configurations as follows.
    1.  The default product configurations are available at
        `              <HELM_HOME>/pattern-distributed/confs             `
        directory. Update them if required.
    2.  Open the
        `               <HELM_HOME>/pattern-distributed/values.yaml              `
        file and provide the following values.

        | Parameter                                                           | Description                                                                                                                                  |
        |---------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
        | `                   username                  `                     | Your WSO2 username.                                                                                                                          |
        | `                   password                  `                     | Your WSO2 password.                                                                                                                          |
        | `                   email                  `                        | Your WSO2 email.                                                                                                                             |
        | `                   serverIp                  `                     | NFS Server IP.                                                                                                                               |
        | `                   sharedDeploymentLocationPath                  ` | The NFS location path of the shared Siddhi file directory (i.e.., `                   <SP_HOME>/deployment/siddhi-files                  ` ) |

4.  Deploy product database(s) using MySQL in Kubernetes.

    ``` text
            helm install --name sp-rdbms -f <HELM_HOME>/mysql/values.yaml stable/mysql --namespace wso2
    ```

    For a serious deployment (e.g. production grade setup), it is
    recommended to connect product instances to a user owned and managed
    RDBMS instance.

5.  Deploy the fully distributed deployment of WSO2 Stream Processor.  

    ``` text
            helm install --name <RELEASE_NAME> <HELM_HOME>/pattern-distributed
    ```

6.  Access product management consoles. Then do the following.
    1.  Obtain the external IP (EXTERNAL-IP) of the Ingress resources by
        listing down the Kubernetes Ingresses.

        ``` text
                    kubectl get ing -n wso2
        ```

          

    2.  Add the above host as an entry in /etc/hosts file as shown
        below:

        ``` text
                    <EXTERNAL-IP> wso2sp-dashboard
                    <EXTERNAL-IP> wso2sp-manager-1
                    <EXTERNAL-IP> wso2sp-manager-2
        ```

7.  Navigate to <https://wso2sp-dashboard/monitoring> from your favorite
    browser.
