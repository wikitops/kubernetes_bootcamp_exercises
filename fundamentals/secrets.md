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

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in an image, putting it in a Secret object allows for more control over how it is used, and reduces the risk of accidental exposure.

The _create_ command can directly ask the API resource to create a Secret in command line or create a Secret object based on a yaml file definition.

The creation method of Secrets is similar to ConfigMap object. It can be created from a literal value, from files or from directories. The difference is that the content in the files and the directories has to be encoded in base64 before creating the Secret object otherwise the Kubernetes API will not be able to create it and an explicit error will be displayed.

#### Exercise n°1

Create a Secrets in command line from a literal value.

```bash
kubectl create secret generic SECRET_NAME --from-literal=KEY=VALUE
```

#### Exercise n°2

Create a Secrets in declarative mode with YAML file.

```bash
kubectl create secrets ...
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Name : The name of the Secrets object needed to attach it to a Pod
* Type : The type of Secrets, indication of type of content in the object
* Data : The count of keys in the Secrets
* Age : Duration from the creation of the object

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

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain secrets.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A Secrets can only be deleted when it is not attached to a Pod.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous Secrets created.

```bash
kubectl delete secrets SECRETS_NAME
```

## Module exercise

Based on your reads, try to do it as simple as possible.

{% tabs %}
{% tab title="Exercise" %}
1.
{% endtab %}

{% tab title="Resolution" %}
```bash

```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

