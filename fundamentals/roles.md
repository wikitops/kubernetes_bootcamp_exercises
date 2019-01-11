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

Kubernetes role-based __access __control \(RBAC\) system lets exercise fine-grained control over how users access the API resources running on a cluster. RBAC can be use to dynamically configure permissions for the cluster's users and define the kinds of resources with which users can interact.

RBAC allow to create permissions that apply to an entire cluster, or to specific namespaces within a cluster. Cluster-wide permissions are useful for limiting certain users' access to specific API resources \(such as security policies or Secrets\). Namespace-specific __permissions are useful when multiple groups of users operating within their own respective namespaces. RBAC can help to ensure that users only have access to cluster resources within their own namespace.

RBAC uses the `rbac.authorization.k8s.io` API group to drive authorization decisions, allowing admins to dynamically configure policies through the Kubernetes API.

To be able to work with RBAC on a Kubernetes cluster, the apiserver has to be started with `--authorization-mode=RBAC`

The _create_ command can directly ask the API resource to create a Roles / Rolebinding in command line or create a Roles / Rolebindng object based on a yaml file definition.

#### Exercise n°1

Create a Role in the default namespace to grant read access to pods.

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

#### Exercise n°2

Create a ClusterRole to grant read access to secrets in any particular namespace.

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

#### Exercise n°3

Create a RoleBinding to grant the "pod-reader" role to a user "jane" within the default namespace.

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: jane
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

#### Exercise n°4

Create a RoleBinding to allow any user in the group "manager" to read secrets in any namespace.

```yaml
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: manager
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

#### Roles

The default output display some useful information about each services :

* name : the name of the newly created resource
* age : the age from the object creation

#### Exercise n°1

List the existing Roles and Rolebinding in the default namespace.

```bash
# List the roles
kubectl get roles

# List the rolebinding
kubectl get rolebinding
```

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
# Explain roles specifications
kubectl explain roles.spec

# Explain rolebinding specifications
kubectl explain rolebinding.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A role can be deleted only if it is not used by a running Kubernetes resource.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous role in command line.

```bash
# Delete a rolebinding
kubectl delete rolebinding ROLEBINDING_NAME

# Delete a role
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

