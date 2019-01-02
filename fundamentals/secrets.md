---
description: >-
  Objects of type secret are intended to hold sensitive information, such as
  passwords, OAuth tokens, and ssh keys.
---

# Secrets

## Module Overview

At the end of this module, you will :

* _Learn to manage sensitive data within the cluster_
* _Learn to deploy Secrets_
* _Learn to access secrets data within a container_

## Create

#### Exercise n°1

Create a Secrets in declarative mode with YAML file.

```bash
kubectl apply -f FILENAME
```

#### Exercise n°2

Create a Secrets in command line.

```bash
kubectl create secrets ...
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* 
#### Exercise n°1

Get a list of existing Secrets in the default namespace.

```bash
kubectl get secrets
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Secrets \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the Secrets and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing Secrets in the default namespace.

```bash
kubectl describe secrets SECRETS_NAME
```

## Attach

### In environment variable

### In file path

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A Secrets can only be deleted when it is not attached to a Pod.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous Secrets created.

```bash
kubectl delete secrets SECRETS_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)



