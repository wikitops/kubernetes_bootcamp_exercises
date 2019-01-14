---
description: >-
  Etcd is a consistent and highly-available key value store used as Kubernetes
  backing store for all cluster data.
---

# Etcd

## Clustering

Etcd can be deployed on a single server for test and development or as a cluster in production to ensure the high availability of the service.

Etcd is the default database of Kubernetes, if the service goes down, Kubernetes will not be able to manage any action on the deployed objects.

Depending on the size of the cluster, Etcd is usually deployed on the master nodes but remember that in production it is recommended to dedicate servers to the Etcd cluster.

### Initiating a cluster

For durability and high availability, run etcd as a multi-node cluster in production and back it up periodically. A five-member cluster is recommended in production.

#### Single-node etcd cluster

Use a single-node etcd cluster only for testing purpose.

```bash
etcd --listen-client-urls=http://ETCD_NODE_IP:2379 --advertise-client-urls=http://ETCD_NODE_IP:2379
```

#### Multi-node etcd cluster <a id="multi-node-etcd-cluster"></a>

```bash
etcd --listen-client-urls=http://ETCD_NODE_IP1:2379, http://ETCD_NODE_IP2:2379, http://ETCD_NODE_IP3:2379 --advertise-client-urls=http://ETCD_NODE_IP1:2379, http://ETCD_NODE_IP2:2379, http://ETCD_NODE_IP3:2379
```

### Getting members

Getting all the members of an etcd cluster.

```bash
etcdctl --endpoints=http://ETCD_IP1,http://ETCD_IP2 member list
```

### Adding member

Adding a node to an existing cluster. This command has to be done on an existing node of the cluster.

```bash
etcdctl member add NODE_NAME --peer-urls=http://NODE_IP:2380
```

### Removing member

Removing a node from an existing cluster. The NODE\_ID can be find in the getting member command line response.

```bash
etcdctl member remove NODE_ID
```

### Starting member

Starting a node needs some metadata to configure it in the cluster. This command has to be done after adding a node in the cluster. It does not initate a cluster.

```bash
etcd \
  --name ETCD_NODE_NAME \
  --listen-client-urls http://ETCD_NODE_IP:2379 \
  --advertise-client-urls http://ETCD_NODE_IP:2379 \
  --listen-peer-urls http://ETCD_NODE_IP:2380
```

## Backing up

All Kubernetes objects are stored on etcd. Periodically backing up the etcd cluster data is important to recover Kubernetes clusters under disaster scenarios, such as losing all master nodes.

### Running backup

Running a backup on an existing etcd cluster.

```bash
ETCDCTL_API=3 etcdctl --endpoints ETCD_CLUSTER_URL snapshot save PATH_TO_BACKUP_FILE
```

### Getting status

Getting the status of a backup.

```bash
ETCDCTL_API=3 etcdctl --write-out=table snapshot status PATH_TO_BACKUP_FILE
```

## Restoring

To restore a cluster, all that is needed is a single snapshot "db" file. A cluster restore with `etcdctl snapshot restore` creates new etcd data directories; all members should restore using the same snapshot. Restoring overwrites some snapshot metadata \(specifically, the member ID and cluster ID\); the member loses its former identity. This metadata overwrite prevents the new member from inadvertently joining an existing cluster. Therefore in order to start a cluster from a snapshot, the restore must start a new logical cluster.

In the case of a cluster of 3 servers, The command below has to be done on each node to reconfigure the cluster based on the same backup file.

```bash
ETCDCTL_API=3 etcdctl snapshot restore PATH_TO_BACKUP_FILE \
  --name ETCD_NODE_NAME1 \
  --initial-cluster ETCD_NODE_NAME1=http://ETCD_NODE_IP1:2380,ETCD_NODE_NAME2=http://ETCD_NODE_IP2:2380,ETCD_NODE_NAME3=http://ETCD_NODE_IP3:2380 \
  --initial-cluster-token ETCD_CLUSTER_TOKEN \
  --initial-advertise-peer-urls http://ETCD_NODE_IP1:2380
```

{% hint style="info" %}
The parameters ETCD\_NODE\_NAMEX and ETCD\_NODE\_IPX must be updated based on the host information to correctly restore the cluster.
{% endhint %}

## External documentation

To go further in the management of etcd, please refer to these documentations :

* Official [Kubernetes documentation](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/) on how operate an etcd cluster
* Official [Github documentation](https://github.com/etcd-io/etcd/tree/master/Documentation)
* Official [Github documentation](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/recovery.md#restoring-a-cluster) on disaster recovery
* Official [Github documentation](https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/clustering.md) on etcd clustering 



