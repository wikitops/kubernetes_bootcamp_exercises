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

## Run

Why ? What ? How ?

#### Exercise n°1

Run a busybox pod on the default namespace.

```bash
$ kubectl run busybox --image=busybox -n default
```

## Create

How to deploy ? CLI ? Dashboard ? Difference with Run ?

#### Exercise n°1

Create a pod that contain a single nginx container with a simple YAML file.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mysingleapp
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

Create a pod that contain those containers : nginx, redis, postgres with a single YAML file.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mymultiapp
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

## List

Objectives ? Filter ? Format ?

#### Exercise n°1

List pods deployed on the current namespace

```bash
$ kubectl get pods
```

#### Exercise n°2

List pods deployed on the namespace _app-demo_.

```bash
$ kubectl get pods -n app-demo
```

#### Exercise n°3

List all pods in all namespaces.

```bash
$ kubectl get pods --all-namespaces
```

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

## Delete

Why ? What ? How ?

#### Exercise n°1

Delete the busybox pod deployed previously in the default namespace.

```bash
$ kubectl delete pods busybox -n default
```



