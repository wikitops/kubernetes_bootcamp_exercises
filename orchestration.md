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

### Installation

There are a few methods to install kubectl, here are the basics depending on the operating system :

{% tabs %}
{% tab title="Debian / Ubuntu" %}
```bash
$ sudo apt-get update && sudo apt-get install -y apt-transport-https
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubectl
```
{% endtab %}

{% tab title="CentOS / RedHat / Fedora" %}
```text
$ cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
$ yum install -y kubectl
```
{% endtab %}
{% endtabs %}

### Configuration

In order for kubectl to find and access a Kubernetes cluster, it needs a kubeconfig file, which is created automatically when you create a cluster or successfully deploy a Minikube cluster. By default, kubectl configuration is located at `~/.kube/config`.

### Usage

Generally the command line format can be divide in three parts :

1. The kubectl command line binary
2. The action 
3. The object  to manage

This can be represented like this :

```bash
$ kubectl <ACTION> <OBJECT>
```

#### Actions

Here is an exhaustive list of actions that can be done :

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

#### Objects

Here is an exhaustive list of Kubernetes objects that can be managed :

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

Get the cluster information in command line.

```bash
$ kubectl cluster-info
```

#### Exercise n°2

Get the config deployed in the Kubernetes cluster.

```bash
$ kubectl config view
```

#### Exercise n°3

Get each elements deployed in the cluster in command line.

```bash
$ kubectl get all --all-namespaces
```

## YAML file

YAML, which stands for Yet Another Markup Language, or YAML Ain’t Markup Language is a human-readable text-based format for specifying configuration-type information.

Using YAML for Kubernetes definitions gives a number of advantages, including:

* **Convenience:** Declaring all the parameters in a command line is no longer needed
* **Maintenance:** YAML files can be added to source control to track changes
* **Flexibility:** Easier to configure complex structure in a file than a command line

YAML is a superset of JSON, which means that any valid JSON file is also a valid YAML file.

The usual basic structure of a Kubernetes YAML file definition look like this :

```yaml
apiVersion: [...]
kind: [...]
metadata:
    - [...]
spec:
    - [...]
```

* **apiVersion** : API version of the object to declare
* **kind** : Kubernetes object to manage \(ex: Pod, Deployment, Service ...\)
* **metadata** : metadata of the object declared \(like labels, annotations ...\)
* **spec** : main part of a YAML file, specification of each parameter defining the object declared \(ex: image, replicas, volumes, secrets, environment ...\)

#### Exercise n°1

Extract the default namespace YAML file definition with the command line.

```bash
$ kubectl get namespace default -o yaml
```

## Masters / Nodes

_Master_ components provide the cluster’s control plane. Master components make global decisions about the cluster \(for example, scheduling\), and detecting and responding to cluster events \(starting up a new pod when a replication controller’s ‘replicas’ field is unsatisfied\).

_Master_ components can be run on any machine in the cluster. However, for simplicity, set up scripts typically start all master components on the same machine, and do not run user containers on this machine.

_Node_ components run on every node, maintaining running pods and providing the Kubernetes runtime environment. They are the resources pool that will be managed by the masters to schedule the requiested objects.

#### Exercise n°1

List the all nodes of the cluster and identify the roles of each one.

```bash
$ kubectl get nodes
```

#### Exercise n°2

Describe one of the master node.

```bash
$ kubectl describe node MASTER_NAME
```

#### Exercise n°3

Get more information about nodes in one commande line.

```bash
$ kubectl get nodes -o wide
```

## Namespace

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces.

Namespaces are a way to divide cluster resources between multiple users via the definition of resource quotas.

#### Exercise n°1

List all the default namespaces created by the installer.

```bash
$ kubectl get namespace
```

#### Exercise n°2

Create the namespace _app-demo_ with the command line.

```bash
$ kubectl create namespace app-demo
```

#### Exercise n°3

Create a namespace _another-demo_ in declarative mode with a YAML file.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: another-demo
```

```bash
$ kubectl create -f namespace-demo.yaml
```

#### Exercise n°4

Describe the namespace _app-demo_.

```bash
$ kubectl describe namespace app-demo
```

#### Exercise n°5

Delete the namespace named "_another-demo_".

{% hint style="info" %}
Remember, when a namespace is deleted, each dependent objects are also deleted.
{% endhint %}

```bash
$ kubectl delete namespace another-demo
```

## Labels

Labels are key/value pairs that are attached to objects, such as pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. Labels can be used to organize and to select subsets of objects. Labels can be attached to objects at creation time and subsequently added and modified at any time. Each object can have a set of key/value labels defined. Each Key must be unique for a given object.

#### Exercise n°1

List all nodes of the cluster and display all their labels.

```bash
$ kubectl get nodes --show-labels
```

#### Exercise n°2

Add the key/value pair : _random-key=random-value_ to the first node of the cluster.

```bash
$ kubectl label nodes NODE_NAME random-key=random-value
```

#### Exercise n°3

Delete the key/value pair : _random-key=random-value_ of the first node of the cluster.

```bash
$ kubectl label nodes NODE_NAME random-key-
```

## External documentations

Those documentations can help you to go further in this topic :

* Kubernetes Official documentation of [the command line](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* Kubernetes Official [command line cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* Kubernetes Official documentation of [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
* Kubernetes Official documentation of [labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)



