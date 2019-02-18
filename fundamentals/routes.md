---
description: >-
  Kubernetes have advanced networking capabilities that allow Pods and Services
  to communicate inside and outside the cluster's network.
---

# Routes

## Module Overview

At the end of this module, you will :

* _Learn to manage the external access of internal resources_
* _Learn to manage ingress controller_
* _Learn to secure the cluster access_

{% hint style="info" %}
This module needs an Ingress controller to be deployed on the cluster. The default Ingress controller used in this module in Nginx. Ensure this module is up and running before continuing.
{% endhint %}

## Create

Ingress exposes HTTP and HTTPS routes from outside the cluster to Services within the cluster. Traffic routing is controlled by rules defined on the ingress resource.

An ingress can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL, and offer name based virtual hosting. An ingress controller is responsible for fulfilling the ingress, usually with a loadbalancer, though it may also configure your edge router or additional frontends to help handle the traffic.

In order for the ingress resource to work, the cluster must have an ingress controller running :

| Name | Description |
| :--- | :--- |
| Contour | An Envoy based ingress controller provided and supported by Heptio. |
| F5 Networks | Provides support and maintenance for the F5 BIG-IP Controller for Kubernetes. |
| HAproxy | Offers support and maintenance for HAproxy Enterprise and the ingress controller jcmoraisjr/haproxy-ingress. |
| Istio | Based ingress controller Control Ingress Traffic. |
| Kong | Offers community or commercial support and maintenance for the Kong Ingress Controller for Kubernetes. |
| Nginx | Offers support and maintenance for the NGINX Ingress Controller for Kubernetes. |
| Traefik | Fully featured ingress controller \(Let’s Encrypt, secrets, http2, websocket\), and it also comes with commercial support by Containous. |

The _create_ command can create a Ingress object based on a yaml file definition.

#### Exercise n°1

{% hint style="info" %}
On Minikube, ensure that the ingress addons is enable before continuing :

`minikube addons enable ingress`
{% endhint %}

First, deploy two static website in two different deployments. Then, expose each one on the port 80.

{% code-tabs %}
{% code-tabs-item title="/data/routes/01\_deployments.yaml" %}
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver1
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webserver1
  template:
    metadata:
      labels:
        app: webserver1
    spec:
      containers:
      - name: static
        image: dockersamples/static-site
        env:
        - name: AUTHOR
          value: wikitops1
        ports:
        - containerPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver2
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webserver2
  template:
    metadata:
      labels:
        app: webserver2
    spec:
      containers:
      - name: static
        image: dockersamples/static-site
        env:
        - name: AUTHOR
          value: wikitops2
        ports:
        - containerPort: 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Expose each on of the Deployment on port 80.

{% code-tabs %}
{% code-tabs-item title="/data/routes/02\_services.yaml" %}
```yaml
apiVersion: v1
kind: Service
metadata:
  name: webserver1
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: webserver1
---
apiVersion: v1
kind: Service
metadata:
  name: webserver2
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: webserver2
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Create an Ingress resource to expose an Nginx pod Service's on port 80.

{% code-tabs %}
{% code-tabs-item title="/data/routes/03\_ingress.yaml" %}
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
  name: myfirstingress
spec:
  rules:
  - host: training.wikitops.io
    http:
      paths:
      - backend:
          serviceName: webserver1
          servicePort: 80
        path: /path1
      - backend:
          serviceName: webserver2
          servicePort: 80
        path: /path2
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Create each resources based on the previous yaml files definiton.

```bash
kubectl create -f /data/routes/01_deployments.yaml -f /data/routes/02_services.yaml -f /data/routes/03_ingress.yaml
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Name : the name of the newly created resource
* Hosts : the host to apply the newly created resource
* Address : the address exposed by the newly created resource
* Ports : the ports exposed by the resource
* Age : the age since his creation

#### Exercise n°1

List the current Ingress resources created.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl get ingress
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAME             HOSTS                  ADDRESS     PORTS     AGE
myfirstingress   training.wikitops.io   10.0.2.15   80        5m
```
{% endtab %}
{% endtabs %}

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Ingress \(labels, annotations, events, backend associated, IP address associated, etc.\) and the rules available for each ingress \(hosts, path, backend associated\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the existing Ingress in the default namespace.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl describe ingress myfirstingress
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
Name:             myfirstingress
Namespace:        default
Address:          10.0.2.15
Default backend:  default-http-backend:80 (172.17.0.4:8080)
Rules:
  Host                  Path  Backends
  ----                  ----  --------
  training.wikitops.io  
                        /path1   webserver1:80 (<none>)
                        /path2   webserver2:80 (<none>)
Annotations:
  kubectl.kubernetes.io/last-applied-configuration:  {"apiVersion":"extensions/v1beta1","kind":"Ingress","metadata":{"annotations":{"nginx.ingress.kubernetes.io/rewrite-target":"/"},"name":"myfirstingress","namespace":"default"},"spec":{"rules":[{"host":"training.wikitops.io","http":{"paths":[{"backend":{"serviceName":"webserver1","servicePort":80},"path":"/path1"},{"backend":{"serviceName":"webserver2","servicePort":80},"path":"/path2"}]}}]}}

  nginx.ingress.kubernetes.io/rewrite-target:  /
Events:
  Type    Reason  Age              From                      Message
  ----    ------  ----             ----                      -------
  Normal  CREATE  3m               nginx-ingress-controller  Ingress default/myfirstingress
  Normal  UPDATE  2m (x2 over 3m)  nginx-ingress-controller  Ingress default/myfirstingress
```
{% endtab %}
{% endtabs %}

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

{% tabs %}
{% tab title="Command" %}
```bash
kubectl explain ingresses.spec
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
KIND:     Ingress
VERSION:  extensions/v1beta1

RESOURCE: spec <Object>

DESCRIPTION:
     Spec is the desired state of the Ingress. More info:
     https://git.k8s.io/community/contributors/devel/api-conventions.md#spec-and-status

     IngressSpec describes the Ingress the user wishes to exist.

FIELDS:
   backend	<Object>
     A default backend capable of servicing requests that don't match any rule.
     At least one of 'backend' or 'rules' must be specified. This field is
     optional to allow the loadbalancer controller or defaulting logic to
     specify a global default.

   rules	<[]Object>
     A list of host rules used to configure the Ingress. If unspecified, or no
     rule matches, all traffic is sent to the default backend.

   tls	<[]Object>
     TLS configuration. Currently the Ingress only supports a single TLS port,
     443. If multiple members of this list specify different hosts, they will be
     multiplexed on the same port according to the hostname specified through
     the SNI TLS extension, if the ingress controller fulfilling the ingress
     supports SNI.
```
{% endtab %}
{% endtabs %}

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Be careful on the deletion of a Ingress object. This will interupt existing communication based on this resource.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous ingress in command line.

```bash
# Delete Ingress
kubectl delete ingress myfirstingress

# Delete Deployments
kubectl delete deployment webserver1 webserver2

# Delete Services
kubectl delete service webserver1 webserver2
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to manage and secure the external access of Kubernetes resources.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/12_routes`

{% tabs %}
{% tab title="Exercise" %}
1. Expose the vote Services  as an Ingress controller.
2. Expose the result Services as an Ingress controller.
3. Ensure that you accessed both services externally.
{% endtab %}

{% tab title="Solution" %}
Create the yaml file definition to expose the vote service.

{% code-tabs %}
{% code-tabs-item title="/data/votingapp/11\_routes/routes.yaml" %}
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: vote
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: vote
          servicePort: 80
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: vote
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: result
          servicePort: 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Create the yaml file definition to expose the result service.

```bash
kubectl create -f /data/votingapp/11_routes/routes.yaml
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress/)

