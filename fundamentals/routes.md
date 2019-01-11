---
description: >-
  Kubernetes have advanced networking capabilities that allow Pods and Services
  to communicate inside and outside the cluster's network.
---

# Route

## Module Overview

At the end of this module, you will :

* _Learn to manage the external access of internal resources_
* _Learn to manage ingress controller_
* _Learn to secure the cluster access_

## Create

Ingress exposes HTTP and HTTPS routes from outside the cluster to Services within the cluster. Traffic routing is controlled by rules defined on the ingress resource.

An ingress can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL, and offer name based virtual hosting. An ingress controller is responsible for fulfilling the ingress, usually with a loadbalancer, though it may also configure your edge router or additional frontends to help handle the traffic.

The _create_ command can create a Ingress object based on a yaml file definition.

#### Exercise n°1

Create an Ingress resource to expose an Nginx pod Service's on port 80.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myfirstingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /foo
        backend:
          serviceName: <service_name>
          servicePort: 80
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* name : the name of the newly created resource
* hosts : the host to apply the newly created resource
* address : the address exposed by the newly created resource
* ports : the ports exposed by the resource
* age : the age since the creation of the resource

#### Exercise n°1

List the current Ingress resources created.

```bash
kubectl get ingress
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Ingress \(labels, annotations, events, backend associated, IP address associated, etc.\) and the rules available for each ingress \(hosts, path, backend associated\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing Ingress in the default namespace.

```bash
kubectl describe ingress INGRESS_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain ingresses.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Be careful on the deletion of a Ingress object. This will interupt existing communication based on this resource.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous ingress in command line.

```bash
kubectl delete ingress INGRESS_NAME
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

* Kubernetes official documentation on [ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress/)

