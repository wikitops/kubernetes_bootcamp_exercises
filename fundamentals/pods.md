---
description: >-
  Pods are the smallest deployable units of computing that can be created and
  managed in Kubernetes.
---

# Pods

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML file_
* _Learn how to manage a Pods_
* _Learn how to manage containers_

## Run

The _run_ command creates a Pod based on the parameters specified, such as the image or the environment variables. This deployment is issued to the Kubernetes master which launches the Pods and containers required. Kubectl run is similar to _docker run_ but at a cluster level.

The format of the command is :

```bash
kubectl run <pod_name> <properties>
```

#### Exercise n°1

Run a busybox pod on the default namespace.

```bash
kubectl run busybox --image=busybox -n default
```

## Create

In imperative object configuration, the kubectl command specifies the operation \(create, replace, etc.\), optional flags and at least one file name. The file specified must contain a full definition of the object in YAML or JSON format.

The _create_ command create a resources based on the imperative declaration \(command line or YAML file\).

#### Exercise n°1

Create a pod that contain a single nginx container with a simple YAML file.

{% hint style="info" %}
The file created in this exercise will be reused, do not delete it.
{% endhint %}

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-single-app
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx
```

```bash
kubectl create -f FILENAME
```

#### Exercise n°2

Do the same things using another syntax.

```bash
kubectl create deployment busybox --image busybox
```

#### Exercise n°3

Create a pod that contain multiple containers : nginx, redis, postgres with a single YAML file.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-multi-app
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx
  - name: postgres
    image: postgres
  - name: redis
    image: redis
```

```bash
kubectl create -f FILENAME
```

## Apply

When using declarative object configuration, a user operates on object configuration files stored locally, however the user does not define the operations to be taken on the files. Create, update, and delete operations are automatically detected per-object by kubectl. This enables working on directories, where different operations might be needed for different objects.

The _apply_ command manage the status of resources in the Kubernetes cluster. It can create, update and delete resources based on one or more YAML files.

#### Exercise n°1

Update the image version of the previous nginx pods.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-single-app
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx:1.14-alpine
```

```bash
kubectl apply -f FILENAME
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* name : the name of the newly created resource
* ready : the amount of container ready in the Pods
* staus : the deployment status
* restarts : the count that the Pods has restarted from his creation
* age : the age since the Pods creation

#### Exercise n°1

List pods deployed on the current namespace.

```bash
kubectl get pods
```

#### Exercise n°2

List pods deployed on the namespace _app-demo_.

```text
kubectl get pods -n app-demo
```

#### Exercise n°3

List all pods in all namespaces.

```text
kubectl get pods --all-namespaces
```

#### Exercise n°4

List all pods, services, deployments in the _kube-system_ namespace.

```bash
kubectl get pods,service,deployment -n kube-system
```

## Describe

Once an application is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the container\(s\) and Pod \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the container\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug a container deployed in a Pod.

#### Exercise n°1

Get the information of the pod nginx deployed on the namespace app-demo

```bash
kubectl describe po nginx -n app-demo
```

## Exec

Execute a command in a container is sometimes needed for debug or development purpose.

The _exec_ command manage the SSH connection to a container deployed in a Pod.

Usually, the format is : _kubectl exec -it POD\_NAME -c CONTAINER\_NAME_

#### Exercise n°1

Connect to the pod _nginx_ in the namespace _app-demo_.

```bash
kubectl exec -it nginx -n app-demo
```

#### Exercise n°2

Connect to the postgres container in the _my-multi-app_ pod in the namespace _app-demo._

```bash
kubectl exec -it -c postgres my-multi-app -n app-demo
```

## Edit

Sometimes it’s necessary to make narrow, non-disruptive updates to resources created. It is suggested to maintain a set of configuration files in source control, so that they can be maintained and versioned along with the code for the resources they configure. But sometimes for debug and development purpose, it could be useful to directly manage a Kubernetes object deployed in a cluster in real time.

The _edit_ command allows to directly edit any API resource retrieve via the command line tools. It will open the editor defined by your KUBE \_EDITOR, or EDITOR environment variables, or fall back to 'vi' for Linux or 'notepad' for Windows. Multiple objects can edit, although changes are applied one at a time.

#### Exercise n°1

Edit the _nginx_ pods to update the image.

```bash
kubectl edit POD_NAME
```

#### Exercise n°2

Edit the _nginx_ pods with _nano_ editor.

```bash
KUBE_EDITOR="nano" kubectl edit POD_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be find easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain pods.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Some resources, such as pods, support graceful deletion. These resources define a default period before they are forcibly terminated \(the grace period\) but you may override that value with the --grace-period flag, or pass --now to set a grace-period of 1.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the busybox pod deployed previously in the default namespace.

```bash
kubectl delete pods busybox -n default
```

#### Exercise n°2

Delete the nginx pod deployed previously in the default namespace with declarative method.

```bash
kubectl delete -f FILENAME
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to deploy the Voting App Pods in his dedicated namespace.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/01_pods`

{% tabs %}
{% tab title="Exercise" %}
1. Deploy each containers of the Voting App in a single Pod called `voting-app-pod` in his dedicated namespace created in the previous module.
2. Ensure the Pods is up and running.
{% endtab %}

{% tab title="Solution" %}
Yaml file definition to deploy the Voting App containers.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: voting-app-pod
  namespace: voting-app
  labels:
    env: formation
spec:
  containers:
  - name: db
    image: postgres
  - name: redis
    image: redis
  - name: result-app
    image: result
  - name: voting-app
    image: vote
  - name: worker
    image: worker
```

Kubectl command to deploy the Pods based on the previous definition file.

```bash
kubectl create -f /data/votingapp/01_pods/pods.yaml
```

Ensure the Pods is up and running.

```bash
kubectl get pods -n voting-app
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official [cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) documentation
* [Kubectl official Reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-) documentation
* Kubernetes official documentation [pod overview](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)
* Kubernetes official documentation on [object management](https://kubernetes.io/docs/concepts/overview/object-management-kubectl/overview/)
* Kubernetes official documentation on [list pods](https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/)
* Kubernetes official documentation on [introspection and debugging](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application-introspection/#using-kubectl-describe-pod-to-fetch-details-about-pods)
* Kubernetes official documentation on [getting a shell to a running container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)
* Kubernetes official documentation on [resources management](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/)

