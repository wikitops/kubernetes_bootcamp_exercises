---
description: >-
  A ReplicaSet ensures that a specified number of pod replicas are running at
  any given time.
---

# ReplicaSet

## Module Overview

At the end of this module, you will :

* _Learn what is a Replicaset_
* _Learn how to scale a Pod_
* _Understand why replication is part of High Availability_

## Create

A ReplicaSet can be easily scaled up or down by simply updating the .spec.replicas field in a YAML file definition. The ReplicaSet controller ensures that a desired number of pods with a matching label selector are available and operational

The _create_ command create a ReplicaSet object based on a yaml file definition to manage the number of a pods identified by his labels.

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

List the ReplicaSet created in the namespace app-demo.

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

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain replicaset.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Scale

Some Kubernetes objects like Deployment, ReplicaSet, Replication Controller, or Job can be easily scaled in command line.

The command line management has to be used only for debug purpose. In production environment, it's recommended to update the YAML file definition to ensure a consistent deployment.

#### Exercise n°1

Scale the Nginx Pod in the namespace app-demo to 5 with the command line.

```bash
kubectl scale --replicas=5 -n app-demo POD_NAME
```

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Deleting a ReplicaSet will automatically delete each Pods associated with.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the ReplicaSet of the Nginx Pod in the namespace app-demo.

```bash
kubectl delete replicaset REPLICASET_NAME -n app-demo
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to extract each part of the Voting App in a distinct Pod and manage it with a ReplicaSet.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/02_replicas`

{% tabs %}
{% tab title="Exercise" %}
1. Delete the Pods created in the previous module exercise
2. Create a ReplicaSet to replicate the worker Pod to 1
3. Ensure the Pod is replicate to 1 and it is up and running
4. Scale the worker Pods to 3 in command line
5. Ensure the worker Pod is  the only Pods replicate to 3 and it is up and running
{% endtab %}

{% tab title="Solution" %}
Delete the previous Pods created

```bash
kubectl delete -f /data/votingapp/01_pods/pods.yaml
```

Yaml file definition to deploy the Voting App ReplicaSet.

{% code-tabs %}
{% code-tabs-item title="/data/votingapp/02\_replicas/replicaset.yaml" %}
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: db
  labels:
    app: voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
      - name: db
        image: postgresql
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: redis
  labels:
    app: voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: vote
  labels:
    app: voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: vote
  template:
    metadata:
      labels:
        app: vote
    spec:
      containers:
      - name: vote
        image: vote
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: result
  labels:
    app: voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: result
  template:
    metadata:
      labels:
        app: result
    spec:
      containers:
      - name: result
        image: result
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: worker
  labels:
    app: voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: worker
  template:
    metadata:
      labels:
        app: worker
    spec:
      containers:
      - name: worker
        image: worker
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Kubectl command to deploy the Pods based on the previous definition file.

```text
kubectl create -f /data/votingapp/02_replicas/replicaset.yaml
```

Ensure the Pods is up and running.

```bash
kubectl get pods,replicaset -n voting-app
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [replicaset](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

