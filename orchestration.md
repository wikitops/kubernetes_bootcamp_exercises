---
description: >-
  Orchestration is the automated arrangement, coordination, and management of
  computer systems, middleware, and services.
---

# Orchestration

## Module Overview

At the end of this module, you will :

* _Learn what a Kubernetes cluster is_
* _Learn how to manage it in command line_
* _Learn how to manage basic resources on a Kubernetes cluster_

## Command Line

The Kubernetes command-line tool, **kubectl**, is used to to deploy and manage applications on Kubernetes. Using kubectl, you can inspect cluster resources, create, delete, and update components, look at your new cluster and bring up apps.

You must use a kubectl version that is within one minor version difference of your cluster. For example, a v1.12 client should work with v1.11, v1.12, and v1.13 master. Using the latest version of kubectl helps avoid unforeseen issues.

#### Installation

#### Configuration

#### Usage



Explain the format :

| Action | Description |
| :--- | :--- |
| annotate | Update the annotations on a resource |
| api-resources | Print the supported API versions on the server, in the form of "group/version" |
| apply | Apply a configuration to a resource by filename or stdin |
| attach | Attach to a running container |
| auth | Inspect authorization |
| autoscale | Auto-scale a Deployment, ReplicaSet, or ReplicationController |
| certificate | Modify certificate resources |
| cluster-info | Display cluster info |
| config | Modify kubeconfig files |
| convert | Convert config files between different API versions |
| cordon | Mark node as unschedulable |
| cp | Copy files and directories to and from containers |
| create | Create a resource from a file or from stdin |
| delete | Delete resources by filenames, stdin, resources and names, or by resources and label selector |
| describe | Show details of a specific resource or group of resources |
| drain | Drain node in preparation for maintenance |
| edit | Edit a resource on the server |
| exec | Execute a command in a container |
| explain | Documentation of resources |
| expose | Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service |
| get | Display one or many resources |
| label | Update the labels on a resource |
| logs | Print the logs for a container in a pod |
| patch | Update field\(s\) of a resource using strategic merge patch |
| plugin | Runs a command-line plugin |
| port-forward | Forward one or more local ports to a pod |
| proxy | Run a proxy to the Kubernetes API server |
| replace | Replace a resource by filename or stdin |
| rolling-update | Perform a rolling update of the given ReplicationController \(Deprecated\) |
| rollout | Manage the rollout of a resource |
| run | Run a particular image on the cluster |
| scale  | Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job |
| set | Set specific features on objects |
| taint | Update the taints on one or more nodes |
| top | Display Resource \(CPU/Memory/Storage\) usage |
| uncordon | Mark node as schedulable |
| version | Print the client and server version information |

Explain the objects :

| Object name | Object short name |
| :--- | :--- |
| all | all |
| certificatesigningrequests | csr |
| clusterrolebindings | clusterrolebindings |
| clusterroles | clusterroles |
| componentstatuses | cs |
| configmaps | cm |
| controllerrevisions | controllerrevisions |
| cronjobs | cronjobs |
| customresourcedefinition | crd |
| daemonsets | ds |
| deployments | deploy |
| endpoints | ep |
| events | ev |
| horizontalpodautoscalers | hpa |
| ingresses | ing |
| jobs | jobs |
| limitranges | limits |
| namespaces | ns |
| networkpolicies | netpol |
| nodes | no |
| persistentvolumeclaims | pvc |
| persistentvolumes | pv |
| poddisruptionbudgets | pdb |
| podpreset | podpreset |
| pods | po |
| podsecuritypolicies | psp |
| podtemplates | podtemplates |
| replicasets | rs |
| replicationcontrollers | rc |
| resourcequotas | quota |
| rolebindings | rolebindings |
| roles | roles |
| secrets | secrets |
| serviceaccounts | sa |
| services | svc |
| statefulsets | sts |
| storageclasses | sc |

#### Exercise n°1

Get each elements deployed in the cluster.

```text
kubectl get all --all-namespaces
```

## Masters / Workers

What is a master ? What is a worker ?

#### Exercise n°1

List the nodes of the cluster and identify the roles of each one

```text
kubectl get nodes
```

## Namespace

What it is ?

#### Exercise n°1

List all the namespaces

```text
kubectl get namespace
```

#### Exercise n°2

Create a namespace app-demo in command line

```text
kubectl create namespace app-demo
```

#### Exercise n°3

Create a namespace namespace-demo in a file declaration

```text
apiVersion: v1
kind: Namespace
metadata:
  name: namespace-demo
```

```text
kubectl create -f namespace-demo.yaml
```

#### Exercise n°4

Describe the namespace app-demo

```text
kubectl describe namespace app-demo
```

## Labels

What is a label ?

#### Exercise n°1

Display the labels of each nodes

```text
kubectl get nodes --show-labels
```

#### Exercise n°2

Attach a label to the all nodes ...

```text
kubectl label nodes slave1 schedulePodName=hello-pod`
node "slave1" labeled
```

#### Exercise n°3

Attach a label to a specific node ...

```text
kubectl label nodes slave1 schedulePodName=hello-pod`
node "slave1" labeled
```

## External documentations

Those documentations can help you to go further in this topic :

* Kubernetes Official documentation of [the command line](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* Kubernetes Official [command line cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)



