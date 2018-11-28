# Service

## Module Overview

At the end of the module, you will :

* _Learn how to expose a Pod internally_
* _Learn how the domain name resolution works on a Kubernetes cluster_
* _Learn how to expose a Pod externally_

## Create

What ? Why ? How ?

Explain port types :

| Type | Description |
| :--- | :--- |
| ClusterIP | Internal |
| NodePort | Host |
| LoadBalancer | External |
| Endpoint | Endpoint |

#### Exercise n°1

Expose the pod nginx in the namespace app-demo to be able to access it on HTTP in command line.

The pod has to be opened outside the cluster.

```bash
kubectl create services --type=NodePort --port=80
```

#### Exercise n°2

Expose the pod nginx in the namespace app-demo to be able to access it on HTTP and HTTPS thanks to a YAML file.

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

```text
kubectl apply -f nginx.yaml
```

## List

```bash
kubectl get services
```

## Describe

```bash
kubectl describe services SERVICE_NAME
```

## Delete

```bash
kubectl delete services SERVICE_NAME
```

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes Official documentation on [services](https://kubernetes.io/docs/concepts/services-networking/service/)
