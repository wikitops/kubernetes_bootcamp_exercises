---
description: >-
  A ReplicaSet ensures that a specified number of pod replicas are running at
  any given time.
---

# Replicas

## Module Overview

At the end of this module, you will :

* _Learn what is a Replicaset_
* _Learn how to scale a Pod_
* _Understand why replication is part of High Availability_

## Create

A ReplicaSet is a Kubernetes object that can be easily managed in a YAML file. A ReplicaSet has to be attached to a Pod definition thanks to selectors and labels to identify which object has to be scaled.

#### Exercise n°1

Create a ReplicaSet object to scale an Nginx Pod with 3 replicas.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: mynginxreplicaset
  labels:
    app: mynginxreplicaset
    env: formation
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mynginxreplicaset
    matchExpressions:
      - {key: app, operator: In, values: [mynginxreplicaset]}
  template:
    metadata:
      labels:
        app: mynginxreplicaset
        env: formation
    spec:
      containers:
      - name: nginx
        image: nginx
```

```bash
kubectl create -f FILENAME
```

## Get

Getting a list of ReplicaSet deployed on a Kubernetes cluster can be useful to quickly get the status of the object managed. A ReplicaSet has 3 states :

* desired : number of replicas desired
* current : current number of replicas in the cluster
* ready : number of replicas ready to production usage

#### Exercise n°1

List the ReplicaSet created in the namespace app-demo

```bash
kubectl get replicaset -n app-demo
```

## Describe

Describe a ReplicaSet object can be useful to get exhaustive informations on it for debug purpose.

#### Exercise n°1

Describe the replicaset of the nginx pod in the namespace app-demo.

```bash
kubectl describe replicaset REPLICASET_NAME -n app-demo
```

## Delete

What ? Why ? How ?

#### Exercise n°1

Delete the replicaset of the nginx pod in the namespace app-demo

```bash
kubectl delete replicaset REPLICASET_NAME -n app-demo
```

## Scale

What ? Why ? How ?

#### Exercise n°1

Scale the nginx pod in the namespace app-demo to 5 with the command line.

```bash
kubectl scale --replicas=5 -n app-demo POD_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [replicaset](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

