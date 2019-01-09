---
description: >-
  Role-based access control (RBAC) is a method of regulating access to computer
  or network resources based on the roles of individual users within an
  enterprise.
---

# Roles

## Module Overview

At the end of this module, you will :

* _Learn to restrict access of resources_
* _Learn to manage roles_
* _Learn to secure the cluster access_

## Create

## Get

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Roles \(labels, annotations, etc.\) and the binding policy \(resources associated, resources name, verbs ...\)

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing Role in the default namespace.

```bash
# Describe a role
kubectl describe role ROLE_NAME

# Describe the rolebinding object associated
kubectl describe rolebinding ROLEBINDING_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain roles.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A role can be deleted only if it is not used by a running Kubernetes resource.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous role in command line.

```bash
kubectl delete roles ROLE_NAME
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

* Kubernetes official documentation on [RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

