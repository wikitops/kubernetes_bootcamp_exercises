---
description: >-
  A StorageClass provides a way to describe the “classes” of storage to offer.
  Different classes might map to quality-of-service levels, or to backup
  policies, or to arbitrary policies.
---

# StorageClass

## Module Overview

At the end of this module, you will :

* _Learn to persist data thanks to a dynamic volumes_
* _Learn to manage the volumes and the claim_
* _Learn to access data within a container_

## Create

## Get

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the StorageClass \(labels, annotations, etc.\) and the class definition depending on the external storage service \(provider, access, type ...\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing StorageClass in the default namespace.

```bash
kubectl describe storageclass STORAGECLASS_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain storageclasses.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A StorageClass can only be deleted if it is not used by a running storage object.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous storage class in command line.

```bash
kubectl delete storageclasses STORAGECLASS_NAME
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

* Kubernetes official documentation on [storageclass](https://kubernetes.io/docs/concepts/storage/storage-classes/)

