---
description: >-
  In this section, you will learn how to orchestrate containers on a Kubernetes
  cluster.
---

# Orchestration

## Command Line

What ? Why ? How ? 

Explain the format :

* create
* delete
* describe
* get
* ...

Explain the objects :

| Object name | Object short name |
| :--- | :--- |
| pods | po |
| replicas | rs |
| namespace | ns |
| ... | ... |

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

Create a namespace app-demo

```text
kubectl create namespace app-demo
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



