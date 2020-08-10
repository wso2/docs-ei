# RabbitMQ Deployment 

!!! Warning
	<b>Work in progress!</b>

Please refer the [Downloading and Installing RabbitMQ](https://www.rabbitmq.com/download.html) to configure the 
RabbitMQ deployment.

Following are the important points to be noted while doing the RabbitMQ Setup.

### High-Availability with RabbitMQ 

For highly availability, rabbitmq nodes need to be clustered. Please refer the [Clustering Guide](https://www.rabbitmq.com/clustering.html).

!!! Note
    Minimum node count recommended for clustering is 3 since RabbitMQ uses a [Quorum based distributed consensus](https://www.rabbitmq.com/clustering.html#node-count).
    Queues need to be mirrored for high availability and for fault tolerance. Refer [Mirrored Queues](https://www.rabbitmq.com/ha.html).
    
#### Example steps to setup a 3 node cluster ( version 3.8.2)

1. Edit each node `/etc/hostname` file and add following name (e.g: for rabbitmq-cluster-1-node1, add rabbitmq-cluster-1-node1.messaging.svc.local)

```toml tab='Node 1'
rabbitmq-cluster-1-node1.messaging.svc.local
```

```toml tab='Node 2'
rabbitmq-cluster-1-node2.messaging.svc.local
```

```toml tab='Node 3'
rabbitmq-cluster-1-node3.messaging.svc.local
```

!!! Note
    The host name should start with rabbitmq
    
2. Reboot each node for the changes to take effect

3. Add the following entries to each node `/etc/hosts` file

```bash
{node1_ip} rabbitmq-cluster-1-node1.messaging.svc.local rabbitmq-cluster-1-node1
{node2_ip} rabbitmq-cluster-1-node2.messaging.svc.local rabbitmq-cluster-1-node2
{node3_ip} rabbitmq-cluster-1-node3.messaging.svc.local rabbitmq-cluster-1-node3
```

4. Download RabbitMQ distribution to the `$HOME` folder

```bash
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.2/rabbitmq-server-generic-unix-3.8.2.tar.xz
```

5. Extract the distribution

```bash
tar -xf rabbitmq-server-generic-unix-3.8.2.tar.xz
```

6. Install the erlang distribution

```bash
wget -O- https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo apt-key add -
echo "deb https://packages.erlang-solutions.com/ubuntu bionic contrib" | sudo tee /etc/apt/sources.list.d/rabbitmq.list && \
sudo apt update && \
sudo apt -y install erlang
```

7. Create a file named as `rabbitmq.conf` in `$RABBITMQ_HOME/etc/rabbitmq/` and add the following entries to it

```bash
##
## Clustering
##

cluster_formation.peer_discovery_backend = rabbit_peer_discovery_classic_config

cluster_formation.classic_config.nodes.1 = rabbit@rabbitmq-cluster-1-node1
cluster_formation.classic_config.nodes.2 = rabbit@rabbitmq-cluster-1-node2
cluster_formation.classic_config.nodes.3 = rabbit@rabbitmq-cluster-1-node3
loopback_users = none
```

8. Repeat the above steps in other two nodes as well

9. Copy the `.erlang.cookie` file in the `$HOME (/home/ubuntu)` folder of node1 to other nodes (If the file does not exist, start and stop the RabbitMQ server)

10. Start the RabbitMQ server in each node one after the other using the following command

```bash
$RABBITMQ_HOME/sbin/rabbitmq-server -detached
```

11. Check the cluster status by RabbitMQ command line tool

```bash
$RABBITMQ_HOME/sbin/rabbitmqctl cluster_status
```

#### Other useful command with RabbitMQ

1. Create policy named high-available that has regular expression for applying the policy to queue name start with “queue”

```bash
rabbitmqctl set_policy high-available "^queue*" '{"ha-mode":"exactly","ha-params":2,"ha-sync-mode":"automatic"}'
```

2. Create a user

```bash
rabbitmqctl add_user wso2 password
rabbitmqctl set_user_tags wso2 administrator
rabbitmqctl list_users
```

3. Set permissions

```bash
rabbitmqctl set_permissions -p / wso2 ".*" ".*" ".*"
rabbitmqctl list_permissions -p /
```

4. Download rabbitmqadmin client to **sbin** folder

```bash
wget https://raw.githubusercontent.com/rabbitmq/rabbitmq-management/v3.8.2/bin/rabbitmqadmin
```

5. Set executable permission

```bash
chmod 755 rabbitmqadmin
```

6. Python required for execute the rabbitmqadmin

```bash
sudo apt install python
```

7. Install RabbitMQ management plugin

- Make sure you are in the sbin directory
- Execute the following command

```bash
rabbitmq-plugins enable rabbitmq_management
```

8. Create an exchange

```bash
rabbitmqadmin declare exchange --vhost=/ --user=wso2 --password=password name=queue-exchange type=direct durable=true
rabbitmqctl list_exchanges -p /
```

9. Create a queue

```bash
rabbitmqadmin declare queue --vhost=/ --user=wso2 --password=password name=queue1 durable=true
rabbitmqctl list_queues -p /
```

10. Add binding to the queue

```bash
rabbitmqadmin declare binding --vhost=/ --user=wso2 --password=password source=queue-exchange destination=queue1
rabbitmqctl list_bindings -p /
```
