---
description: >-
  A Service is an abstraction which defines a logical set of Pods and a policy
  by which to access them based on labels.
---

# Services

## Module Overview

At the end of the module, you will :

* _Learn how to expose a Pod internally_
* _Learn how the domain name resolution works on a Kubernetes cluster_
* _Learn how to expose a Pod externally_

## Create

Kubernetes Pods are mortal. They are born and when they die, they are not resurrected. ReplicaSets in particular create and destroy Pods dynamically. While each Pod gets its own IP address, even those IP addresses cannot be relied upon to be stable over time. This leads to a problem.

A Kubernetes Service is an abstraction which defines a logical set of Pods and a policy by which to access them. The set of Pods targeted by a Service is determined by a label selector.

Kubernetes manage four kinds of services depending on the exposition needed for the Pods attached :

| Type | Description |
| :--- | :--- |
| ClusterIP | Only accessed within the Kubernetes cluster |
| NodePort | Host exposition accessible from the local network |
| LoadBalancer | External exposition, Internet access |
| Endpoint | External endpoint definition to easily use it in the cluster |

The _create_ command can directly ask the API resource to create a Service in command line or create a Service object based on a yaml file definition.

#### Exercise n°1

Expose the Nginx Pod in the namespace app-demo to be able to access it from the local network on HTTP \(80\) port in command line.

```bash
kubectl create service --type=NodePort --port=80
```

#### Exercise n°2

Update the Nginx Pod in the namespace app-demo to be able to access it on HTTP \(80\) and HTTPS \(443\) port thanks to a YAML file.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
  - port: 443
    targetPort: 443
  selector:
    app: nginx
```

```bash
kubectl apply -f FILENAME
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Type : display the type of exposition defined at service creation \(see previous table\).
* Cluster-IP : display the internal cluster IP of the resources
* External-IP : display the external resources IP of the resources, field configured when LoadBalancer type is used
* Port\(s\) : display a list of ports opened in the pods managed by the service

#### Exercise n°1

Get the list of existing service in the default namespace.

```bash
kubectl get service
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Service\(s\) \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the Service\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing service in the default namespace.

```bash
kubectl describe service SERVICE_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain service.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Deleting a Service will have no effect on the Pod state. It only delete the access to each resources managed by the service.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the service Nginx deployed previously in the default namespace.

```bash
kubectl delete service SERVICE_NAME
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to expose a service depending on the security level needed.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/03_services`

{% tabs %}
{% tab title="Exercise" %}
1. Manage the access of each part of the Voting App based on those conditions :
   1. The PostgreSQL database  Deployment must only be access from the Kubernetes cluster on port 5432.
   2. The Redis queue Deployment must only be access from the Kubernetes cluster on port 6379.
   3. The vote and result Deployment must be publicly access on port 8080.
   4. The worker Pods must not be exposed.
2. Try to access the vote web site
3. Try to access the result web site
{% endtab %}

{% tab title="Solution" %}
An example of yaml definition file to manage the Pods access with Services resources.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db
  namespace: voting-app-prd
spec:
  ports:
  - name: db
    port: 5432
    protocol: TCP
    targetPort: 5432
  selector:
    name: db
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: redis
  namespace: voting-app-prd
spec:
  ports:
    - name: redis
      port: 6379
      protocol: TCP
      targetPort: 6379
  selector:
    app: redis
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: result
  namespace: voting-app-prd
spec:
  ports:
  - name: 8080-tcp
    nodePort: 31001
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    name: result
  type: NodePort
---
apiVersion: v1
kind: Service
metadata:
  name: vote
  namespace: voting-app-prd
spec:
  ports:
  - name: 8080-tcp
    nodePort: 31000
    port: 8080
    targetPort: 8080
  selector:
    name: vote
  type: NodePort
```

Create the Services resources based on the previous yaml file definition.

```bash
kubectl create -f /data/votingapp/04_services/services.yaml
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [services](https://kubernetes.io/docs/concepts/services-networking/service/)

