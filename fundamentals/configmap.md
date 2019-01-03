---
description: >-
  ConfigMaps allow you to decouple configuration artifacts from image content to
  keep containerized applications portable.
---

# ConfigMaps

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML ConfigMaps file_
* _Learn how to manage a ConfigMaps_
* _Learn the composition of a ConfigMaps_

## Create

ConfigMap can be created from directories, files, or literal values thanks to the Kubectl client.

Independently from the deployment method, the command line to create a ConfigMaps should look like this :

```bash
kubectl create configmap CONFIGMAP_NAME DATA_SOURCE
```

* CONFIGMAP\_NAME is the name to assign to the ConfigMap 
* DATA\_SOURCE is the directory, file, or literal value to draw the data from

The data source corresponds to a key-value pair in the ConfigMap, where

* key is the file name or the key provided on the command line
* value is the file contents or the literal value provided on the command line

### From literal values

Multiple key-value pairs can be passed to the Kubectl client to create a single ConfigMap. Each pair provided on the command line is represented as a separate entry in the data section of the ConfigMap.

In this particular method, the key / value are explicitly defined in the command line and each key / value will generate a new entry in the ConfigMap.

The command line for this kind of ConfigMaps creation should look like this :

```bash
kubectl create configmap CONFIGMAP_NAME --from-literal=KEY_NAME=VALUE
```

#### Exercise n°1

Create a ConfigMap named MySimpleConfigMap with those key / value pairs :

* course=kubernetes
* subject=configmap
* subject.title=MySimpleConfigMap

```bash
kubectl create configmap MySimpleConfigMap --from-literal=course=kubernetes --from-literal=subject=configmap --from-literal=subject.title=MySimpleConfigMap
```

{% hint style="info" %}
This method is not recommended in production. It is recommended to version the ConfigMaps files in a sources repository to manage it with a CI/CD tool.
{% endhint %}

### From a directory

Like any other object in Kubernetes, multiple ConfigMap can be created based on files stocked in a directory. The Kubectl client take care to parse each file in the directory given to ensure that each content files are integrated as ConfigMaps.

In this particular method, the key of the ConfigMap will be the name of the file and the value will be the entire content of the file.

The command line for this kind of ConfigMaps creation should look like this :

```bash
kubectl create configmap CONFIGMAP_NAME --from-directory=PATH_TO_DIRECTORY
```

#### Exercise n°1

1. Create a directory
2. Create a properties file in this directory
3. Copy / paste the content below in the new file
4. Create a ConfigMap based on the directory

```bash
course=kubernetes
subject=configmaps
subject.title=My first ConfigMap
```

### From a file

Like any other object in Kubernetes, multiple ConfigMap can be created based on multiple files stocked in a different directories. The Kubectl client take care to parse each file given to the command line to ensure that each content files are integrated as ConfigMaps.

In this particular method, the key of the ConfigMap will be the name of the file and the value will be the entire content of the file. In this method, it is possible to specify a key at the ConfigMap creation to avoid using the file name.

The command line for this kind of ConfigMaps creation should look like this :

```bash
# Without key specification
kubectl create configmap CONFIGMAP_NAME --from-file=PATH_TO_FILENAME

# With key specification
kubectl create configmap CONFIGMAP_NAME --from-file=KEY_NAME=PATH_TO_FILENAME
```

#### Exercise n°1

1. Create two files
2. Put some properties in each one
3. Create a ConfigMap based on the two property files created

```bash
kubectl create configmap CONFIGMAP_NAME --from-file=PATH_FILE1 --from-file=PATH_FILE2
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some JSON search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each configMaps :

\*

#### Exercise n°1

Get a list of ConfigMaps deployed in the default namespace.

```bash
kubectl get configmaps
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the ConfigMaps \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the ConfigMaps and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the ConfigMaps deployed in the default namespace.

```bash
kubectl describe configmaps CONFIGMAPS_NAME
```

## Attach

### In environment variable

### In file path

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. This can be fin easily in command line or in the official Kubernetes documentation.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain configmaps.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A ConfigMaps can only be deleted when it is not attached to a Pod.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous ConfigMaps created.

```bash
kubectl delete configmaps CONFIGMAPS_NAME
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

* Kubernetes official documentation on [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

