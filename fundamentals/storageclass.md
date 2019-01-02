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

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. This can be fin easily in command line or in the official Kubernetes documentation.

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

