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

Horizontal Pod Autoscaler automatically scales the number of pods in a deployment or replica set based on observed CPU, Memory or Custom Metrics utilization depending the API version used.

The Kubernetes basic autoscaling architecture can be schematized like this :

![](../.gitbook/assets/autoscaling_architecture.png)

The _create_ command can directly ask the API resource to create an HorizontalPodAutoscaler in command line or create an HorizontalPodAutoscaler object based on a yaml file definition.

#### Exercise n°1

1. Run a sample app based on a webserver to expose it on port 80.
2. Create an Horizontal Pod Autoscaler to automatically scale the Deployment if the CPU usage is above 50%.

```bash
# Run a sample app in the default namespace
kubectl run php-apache --image=k8s.gcr.io/hpa-example --requests=cpu=200m --expose --port=80

# Create an Horizontal Pod Autoscaler based on the CPU usage
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

## Get

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Horizontal Pod Autoscaler \(labels, annotations, etc.\) and the scale policy \(selector, type, number of pods, ...\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing Autoscaler in the default namespace.

```bash
kubectl describe horizontalpodautoscaler HPA_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain autoscaling.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Be careful on the deletion of an autoscaling object, this can have effects in the availability of the services associated.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous autoscaling group in command line.

```bash
kubectl delete hpa AUTOSCALE_NAME
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

* Kubernetes official documentation on [Horizontal Pod Auto Scaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) \(HPA\)
* Kubernetes official documentation [walkthrough HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
* Kubernetes official documentation of [autoscale API](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale)



