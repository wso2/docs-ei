# Installing Stream Processor Using Kubernetes

To install WSO2 Stream Processor using Kubernetes, follow the steps
below:

1.  Install
    [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
    and [Kubernetes
    client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
    (tested with v1.10). As a result, an already setup [Kubernetes
    cluster](https://kubernetes.io/docs/setup/pick-right-solution/) with
    [NGINX Ingress
    Controller](https://kubernetes.io/docs/setup/pick-right-solution/)
    is enabled.
2.  A pre-configured Network File System (NFS) is used as the persistent
    volume for artifact sharing and persistence.  
    In the NFS server instance, create a Linux system user account named
    `           wso2carbon          ` with `           802          ` as
    the user ID, and a system group named `           wso2          `
    with `           802          ` as the group ID. Add the
    `           wso2carbon          ` user to the
    `           wso2          ` group.

    ``` text
        groupadd --system -g 802 wso2
        useradd --system -g 802 -u 802 wso2carbon
    ```

## Fully distributed deployment of WSO2 Stream Processor

To set up a fully distributed deployment of WSO2 Stream Processor with
Kubernetes, follow the steps below:

Clone the Kubernetes resources for [WSO2 Stream Processor Git
repository](https://github.com/wso2/kubernetes-sp) .

!!! info

The local copy of the Git repository is referred to as
`           <KUBERNETES_HOME>          ` from here onwards.


``` text
    git clone https://github.com/wso2/kubernetes-sp.git
```

Setup a Network File System (NFS) to be used for persistent storage as
follows.  

1.  Create and export unique directories within the NFS server instance
    for each Kubernetes Persistent Volume resource defined in the
    `             <KUBERNETES_HOME>/pattern-distributed/volumes/persistent-volumes.yaml            `
    file.

    ``` text
            sudo chown -R wso2carbon:wso2 <directory_name>
    ```

2.  Grant ownership to the `             wso2carbon            ` user
    and `             wso2            ` group, for each of the
    previously created directories by issuing the following command.

    ``` text
            chmod -R 700 <directory_name>
    ```

3.  Then, update each Kubernetes Persistent Volume resource with the
    corresponding NFS server IP (NFS\_SERVER\_IP), and the NFS server
    directory path (NFS\_LOCATION\_PATH) of the directory you exported.

Setup product database(s) using MySQL in Kubernetes. Here, a NFS is
needed for persisting MySQL DB data.

Create and export a directory within the NFS server instance.

Provide read-write-execute permissions to other users for the created
folder.

Then, update the Kubernetes Persistent Volume resource with the
corresponding NFS server IP (NFS\_SERVER\_IP) and exported, NFS server
directory path (NFS\_LOCATION\_PATH) in
`           <KUBERNETES_HOME>/pattern-distributed/extras/rdbms/volumes/persistent-volumes.yaml          `
.

For a serious deployment (e.g. production grade setup), it is
recommended to connect product instances to a user owned and managed
RDBMS instance.

Navigate to the
`           <KUBERNETES_HOME>/pattern-distributed/scripts          `
directory as follows.

    cd <KUBERNETES_HOME>/pattern-distributed/scripts

  
Deploy the Kubernetes resources by executing the
`           <KUBERNETES_HOME>/pattern-distributed/scripts/deploy.sh          `
script as follows.

    ./deploy.sh --wso2-username=<WSO2_USERNAME> --wso2-password=<WSO2_PASSWORD> --cluster-admin-password=<K8S_CLUSTER_ADMIN_PASSWORD>WSO2_USERNAME: Your WSO2 username 

  

-   `            WSO2_USERNAME           ` : Your WSO2 username

-   `            WSO2_PASSWORD           ` : Your WSO2 password

-   `            K8S_CLUSTER_ADMIN_PASSWORD           ` : Kubernetes
    cluster admin password

      

Access product management consoles.  
  
Obtain the external IP (EXTERNAL-IP) of the Ingress resources by listing
down the Kubernetes Ingresses.  
kubectl get ing  
  
The external IP can be found under the ADDRESS column of the output.  
  
Add the above host as an entry in the `           /etc/hosts          `
file as shown below:  
  
\<EXTERNAL-IP\> wso2sp-dashboard

    <EXTERNAL-IP> wso2sp-manager-1
    <EXTERNAL-IP> wso2sp-manager-2

  

Try navigating to <https://wso2sp-dashboard/monitoring> from your
favorite browser.
