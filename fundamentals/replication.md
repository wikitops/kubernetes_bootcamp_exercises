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

A ReplicaSet can be easily scaled up or down by simply updating the .spec.replicas field in a YAML file definition. The ReplicaSet controller ensures that a desired number of pods with a matching label selector are available and operational

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

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output of a get ReplicaSet command display three replicas status :

* desired : display the number of replicas defined in the YAML file definition
* current : display the current number of replicas deployed in the cluster
* ready : display the number of production ready replicas in the cluster

#### Exercise n°1

List the ReplicaSet created in the namespace app-demo

```bash
kubectl get replicaset -n app-demo
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the ReplicaSet\(s\) \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the ReplicaSet\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe the ReplicaSet of the Nginx Pod in the namespace app-demo.

```bash
kubectl describe replicaset REPLICASET_NAME -n app-demo
```

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Deleting a ReplicaSet will automatically delete each Pods associated with.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the ReplicaSet of the Nginx Pod in the namespace app-demo

```bash
kubectl delete replicaset REPLICASET_NAME -n app-demo
```

## Scale

Some Kubernetes objects like Deployment, ReplicaSet, Replication Controller, or Job can be easily scaled in command line.

The command line management has to be used only for debug purpose. In production environment, it's recommended to update the YAML file definition to ensure a consistent deployment.

#### Exercise n°1

Scale the Nginx Pod in the namespace app-demo to 5 with the command line.

```bash
kubectl scale --replicas=5 -n app-demo POD_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [replicaset](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

