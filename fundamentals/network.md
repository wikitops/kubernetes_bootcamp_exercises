---
description: >-
  Kubernetes networks define how the pods in the same namespace will communicate
  with each other and the network endpoint.
---

# Network

## Module Overview

At the end of this module, you will :

* _Learn what is a CNI_
* _Learn to manage network rules_
* _Learn to secure the communication within a cluster_

## Create

## Get

## Describe

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. This can be fin easily in command line or in the official Kubernetes documentation.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain networkpolicies.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Be careful on the deletion of a network policy, this can isolate resource or expose it and cause damaged to your services.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous network policy in command line.

```bash
kubectl delete networkpolicies NETWORKPOLICY_NAME
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

* Kubernetes official documentation on [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

