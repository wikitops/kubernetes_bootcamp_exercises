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

Looks up a Deployment, ReplicaSet, or ReplicationController by name and creates an autoscaler that uses the given resource as a reference. An autoscaler can automatically increase or decrease number of pods deployed within the system as needed.

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

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* name : The name for the newly created object.
* reference : The object managed by the autoscaler, like  Pod name, a Deployment name ...
* targets : The metrics defined to autoscale the referenced resource
* minpods : The lower limit for the number of pods that can be set by the autoscaler.
* maxpods : The upper limit for the number of pods that can be set by the autoscaler.
* replicas : Current replicas number
* age : the age of the object from his creation

#### Exercise n°1

Get the current HorizontalPodAutoscaler resources in the default namespace.

```text
kubectl get hpa
```

#### Exercise n°2

Stress the Pod created in the previous section and check the HorizontalPoMindAutoscaler associated.

```bash
# Connect to the Pod
kubectl run -i --tty load-generator --image=busybox /bin/sh

# Run a loop bash command in the container to stress the CPU
while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done

# Check the Horizontal Pod Autoscaler status
kubectl get hpa
```

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
kubectl explain hpa.spec
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

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to dynamically and automatically manage the number of Pods needed to handle the workload.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/09_autoscaling`

{% tabs %}
{% tab title="Exercise" %}
1. Manage the HorizontalPodAutoscaler of the worker Pods to :
   1. Ensure that the worker has minimum one Pods
   2. Ensure that the worker has maximum five Pods
   3. Ensure that the Pods is autoscaled when the CPU is above 80%.
{% endtab %}

{% tab title="Solution" %}
Create the HorizontalPodAutoscaler to manage the worker workload.

```bash
kubectl autoscale deployment worker -n voting-app --cpu-percent=80 --min=1 --max=5
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [Horizontal Pod Auto Scaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) \(HPA\)
* Kubernetes official documentation [walkthrough HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
* Kubernetes official documentation of [autoscale API](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale)



