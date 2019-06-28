# Message Processing in a Worker-Manager Cluster Mode

This section explains the use of message processor in a worker-manager
cluster mode. In such situations, only workers are required to
process/serve client requests while managers are mainly required to
carry out administration activities and synchronize the administrative
information of the workers in the cluster. A message processor may be
executed by one or more worker nodes.

When WSO2 EI is deployed in a clustered mode, you can increase the
number of tasks for a particular message processor by specifying the
task count in the message processor configuration as follows:

``` xml
     <parameter name="member.count">2</parameter> 
```

The above sample configuration ensures that there are two tasks running
for the given message processor. These tasks can be running in one or
more worker nodes in the clustered setup.

!!! info

This configuration does not guarantee that the message processor task
will run in all worker nodes in the clustered setup.

If the task count is less than the number of worker nodes in the
cluster, the task will not run in some workers nodes. On the other hand,
if the task count is larger than the worker nodes in the cluster, some
worker nodes may run more than one task.


All configuration changes done to the message processor will take effect
after the next EI instance/cluster restart.

### Shutting down a member with one or more message processors

When a worker node on which one or more message processor tasks are
running is shut down, those message processor tasks can be transferred
to other worker nodes in the cluster.
