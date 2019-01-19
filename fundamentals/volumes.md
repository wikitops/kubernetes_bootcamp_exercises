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

The Kubernetes basic architecture can be schematized like this :

![Kubernetes volumes architecture](../.gitbook/assets/volumes_architecture.png)

The _create_ command can create a PersistentVolume / PersistentVolumeClaim object based on a yaml file definition.

### EmptyDir

An EmptyDir Volume, as the name says, attach an empty volume to all the containers in a single Pods. 

Containers in the Pod can all read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each Container. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever.

#### Exercise n°1

Create an Nginx Pod and attach an EmptyDir volume to it.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: nginx-storage
      mountPath: /data/test
  volumes:
  - name: nginx-storage
    emptyDir: {}
```

### HostPath

Kubernetes supports hostPath for development and testing on a single-node cluster. A hostPath PersistentVolume uses a file or directory on the Node to emulate network-attached storage.

HostPath volumes should not be used in a production cluster. Instead a cluster administrator would provision a network resource like a Google Compute Engine persistent disk, an NFS share, or an Amazon Elastic Block Store volume. Cluster administrators can also use StorageClasses to set up dynamic provisioning.

#### Exercise n°1

1. Create an HostPath volume based on that directory : /data/nginx/conf
2. Create the PersistentVolumeClaim to claim this volume
3. Attach this volume to an Nginx Pod

```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: myfirsthostpathvolume
spec:
  storageClassName: manual
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/data/nginx/conf"
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myfirsthostpathvolumeclaim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
kind: Pod
apiVersion: v1
metadata:
  name: myfirsthostpathpod
spec:
  volumes:
    - name: myfirsthostpathvolume
      persistentVolumeClaim:
       claimName: myfirsthostpathvolumeclaim
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: myfirsthostpathvolume
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

### PersistentVolume

The default output display some useful information about each services :

* name : the name of the newly created resource
* capacity : the storage capacity available in the volume
* access modes : the access modes available \(read, write, owner\)
* reclaim policy :  the management of the resource when pod is deleted
* status : the status of the resource
* claim : the claim object associated
* storageclass : the storage class used to create the new resource
* reason : the reason of the resource creation
* age : the age since the creation resource 

#### Exercise n°1

List all existing Volumes in the default namespace.

```bash
kubectl get persistentvolume
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Volumes \(labels, annotations, etc.\) and the claim policy of each resources \(storageclass, type, size, access, etc\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise  n°1

Describe one of the existing Volumes in the default namespace.

```bash
# Describe a persistent volume
kubectl describe persistentvolume VOLUME_NAME

# Describe the persistent volume claim associated
kubectl describe persitentvolumeclaim PVC_NAME
```

## Attach

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain persistentvolumes.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A Volume can only be deleted if it is not attached to a Pod. Use with caution, a deleted volumes cannot be recover.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous volumes deployed in the default namespace.

```bash
kubectl delete persistentvolume VOLUME_NAME
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

* Kubernetes official documentation on [volumes](https://kubernetes.io/docs/concepts/storage/volumes/)
* Kubernetes official documentation on [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* Kubernetes official documentation on [how to attach a volume to a pod](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/)

