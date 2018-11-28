---
description: >-
  A Deployment object describe the state of Pods and ReplicaSets in a
  declarative ways.
---

# Deployment

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML Deployment file_
* _Learn how to manage a Deployment_
* _Learn the composition of a Deployment_

## Create

A Deployment controller provides declarative updates for Pods and ReplicaSets.

Describe a desired state in a Deployment object, and the Deployment controller changes the actual state to the desired state at a controlled rate. A Deployment object can create, update or delete resources or parameters.

#### Exercise n°1

Deploy an Nginx Pod in a declarative way with a Deployment object.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mynginxdeployment
  labels:
    app: mynginxdeployment
spec:
  selector:
    matchLabels:
      app: mynginxdeployment
  template:
    metadata:
      labels:
        app: mynginxdeployment
    spec:
      containers:
      - name: nginx
        image: nginx
```

#### Exercise n°2

Update the previous deployment to scale it at 3.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mynginxdeployment
  labels:
    app: mynginxdeployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mynginxdeployment
  template:
    metadata:
      labels:
        app: mynginxdeployment
    spec:
      containers:
      - name: nginx
        image: nginx
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Desired : Display the number of desired running Pods, similar to a ReplicaSet output
* Current : Display the number of current running Pods, similar to a ReplicaSet output
* Up-to-date : Display the number of up to date Pods when a Deployment is updated
* Available : Display the number of production ready Pods, similar to a ReplicaSet output

#### Exercise n°1

Get a list of the existing deployment object in the default namespace.

```bash
kubectl get deployment
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Deployment\(s\) \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the Deployment\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

```bash
kubectl describe deployment SERVICE_NAME
```

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Deleting a Deployment will automatically delete each resources associated like Pods and ReplicaSet.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

```bash
kubectl delete deployment SERVICE_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

