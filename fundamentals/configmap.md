---
description: >-
  ConfigMaps decouple configuration artifacts from image content to keep
  containerized applications portable.
---

# ConfigMaps

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML ConfigMaps file_
* _Learn how to manage a ConfigMaps_
* _Learn the composition of a ConfigMaps_

## Create

ConfigMaps allow to decouple configuration artifacts from image content to keep containerized applications portable.

### From a directory

### From a file

### From command line

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some JSON search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* 
```bash
kubectl get configmaps
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the ConfigMaps \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the ConfigMaps and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

```bash
kubectl describe configmaps CONFIGMAPS_NAME
```

## Attach to a pod

### In environment variable

### In file path

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A ConfigMaps can only be deleted when it is not attached to a Pod.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

```bash
kubectl delete configmaps CONFIGMAPS_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

