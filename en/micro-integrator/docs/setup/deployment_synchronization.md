Use the following deployment synchronization recommendations based on the rate of change of artifacts that happen in your cluster:

* If your artifacts are changing frequently, use Network File Share (NFS).
* If your artifacts are changing at a medium rate, use Remote Synchronization (Rsync).
* If your artifacts are changing less frequently (for example, once a week) use a Configuration Management System (Puppet, Chef etc.).

## Using Network File share

You can use a common shared file system such as Network File System (NFS) or any other shared file system as the content synchronization mechanism. You need to mount the <EI_HOME>/repository/deployment/server directory of the two nodes to the shared file system to share all the artifacts between both nodes.

## Using Remote Synchronization

Deployment synchronization can be done using rsync, which is a file copying tool. These changes must be done in the manager node and in the same directory.

Create a file called workers-list.txt that lists all the worker nodes in the deployment. The following is a sample of the file where there are two worker nodes.

> Different nodes are separated by new lines.

``` bash
ubuntu@192.168.1.1:~/setup/192.168.1.1/as/as_worker/repository/deployment/server
ubuntu@192.168.1.2:~/setup/192.168.1.2/as/as_worker/repository/deployment/server
```

Create a file to synchronize the <PRODUCT_HOME>/repository/deployment/server folders between the manager and all worker nodes.

> You must create your own SSH key and define it as the pem_file. Alternatively, you can use an existing SSH key. Specify the manager_server_dir depending on the location in your local machine. Change the logs.txt file path and the lock location based on where they are located in your machine.

``` bash
#!/bin/sh

manager_server_dir=~/wso2as-5.2.1/repository/deployment/server/
pem_file=~/.ssh/carbon-440-test.pem


#delete the lock on exit
trap 'rm -rf /var/lock/depsync-lock' EXIT

mkdir /tmp/carbon-rsync-logs/

#keep a lock to stop parallel runs
if mkdir /var/lock/depsync-lock; then
  echo "Locking succeeded" >&2
else
  echo "Lock failed - exit" >&2
  exit 1
fi

#get the workers-list.txt
pushd `dirname $0` > /dev/null
SCRIPTPATH=`pwd`
popd > /dev/null
echo $SCRIPTPATH

for x in `cat ${SCRIPTPATH}/workers-list.txt`
do
echo ================================================== >> /tmp/carbon-rsync-logs/logs.txt;
echo Syncing $x;
rsync --delete -arve "ssh -i  $pem_file -o StrictHostKeyChecking=no" $manager_server_dir $x >> /tmp/carbon-rsync-logs/logs.txt
echo ================================================== >> /tmp/carbon-rsync-logs/logs.txt;
done

Create a Cron job that executes the above file every minute for deployment synchronization. Do this by running the following command in your command line.

Note: You can run the Cron job on one given node (master) at a given time. If you switch it to another node, you must stop the Cron job on the existing node and start a new Cron job on the new node after updating it with the latest files so far.

*   *  *   *   *     /home/ubuntu/setup/rsync-for-depsync/rsync-for-depsync.sh
```

## Configuring the WSO2 EI server

Depending on the deployment pattern you are setting up, you may have one or several WSO2 EI nodes installed in your deployment.

See [Deploying WSO2 Enterprise Integrator](deploying_wso2_ei.md) for instructions on how to set up your deployment.