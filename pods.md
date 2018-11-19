---
description: This section will give you some information about the pods management.
---

# Pods

## YAML file

What is YAML ? Structure ? Pros and Cons ? AKMS ?

#### Exercise n°1

Develop your first pod yaml file deployment

```text
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx
```

## Multi containers

What is a pod ? a container ? Difference ?

#### Exercise n°1

Deploy a pod with 3 containers : nginx, postgres, rabbitmq

```text
apiVersion: v1
kind: Pod
metadata:
  name: myapp
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

## Create

How to deploy ? CLI ? Dashboard ? Run vs deploy ?

#### Exercise n°1

Run your first pod

```text
kubectl run mytomcat --image=tomcat:8.0.52-jre8
```

#### Exercise n°2

Deploy your first app with the CLI command and a file

```text
kubectl create -f FILENAME
```

## List

Objectives ? Filter ? Format ? Contraction namespace = ns

#### Exercise n°1

List pods deployed on the current namespace

```text
kubectl get pods
```

#### Exercise n°2

List pods deployed on a specific namespace

```text
kubectl get pods -n app-demo
```

#### Exercise n°3

List all pods in all namespaces

```text
kubectl get pods --all-namespaces
```

## Describe

Why ? What ? How ?

#### Exercise n°1

Get the information of the pod nginx deployed on the namespace app-demo

```text
kubectl describe po nginx -n app-demo
```

## Connect

Why ? What ? How ?

#### Exercise n°1

Connect to the pod nginx in the namespace app-demo.

```text
kubectl exec -it nginx -n app-demo
```

#### Exercise n°2

Connect to the postgres container in the myapp pod in the namespace app-demo

```text
kubectl exec -it -c postgres myapp -n app-demo
```

## Delete

Why ? What ? How ?

#### Exercise n°1

Delete the nginx pod in the app-demo namespace.

```text
kubectl delete pods nginx -n app-demo
```



