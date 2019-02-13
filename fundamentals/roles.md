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

```bash
kubectl create role pods-reader --verb=get,list,watch --resource=pods
```

#### Exercise n°2

Create a ClusterRole to grant read access to secrets in any particular namespace.

```bash
kubectl create clusterrole secrets-reader --verb=get,list,watch --resource=secrets
```

#### Exercise n°3

Create a RoleBinding to grant the "pod-reader" role to a user "jane" within the default namespace.

```bash
kubectl create rolebinding read-pods --user=jane --role=pods-reader
```

#### Exercise n°4

Create a RoleBinding to allow any user in the group "manager" to read secrets in any namespace.

```bash
kubectl create clusterrolebinding read-secrets --group=manager --clusterrole=secrets-reader
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

#### Roles

The default output display some useful information about each services :

* Name : the name of the newly created resource
* Age : the age since his creation

#### Exercise n°1

List the existing Roles and Rolebinding in the default namespace.

{% tabs %}
{% tab title="Command" %}
```bash
# List the roles
kubectl get roles

# List the rolebinding
kubectl get rolebinding
```
{% endtab %}

{% tab title="CLI Return" %}
List of roles created in command line.

```bash
NAME         AGE
pods-reader   8m
```

List of roles binding created in command line.

```bash
NAME        AGE
read-pods   10m
```
{% endtab %}
{% endtabs %}

#### Exercise n°2

List the existing ClusterRoles and ClusterRolebinding.

{% tabs %}
{% tab title="Command" %}
```bash
# List the cluster roles
kubectl get clusterroles

# List the cluster roles binding
kubectl get clusterrolebinding
```
{% endtab %}

{% tab title="CLI Return" %}
List all the cluster roles created by default and in command line.

```bash
NAME                                                                   AGE
admin                                                                  20m
cluster-admin                                                          20m
edit                                                                   20m
secrets-reader                                                          14m
system:aggregate-to-admin                                              20m
system:aggregate-to-edit                                               20m
system:aggregate-to-view                                               20m
system:auth-delegator                                                  20m
system:aws-cloud-provider                                              20m
system:basic-user                                                      20m
system:certificates.k8s.io:certificatesigningrequests:nodeclient       20m
system:certificates.k8s.io:certificatesigningrequests:selfnodeclient   20m
system:controller:attachdetach-controller                              20m
system:controller:certificate-controller                               20m
system:controller:clusterrole-aggregation-controller                   20m
system:controller:cronjob-controller                                   20m
system:controller:daemon-set-controller                                20m
system:controller:deployment-controller                                20m
system:controller:disruption-controller                                20m
system:controller:endpoint-controller                                  20m
system:controller:expand-controller                                    20m
system:controller:generic-garbage-collector                            20m
system:controller:horizontal-pod-autoscaler                            20m
system:controller:job-controller                                       20m
system:controller:namespace-controller                                 20m
system:controller:node-controller                                      20m
system:controller:persistent-volume-binder                             20m
system:controller:pod-garbage-collector                                20m
system:controller:pv-protection-controller                             20m
system:controller:pvc-protection-controller                            20m
system:controller:replicaset-controller                                20m
system:controller:replication-controller                               20m
system:controller:resourcequota-controller                             20m
system:controller:route-controller                                     20m
system:controller:service-account-controller                           20m
system:controller:service-controller                                   20m
system:controller:statefulset-controller                               20m
system:controller:ttl-controller                                       20m
system:coredns                                                         20m
system:csi-external-attacher                                           20m
system:csi-external-provisioner                                        20m
system:discovery                                                       20m
system:heapster                                                        20m
system:kube-aggregator                                                 20m
system:kube-controller-manager                                         20m
system:kube-dns                                                        20m
system:kube-scheduler                                                  20m
system:kubelet-api-admin                                               20m
system:nginx-ingress                                                   19m
system:node                                                            20m
system:node-bootstrapper                                               20m
system:node-problem-detector                                           20m
system:node-proxier                                                    20m
system:persistent-volume-provisioner                                   20m
system:volume-scheduler                                                20m
view                                                                   20m
```

List all the cluster roles binding created by default and in command line.

```bash
NAME                                                   AGE
cluster-admin                                          19m
kubeadm:kubelet-bootstrap                              19m
kubeadm:node-autoapprove-bootstrap                     19m
kubeadm:node-autoapprove-certificate-rotation          19m
kubeadm:node-proxier                                   19m
minikube-rbac                                          19m
read-secrets-global                                    13m
storage-provisioner                                    19m
system:aws-cloud-provider                              19m
system:basic-user                                      19m
system:controller:attachdetach-controller              19m
system:controller:certificate-controller               19m
system:controller:clusterrole-aggregation-controller   19m
system:controller:cronjob-controller                   19m
system:controller:daemon-set-controller                19m
system:controller:deployment-controller                19m
system:controller:disruption-controller                19m
system:controller:endpoint-controller                  19m
system:controller:expand-controller                    19m
system:controller:generic-garbage-collector            19m
system:controller:horizontal-pod-autoscaler            19m
system:controller:job-controller                       19m
system:controller:namespace-controller                 19m
system:controller:node-controller                      19m
system:controller:persistent-volume-binder             19m
system:controller:pod-garbage-collector                19m
system:controller:pv-protection-controller             19m
system:controller:pvc-protection-controller            19m
system:controller:replicaset-controller                19m
system:controller:replication-controller               19m
system:controller:resourcequota-controller             19m
system:controller:route-controller                     19m
system:controller:service-account-controller           19m
system:controller:service-controller                   19m
system:controller:statefulset-controller               19m
system:controller:ttl-controller                       19m
system:coredns                                         19m
system:discovery                                       19m
system:kube-controller-manager                         19m
system:kube-dns                                        19m
system:kube-scheduler                                  19m
system:nginx-ingress                                   19m
system:node                                            19m
system:node-proxier                                    19m
system:volume-scheduler                                19m
```
{% endtab %}
{% endtabs %}

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Roles \(labels, annotations, etc.\) and the binding policy \(resources associated, resources name, verbs ...\)

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe the roles pod-reader previously created and his binding read-pods.

{% tabs %}
{% tab title="Command" %}
```bash
# Describe a role
kubectl describe role

# Describe the rolebinding associated
kubectl describe rolebinding
```
{% endtab %}

{% tab title="CLI Return" %}
Describe the roles previously created in command line.

```bash
Name:         pods-reader
Labels:       <none>
Annotations:  <none>
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  pods       []                 []              [get list watch]
```

Describe the role binding previously created in command line.

```bash
Name:         read-pods
Labels:       <none>
Annotations:  <none>
Role:
  Kind:  Role
  Name:  pod-reader
Subjects:
  Kind  Name  Namespace
  ----  ----  ---------
  User  jane 
```
{% endtab %}
{% endtabs %}

#### Exercise n°2

Describe the roles secrets-reader previously created and his binding read-secrets.

{% tabs %}
{% tab title="Command" %}
```bash
# Describe a clusterrole
kubectl describe clusterrole secrets-reader

# Describe the clusterrolebinding associated
kubectl describe clusterrolebinding reader-secrets
```
{% endtab %}

{% tab title="CLI Return" %}
Describe the cluster roles previously created in command line.

```bash
Name:         secrets-reader
Labels:       <none>
Annotations:  <none>
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  secrets    []                 []              [get list watch]
```

Describe the cluster roles binding previously created in command line.

```c
Name:         read-secrets
Labels:       <none>
Annotations:  <none>
Role:
  Kind:  ClusterRole
  Name:  secret-reader
Subjects:
  Kind   Name     Namespace
  ----   ----     ---------
  Group  manager  
```
{% endtab %}
{% endtabs %}

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a role resource.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl explain roles
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
KIND:     Role
VERSION:  rbac.authorization.k8s.io/v1

DESCRIPTION:
     Role is a namespaced, logical grouping of PolicyRules that can be
     referenced as a unit by a RoleBinding.

FIELDS:
   apiVersion	<string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/api-conventions.md#resources

   kind	<string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/api-conventions.md#types-kinds

   metadata	<Object>
     Standard object's metadata.

   rules	<[]Object> -required-
     Rules holds all the PolicyRules for this Role
```
{% endtab %}
{% endtabs %}

Add the --recursive flag to display all of the fields at once without descriptions.

#### Exercise n°2

Get the documentation of a specific field of a rolebinding resource.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl explain rolebinding
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
KIND:     RoleBinding
VERSION:  rbac.authorization.k8s.io/v1

DESCRIPTION:
     RoleBinding references a role, but does not contain it. It can reference a
     Role in the same namespace or a ClusterRole in the global namespace. It
     adds who information via Subjects and namespace information by which
     namespace it exists in. RoleBindings in a given namespace only have effect
     in that namespace.

FIELDS:
   apiVersion	<string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/api-conventions.md#resources

   kind	<string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/api-conventions.md#types-kinds

   metadata	<Object>
     Standard object's metadata.

   roleRef	<Object> -required-
     RoleRef can reference a Role in the current namespace or a ClusterRole in
     the global namespace. If the RoleRef cannot be resolved, the Authorizer
     must return an error.

   subjects	<[]Object>
     Subjects holds references to the objects the role applies to.
```
{% endtab %}
{% endtabs %}

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A role can be deleted only if it is not used by a running Kubernetes resource.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous role in command line.

```bash
# Delete a rolebinding
kubectl delete rolebinding read-pods
kubectl delete clusterrolebinding read-secrets

# Delete a role
kubectl delete roles pod-reader
kubectl delete clusterroles secret-reader
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to secure the internal access to respect the least privileges principles.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/15_roles`

{% tabs %}
{% tab title="Exercise" %}
1. Create a votingapp called vote in the namespace voting-app to limit his access to :
   1. Get, list, watch Secrets
   2. Get, list, watch ConfigMaps
   3. Get, list, watch PersistentVolumes and PersistentVolumeClaims
2. Bind the role votingapp to the service account vote and result
{% endtab %}

{% tab title="Solution" %}
Create the votingapp role based on the prerequisites.

```bash
kubectl create role votingapp --verb=get,list,watch --resource=secrets,configmaps,persistentvolume,persistentvolumeclaim -n voting-app
```

Bind the votingapp role to the vote and result service accounts.

```bash
kubectl create rolebinding votingapp --user=vote,result --role=votingapp -n voting-app
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

