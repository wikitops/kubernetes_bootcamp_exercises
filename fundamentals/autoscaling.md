---
description: >-
  Autoscaling is a feature in which the cluster is capable of increasing or
  decreasing the number of pods as the demand for service response need it.
---

# Autoscaling

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML Autoscale file_
* _Learn how to manage a Autoscale_
* _Learn the composition of a Autoscale_

## Create

## Get

## Describe

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. This can be fin easily in command line or in the official Kubernetes documentation.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain autoscaling.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

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

* Kubernetes official documentation on [Horizontal Pod Auto Scaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) \(HPA\)
* Kubernetes official documentation [walkthrough HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
* Kubernetes official documentation of [autoscale API](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale)
