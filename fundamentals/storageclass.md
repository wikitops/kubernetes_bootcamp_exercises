# StorageClass

## Module Overview

At the end of this module, you will :

* _Learn to persist data thanks to a dynamic volumes_
* _Learn to manage the volumes and the claim_
* _Learn to access data within a container_

## Create

StorageClasses are the foundation of dynamic provisioning, allowing cluster administrators to define abstractions for the underlying storage platform. Users simply refer to a StorageClass by name in the PersistentVolumeClaim \(PVC\) using the “storageClassName” parameter.

Storage is a critical part of running stateful containers, and Kubernetes offers powerful primitives for managing it. Dynamic volume provisioning, a feature unique to Kubernetes, allows storage volumes to be created on-demand. The storage resources can be dynamically provisioned using the provisioner specified by the StorageClass object. 

StorageClasses are essentially blueprints that abstract away the underlying storage provider, as well as other parameters, like disk-type.

The _create_ command can create a StorageClass object based on a yaml file definition.

#### Exercise n°1

Create a StorageClass object to automatically use the AWS EBS volumes.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Retain
mountOptions:
  - debug
volumeBindingMode: Immediate
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* name : the name of the newly created resource
* provisioner : the target storage provisioner used to create the futur volumes
* age : the age of the object from his creation

#### Exercise n°1

Get the information of a Storage Class deployed in the default namespace.

```bash
kubectl get storageclass STORAGECLASS_NAME
```

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

## Default Storage Class

The default StorageClass is used to dynamically provision storage for PersistentVolumeClaims that do not require any specific storage class. It simplify the management by using the default Storage Class to create the new Persitent Volumes.

The default Storage Class is defined by an annotation set to true : `storageclass.kubernetes.io/is-default-class`

This parameter can be updated in command line or by editing the yaml file of each Storage Class. If a Storage Class already exist, it is needed to disable the annotation to enable it on another object.

#### Exercise n°1

1. Disable the default StorageClass in the default namespace.
2. Configure the previous StorageClass created as default 

```bash
# Disable the current default StorageClass
kubectl patch storageclass CURRENT_DEFAULT_STORAGECLASS_NAME -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
 
# Configure the new default StorageClass
kubectl patch storageclass NEW_DEFAULT_STORAGECLASS_NAME -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}' 
```

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

