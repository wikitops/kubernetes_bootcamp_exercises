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

#### Exercise n°1

Run a busybox pod on the default namespace.

```bash
kubectl run busybox --image=busybox -n default --restart=Never
```

## Create

In imperative object configuration, the kubectl command specifies the operation \(create, replace, etc.\), optional flags and at least one file name. The file specified must contain a full definition of the object in YAML or JSON format.

The _create_ command create a resources based on the imperative declaration \(command line or YAML file\).

#### Exercise n°1

Create a pod that contain a single nginx container with a simple YAML file.

{% hint style="info" %}
The file created in this exercise will be reused, do not delete it.
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="/data/pods/01\_pods.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-single-nginx
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```bash
kubectl create -f /data/pods/01_pods.yaml
```

#### Exercise n°2

Create a pod that contain multiple containers : nginx, redis, postgres with a single YAML file.

{% code-tabs %}
{% code-tabs-item title="/data/pods/02\_pods.yaml" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

```bash
kubectl create -f /data/pods/02_pods.yaml
```

## Apply

When using declarative object configuration, a user operates on object configuration files stored locally, however the user does not define the operations to be taken on the files. Create, update, and delete operations are automatically detected per-object by kubectl. This enables working on directories, where different operations might be needed for different objects.

The _apply_ command manage the status of resources in the Kubernetes cluster. It can create, update and delete resources based on one or more YAML files.

#### Exercise n°1

Update the image version of the previous nginx pods.

{% code-tabs %}
{% code-tabs-item title="/data/pods/03\_pods.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:4
  name: my-single-nginx14
  labels:
    env: formation
spec:
  containers:
  - name: nginx
    image: nginx:1.14
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```bash
kubectl apply -f /data/pods/03_pods.yaml
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Name : the name of the newly created resource
* Ready : the amount of container ready in the Pods
* Status : the deployment status
* Restarts : the count that the Pods has restarted from his creation
* Age : the age since the Pods creation

#### Exercise n°1

List pods deployed on the current namespace.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl get pods
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAME                READY     STATUS      RESTARTS   AGE
busybox             0/1       Completed   0          4m
my-multi-app        3/3       Running     0          3m
my-single-nginx     1/1       Running     0          3m
my-single-nginx14   1/1       Running     0          3m
```
{% endtab %}
{% endtabs %}

#### Exercise n°2

List pods deployed on the namespace _default_.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl get pods -n default
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAME                READY     STATUS      RESTARTS   AGE
busybox             0/1       Completed   0          9m
my-multi-app        3/3       Running     0          8m
my-single-nginx     1/1       Running     0          8m
my-single-nginx14   1/1       Running     1          8m
```
{% endtab %}
{% endtabs %}

#### Exercise n°3

List all pods in all namespaces.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl get pods --all-namespaces
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAMESPACE     NAME                                       READY     STATUS      RESTARTS   AGE
default       busybox                                    0/1       Completed   0          10m
default       my-multi-app                               3/3       Running     0          9m
default       my-single-nginx                            1/1       Running     0          9m
default       my-single-nginx14                          1/1       Running     1          8m
kube-system   coredns-86c58d9df4-l2hlv                   1/1       Running     0          59m
kube-system   coredns-86c58d9df4-vwf67                   1/1       Running     0          59m
kube-system   default-http-backend-5ff9d456ff-r4fk8      1/1       Running     0          59m
kube-system   etcd-minikube                              1/1       Running     0          58m
kube-system   kube-addon-manager-minikube                1/1       Running     0          58m
kube-system   kube-apiserver-minikube                    1/1       Running     0          59m
kube-system   kube-controller-manager-minikube           1/1       Running     0          58m
kube-system   kube-proxy-s5frv                           1/1       Running     0          59m
kube-system   kube-scheduler-minikube                    1/1       Running     0          58m
kube-system   metrics-server-6fc4b7bcff-wsjsq            1/1       Running     0          59m
kube-system   nginx-ingress-controller-7c66d668b-sc6g8   1/1       Running     0          59m
kube-system   storage-provisioner                        1/1       Running     0          59m

```
{% endtab %}
{% endtabs %}

#### Exercise n°4

List all pods, services, deployments in the _kube-system_ namespace.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl get pods,service,deployment -n kube-system
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAME                                       READY     STATUS    RESTARTS   AGE
coredns-86c58d9df4-l2hlv                   1/1       Running   0          1h
coredns-86c58d9df4-vwf67                   1/1       Running   0          1h
default-http-backend-5ff9d456ff-r4fk8      1/1       Running   0          1h
etcd-minikube                              1/1       Running   0          1h
kube-addon-manager-minikube                1/1       Running   0          1h
kube-apiserver-minikube                    1/1       Running   0          1h
kube-controller-manager-minikube           1/1       Running   2          1h
kube-proxy-s5frv                           1/1       Running   0          1h
kube-scheduler-minikube                    1/1       Running   2          1h
metrics-server-6fc4b7bcff-wsjsq            1/1       Running   0          1h
nginx-ingress-controller-7c66d668b-sc6g8   1/1       Running   0          1h
storage-provisioner                        1/1       Running   0          1h

NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
default-http-backend   NodePort    10.99.155.8     <none>        80:30001/TCP    1h
kube-dns               ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP   1h
metrics-server         ClusterIP   10.98.243.211   <none>        443/TCP         1h

NAME                       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
coredns                    2         2         2            2           1h
default-http-backend       1         1         1            1           1h
metrics-server             1         1         1            1           1h
nginx-ingress-controller   1         1         1            1           1h

```
{% endtab %}
{% endtabs %}

## Describe

Once an application is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the container\(s\) and Pod \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the container\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug a container deployed in a Pod.

#### Exercise n°1

Get the information of the pod busybox deployed on the default namespace.

```bash
kubectl describe po busybox
```

## Exec

Execute a command in a container is sometimes needed for debug or development purpose.

The _exec_ command manage the SSH connection to a container deployed in a Pod.

Usually, the format is : _kubectl exec -it POD\_NAME -c CONTAINER\_NAME_

#### Exercise n°1

Connect to the pod _my-single-nginx14_ in the namespace _default_.

```bash
kubectl exec -it my-single-nginx bash
```

#### Exercise n°2

Connect to the postgres container in the _my-multi-app_ pod in the namespace default_._

```bash
kubectl exec -it my-multi-app -c postgres env
```

## Edit

Sometimes it’s necessary to make narrow, non-disruptive updates to resources created. It is suggested to maintain a set of configuration files in source control, so that they can be maintained and versioned along with the code for the resources they configure. But sometimes for debug and development purpose, it could be useful to directly manage a Kubernetes object deployed in a cluster in real time.

The _edit_ command allows to directly edit any API resource retrieve via the command line tools. It will open the editor defined by your KUBE \_EDITOR, or EDITOR environment variables, or fall back to 'vi' for Linux or 'notepad' for Windows. Multiple objects can edit, although changes are applied one at a time.

#### Exercise n°1

Edit the my-single-app pods to update the image.

```bash
KUBE_EDITOR="nano" kubectl edit pods my-single-app
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
kubectl delete -f /data/pods
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to deploy the Voting App Pods in his dedicated namespace.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/01_pods`

{% tabs %}
{% tab title="Exercise" %}
1. Deploy each containers of the Voting App in a single Pod called `voting-app` in his dedicated namespace created in the previous module.
2. Ensure the Pods is up and running.
{% endtab %}

{% tab title="Solution" %}
Yaml file definition to deploy the Voting App containers.

{% code-tabs %}
{% code-tabs-item title="/data/votingapp/01\_pods/pods.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: voting-app
  namespace: voting-app
spec:
  containers:
  - name: db
    image: centos/postgresql-96-centos7
---
apiVersion: v1
kind: Pod
metadata:
  name: voting-app
  namespace: voting-app
spec:
  containers:
  - name: redis
    image: redis
---
apiVersion: v1
kind: Pod
metadata:
  name: voting-app
  namespace: voting-app
spec:
  containers:
  - name: result-app
    image: dockersamples/examplevotingapp_result:before
---
apiVersion: v1
kind: Pod
metadata:
  name: voting-app
  namespace: voting-app
spec:
  containers:
  - name: voting-app
    image: dockersamples/examplevotingapp_vote:before
---
apiVersion: v1
kind: Pod
metadata:
  name: voting-app
  namespace: voting-app
spec:
  containers:
  - name: worker
    image: dockersamples/examplevotingapp_worker
```
{% endcode-tabs-item %}
{% endcode-tabs %}

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

