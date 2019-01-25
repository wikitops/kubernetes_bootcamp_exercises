---
description: >-
  Kubernetes networks define how the pods in the same namespace will communicate
  with each other and the network endpoint.
---

# Networks

## Module Overview

At the end of this module, you will :

* _Learn what is a CNI_
* _Learn to manage network rules_
* _Learn to secure the communication within a cluster_

## Create

A network policy is a specification of how groups of pods are allowed to communicate with each other and other network endpoints.

NetworkPolicy resources use labels to select pods and define rules which specify what traffic is allowed to the selected pods.

By default, if no policies exist in a namespace, then all ingress and egress traffic is allowed to and from pods in that namespace

The _create_ command can create a NetworkPolicy object based on a yaml file definition.

#### Exercise n°1

Deploy a default Network Policy for each resources in the default namespace to deny all ingress and egress traffic.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: defaultNetworkPolicy
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* name : the name of the newly created resource
* pod-selector : the labels used to associate the Network Policy with the Pods
* age :  the age from the resource creation

#### Exercise n°1

```bash
kubectl get networkpolicy
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the NetworkPolicies \(labels, annotations, etc.\) and the specific policies associated to the current NetworkPolicy \(type, port, selectors, etc\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing Network Policy in the default namespace.

```bash
kubectl describe networkpolicy NETWORK_POLICY_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain networkpolicy.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Be careful on the deletion of a network policy, this can isolate resource or expose it and cause damaged to your services.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous network policy in command line.

```bash
kubectl delete networkpolicy NETWORKPOLICY_NAME
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to secure the communication between Pods.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/10_networks`

{% tabs %}
{% tab title="Exercise" %}
1.
{% endtab %}

{% tab title="Solution" %}
```bash

```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

