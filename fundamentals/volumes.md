---
description: >-
  A Kubernetes volume has an explicit lifetime, the same as the pod that
  encloses it. A volume outlives any containers that run within the pod, and
  data is preserved across container restarts.
---

# Volumes

## Module Overview

At the end of this module, you will :

* _Learn to persist data thanks to external volumes_
* _Learn to manage the volumes and the claim_
* _Learn to access data within a container_

## Create

## Get

## Describe

## Attach to a pod

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A Volume can only be deleted if it is not attached to a Pod. Use with caution, a deleted volumes cannot be recover.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

**Exercise n°1**

Delete the previous volumes deployed in the default namespace.

```bash
kubectl delete persistentvolume VOLUME_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [volumes](https://kubernetes.io/docs/concepts/storage/volumes/)



