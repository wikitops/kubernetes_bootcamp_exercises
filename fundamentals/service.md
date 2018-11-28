---
description: >-
  A Kubernetes Service is an abstraction which defines a logical set of Pods and
  a policy by which to access them.
---

# Service

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

#### Exercise n°1

Expose the Nginx Pod in the namespace app-demo to be able to access it from the local network on HTTP \(80\) port in command line.

```bash
kubectl create services --type=NodePort --port=80
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

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formated to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Type : display the type of exposition defined at service creation \(see previous table\).
* Cluster-IP : display the internal cluster IP of the resources
* External-IP : display the externale resources IP of the resources, field configured when LoadBalancer type is used
* Port\(s\) : display a list of ports opened in the pods managed by the service

```bash
kubectl get service
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Service\(s\) \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the Service\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

```bash
kubectl describe service SERVICE_NAME
```

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Deleting a Service will have no effect on the Pod state. It only delete the access to each resources managed by the service. 

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

```bash
kubectl delete service SERVICE_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [services](https://kubernetes.io/docs/concepts/services-networking/service/)

