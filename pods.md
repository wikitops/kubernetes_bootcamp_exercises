---
description: >-
  Pods are the smallest deployable units of computing that can be created and
  managed in Kubernetes.
---

# Pods

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML file_
* _Learn how to manage a Pods_
* _Learn how to manage containers_

## Get

The get command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

#### Exercise n°1

List pods deployed on the current namespace.

```bash
$ kubectl get pods
```

#### Exercise n°2

List pods deployed on the namespace _app-demo_.

```text
$ kubectl get pods -n app-demo
```

#### Exercise n°3

List all pods in all namespaces.

```text
$ kubectl get pods --all-namespaces
```

#### Exercise n°4

List all pods, services, deployments in the _kube-system_ namespace.

```bash
$ kubectl get pods,service,deployment -n kube-system
```

## Run

The _run_ command creates a deployment based on the parameters specified, such as the image or replicas. This deployment is issued to the Kubernetes master which launches the Pods and containers required. Kubectl run is similar to _docker run_ but at a cluster level.

The format of the command is _kubectl run &lt;name of deployment&gt; &lt;properties&gt;_

#### Exercise n°1

Run a busybox pod on the default namespace.

```bash
$ kubectl run busybox --image=busybox -n default
```

List everything created by the previous command.

```bash
$ kubectl get all -n default
```

## Create

In imperative object configuration, the kubectl command specifies the operation \(create, replace, etc.\), optional flags and at least one file name. The file specified must contain a full definition of the object in YAML or JSON format.

#### Exercise n°1

Create a pod that contain a single nginx container with a simple YAML file.

{% hint style="info" %}
The file created in this exercise will be reused, do not delete it.
{% endhint %}

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-single-app
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx
```

```bash
$ kubectl create -f FILENAME
```

#### Exercise n°2

Do the same things using another syntax.

```bash
$ kubectl create deployment busybox --image busybox
```

#### Exercise n°3

Create a pod that contain those containers : nginx, redis, postgres with a single YAML file.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-multi-app
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx
  - name: postgres
    image: postgres
  - name: rabbitmq
    image: rabbitmq
```

```bash
$ kubectl create -f FILENAME
```

## Apply

When using declarative object configuration, a user operates on object configuration files stored locally, however the user does not define the operations to be taken on the files. Create, update, and delete operations are automatically detected per-object by kubectl. This enables working on directories, where different operations might be needed for different objects.

#### Exercise n°1

Update the image version of the previous nginx pods.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-single-app
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx:1.14-alpine
```

```bash
$ kubectl apply -f FILENAME
```

Objectives ? Filter ? Format ?

## Describe

Why ? What ? How ?

#### Exercise n°1

Get the information of the pod nginx deployed on the namespace app-demo

```bash
$ kubectl describe po nginx -n app-demo
```

## Connect

Why ? What ? How ?

#### Exercise n°1

Connect to the pod nginx in the namespace app-demo.

```bash
$ kubectl exec -it nginx -n app-demo
```

#### Exercise n°2

Connect to the postgres container in the myapp pod in the namespace app-demo

```bash
$ kubectl exec -it -c postgres myapp -n app-demo
```

## Edit

What ?

#### Exercise n°1

Edit the nginx pods to update the image.

```bash
$ kubectl edit POD_NAME
```

## Delete

Why ? What ? How ?

#### Exercise n°1

Delete the busybox pod deployed previously in the default namespace.

```bash
$ kubectl delete pods busybox -n default
```

#### Exercise n°2

Delete the nginx pod deployed previously in the default namespace with declarative method.

```bash
$ kubectl delete -f FILENAME
```

## External documentation

To go further in the management of Pods, please refer to those documentations :

* Kubernetes Official [cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) documentation
* Kubernetes Official documentation on [object management](https://kubernetes.io/docs/concepts/overview/object-management-kubectl/overview/)
* Kubernetes Official documentation on [list pods](https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/)



