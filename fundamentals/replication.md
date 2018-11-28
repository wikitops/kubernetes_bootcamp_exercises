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

What ? Why ? How ? CLI replicaset = rs

#### Exercise n°1

Create a pod YAML file with 2 replicas of the nginx container in the namespace app-demo.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginxreplicated
  labels:
    env: formation
spec:
  replicas: 1
  containers:
  - name: nginx
    image: nginx
```

```bash
kubectl create -f FILENAME
```

## List

What ? Why ? How ?

#### Exercise n°1

List the replicaset created in the namespace app-demo

```bash
kubectl get replicaset -n app-demo
```

## Describe

What ? Why ? How ?

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

